# 🐳 *Docker Seminar – Projeto Hogwarts*  
> Essa atividade tem como principal objetivo **sedimentar os conceitos básicos sobre Docker** e colocá-los em prática com implementação de serviços e testes usando containers Docker.  

---

## 📖 Introdução

Neste repositório, criamos uma infraestrutura containerizada composta por:

1. Um **servidor DNS** (BIND9) com zona primária `hogwarts.local`
2. Um **proxy reverso HTTP/HTTPS** (Nginx) com certificado SSL autoassinado
3. Três **servidores web** (Nginx) com páginas personalizadas para as casas de Hogwarts:
   - Grifinória (`grifinoria.db.hogwarts.local`)
   - Sonserina (`sonserina.db.hogwarts.local`)
   - Lufa-Lufa (`lufalufa.db.hogwarts.local`)

O ambiente foi orquestrado usando **Docker Compose**, permitindo rodar todos os serviços de forma isolada, porém intercomunicável, em uma rede Docker interna.  

---

## 🎯 Objetivo da Atividade

> Essa atividade tem como principal objetivo colocar e **sedimentar os conceitos básicos sobre Docker** e colocá-los em prática com implementação de serviços e testes usando containers Docker.

- Explorar a criação de **imagens customizadas** (BIND9, Nginx).
- Configurar e testar um **servidor DNS autoritativo**.
- Estabelecer um **proxy reverso HTTPS** com certificado SSL.
- Deploy de **aplicações web estáticas** isoladas em containers.
- Demonstrar a resolução de nomes local via **arquivo hosts** do sistema operacional.

---

## 🗂 Estrutura do Repositório

```plaintext
.
├── README.md
├── docker-compose.yml
├── dns/
│   ├── Dockerfile
│   ├── named.conf.options
│   ├── named.conf.local
│   └── zones/
│       └── db.hogwarts.local
├── proxy/
│   ├── Dockerfile
│   ├── nginx.conf
│   └── certs/
│       ├── cert.pem
│       └── key.pem
└── web/
    ├── grifinoria/
    │   ├── Dockerfile
    │   ├── index.html
    │   ├── style.css
    │   └── imagens/
    │       └── grifinoria.webp
    ├── sonserina/
    │   ├── Dockerfile
    │   ├── index.html
    │   ├── style.css
    │   └── imagens/
    │       └── sonserina.webp
    └── lufalufa/
        ├── Dockerfile
        ├── index.html
        ├── style.css
        └── imagens/
            └── lufalufa.webp
```

Cada pasta e arquivo tem um propósito específico:

# 1. *docker-compose.yml*
 - Orquestra todos os serviços (DNS, proxy e web servers) em uma rede Docker interna (hogwarts_net). Define IPs estáticos, volumes e mapeamento de portas.

# 2. *dns/*

 - Dockerfile: cria a imagem Ubuntu com BIND9 instalado.

 - named.conf.options: configurações globais (forwarders, opções de recursão e listas de controle).

 - named.conf.local: define a zona hogwarts.local como master.

 - zones/db.hogwarts.local: arquivo de zona DNS contendo registros SOA, NS e A para cada subdomínio (grifinoria, sonserina, lufalufa e proxy).

# *proxy/*

 - Dockerfile: cria a imagem Nginx (Alpine) e copia o certificado SSL autoassinado + configuração nginx.conf.

 - nginx.conf: define blocos server separados para cada subdomínio em HTTP (redireciona para HTTPS) e em HTTPS (proxy_pass para os containers das casas).

 - certs/cert.pem, certs/key.pem: certificado e chave privada gerados (autoassinados) para hogwarts.local. Atuam no TLS do proxy.

# *web/*
Pasta contendo 3 subdiretórios, um para cada casa de Hogwarts.

 - grifinoria/, sonserina/, lufalufa/

 - Dockerfile: base Nginx:alpine e cópia de todos os arquivos locais para /usr/share/nginx/html.

 - index.html: página HTML personalizada para a casa.

 - style.css: CSS definindo fundo, sobreposição e formatação de texto.

 - imagens/: diretório contendo arquivo .webp de fundo.

# 🚀 Como Funciona na Prática
## 1. Rede Interna Docker
Todos os serviços estão na mesma rede Docker chamada hogwarts_net (subnet 10.5.0.0/16). Cada container recebe um IP fixo:

 - dns: 10.5.0.10

 - grifinoria: 10.5.0.11

 - sonserina: 10.5.0.12

 - lufalufa: 10.5.0.13

 - proxy: 10.5.0.20

Isso garante que cada container possa se comunicar pelo nome de serviço (ex.: grifinoria) ou pelo IP interno.

## 2. Servidor DNS (BIND9)

 - Atua como autoridade para a zona hogwarts.local.

 - No arquivo zones/db.hogwarts.local, cada A record aponta para o IP do container correspondente.

 - Apesar de expor a porta 53, esse DNS interno não precisa ser usado diretamente pelo host. Ele existe para demonstrar a config de BIND9 em Docker.

## 3. Proxy Reverso (Nginx + SSL)

 - O container proxy está mapeado para a porta 80 e 443 do host (0.0.0.0).

 - Quando o navegador chega em https://grifinoria.db.hogwarts.local (resolvido pelo hosts para 192.168.1.63 => porta 443), o Nginx recebe a requisição e procura um bloco server cujo server_name seja “grifinoria.db.hogwarts.local”.

 - Dentro desse bloco, location / { proxy_pass http://grifinoria:80; } redireciona o tráfego para o container “grifinoria”.

 - O mesmo acontece para Sonserina e Lufa-Lufa, cada um em seu próprio bloco server.

## 4. Certificado Autoassinado (SSL/TLS)

 - Ao buildar o proxy, foram gerados cert.pem e key.pem dentro do container (em /etc/nginx/certs/).

 - O Nginx usa esse par para abrir a porta 443 em TLS. Como o certificado não é de uma CA confiável, o navegador mostrará um aviso.

 - Para propósitos de demonstração, basta aceitar o risco no browser.

## 5. Servidores Web (Nginx)

 - Cada pasta em web/<casa>/ contém um Dockerfile mínimo baseado em nginx:alpine.

 - Os arquivos HTML, CSS e imagens são copiados para /usr/share/nginx/html.

 - Ao receber uma requisição via proxy, cada container entrega sua página estática personalizada.


