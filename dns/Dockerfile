FROM ubuntu:22.04

# Instala o BIND9
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y bind9 dnsutils && \
    rm -rf /var/lib/apt/lists/*

# Copia arquivos de configuração
COPY named.conf.options /etc/bind/named.conf.options
COPY named.conf.local   /etc/bind/named.conf.local

# Ajusta permissões e expõe portas
RUN chown -R bind:bind /etc/bind/*
EXPOSE 53/tcp 53/udp

CMD ["named", "-g", "-c", "/etc/bind/named.conf"]
