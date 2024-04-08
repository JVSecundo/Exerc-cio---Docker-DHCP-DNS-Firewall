# Usando a imagem base Ubuntu
FROM ubuntu:latest

#atualiza o repositório e instalando o servidor DNS BIND9
RUN apt-get update && apt-get install -y bind9

#Copiao arquivo de configuração do DNS para dentro do container
COPY named.conf.options /etc/bind/named.conf.options

#Expondo a porta usada pelo servidor DNS
EXPOSE 53/tcp
EXPOSE 53/udp

#Definindo o diretório de trabalho
WORKDIR /etc/bind

# Comando para iniciar o servidor DNS
CMD ["/usr/sbin/named", "-g", "-c", "named.conf", "-u", "bind"]
