#define a imagem base a ser usada
FROM ubuntu:latest

#Atualiza os repositórios de pacotes e instala o servidor DNS Bind9
RUN apt-get update && apt-get install -y bind9

#copia o arquivo de configuração named.conf.options para dentro do contêiner
COPY named.conf.options /etc/bind/named.conf.options

#expondo a porta UDP 53 para aceitar consultas DNS
EXPOSE 53/udp

#expondo a porta TCP 53 para aceitar consultas DNS
EXPOSE 53/tcp

#Define o ponto de entrada padrão para o contêiner
ENTRYPOINT ["/usr/sbin/named", "-g", "-c", "/etc/bind/named.conf", "-u", "bind"]

#define o comando padrão a ser executado quando o contêiner é iniciado
CMD ["tail","-f","/dev/null"]
