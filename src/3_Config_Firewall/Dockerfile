#imagem base Ubuntu
FROM ubuntu:latest

#Atualiza o repositório e instalando o firewall iptables
RUN apt-get update && apt-get install -y iptables

#copia o script de configuração do firewall para dentro do container
COPY firewall.sh /root/firewall.sh

#script de configuração do firewall como executável
RUN chmod +x /root/firewall.sh

#comando para executar o script de configuração do firewall
CMD ["/bin/bash", "/root/firewall.sh"]
