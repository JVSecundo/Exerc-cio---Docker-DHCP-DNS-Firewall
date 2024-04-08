# Usando a imagem base Ubuntu
FROM ubuntu:latest

# Atualiza o repositório e instalando o servidor DHCP
RUN apt-get update && apt-get install -y isc-dhcp-server

#Copia o arquivo de configuração do DHCP para dentro do container
COPY dhcpd.conf /etc/dhcp/dhcpd.conf

# Expondo a porta usada pelo servidor DHCP
EXPOSE 67/udp

#Comando para iniciar o servidor DHCP
CMD ["dhcpd", "-4", "-f", "-d", "--no-pid", "-cf", "/etc/dhcp/dhcpd.conf", "eth0"]
