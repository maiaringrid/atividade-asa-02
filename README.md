🐳 *Docker Seminar – Projeto Hogwarts*  
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

... (continua no arquivo completo)
