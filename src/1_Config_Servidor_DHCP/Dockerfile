#define a imagem base a ser usada
FROM ubuntu:latest

#cria o diretório para armazenar os dados do servidor DHCP
RUN mkdir -p /var/lib/dhcp/

#Cria um arquivo vazio de arrendamentos DHCP
RUN touch /var/lib/dhcp/dhcpd.leases

#Atualiza os repositórios de pacotes e instala o servidor DHCP
RUN apt update && apt install -y isc-dhcp-server

#copia o arquivo de configuração do servidor DHCP para dentro do contêiner
COPY dhcpd.conf /etc/dhcp/dhcpd.conf

#expondo a porta UDP 67 para aceitar conexões DHCP
EXPOSE 67/udp

#define o ponto de entrada padrão para o contêiner
ENTRYPOINT ["dhcpd", "-f", "-d", "--no-pid"]

#Define o comando padrão a ser executado quando o contêiner é iniciado
CMD ["tail","-f","/dev/null"]
