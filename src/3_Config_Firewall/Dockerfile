#define a imagem base a ser usada
FROM ubuntu:latest

#atualiza os repositórios de pacotes e instala o iptables
RUN apt-get update && apt-get install -y iptables

#Copia o script firewall.sh para dentro do contêiner
COPY firewall.sh /root/firewall.sh

#concede permissões de execução ao script
RUN chmod 777 /root/firewall.sh

#define o comando padrão a ser executado quando o contêiner é iniciado
CMD ["/root/firewall.sh"]
