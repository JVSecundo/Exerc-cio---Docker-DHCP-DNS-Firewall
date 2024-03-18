# Exercicio_Docker_DHCP_DNS_Firewall

Documentação de Configuração de Serviços de Rede com Docker
Nesta documentação, abordaremos a configuração de três serviços de rede essenciais: DHCP (Dynamic Host Configuration Protocol), DNS (Domain Name System) e Firewall. Cada serviço será configurado em um contêiner Docker separado para facilitar a implantação e o gerenciamento.

# 1. Configuração do Servidor DHCP
Arquivo de Configuração (dhcpd.conf)
dhcpd.conf
Copy code
subnet 172.17.0.0 netmask 255.255.0.0 {
  range 172.17.0.10 172.17.0.100; 
  option routers 172.17.0.1;      
  option broadcast-address 172.17.255.255;
  default-lease-time 600;         
  max-lease-time 7200;            
}

## Dockerfile

- Dockerfile
Copy code
FROM ubuntu:latest

RUN mkdir -p /var/lib/dhcp/
RUN touch /var/lib/dhcp/dhcpd.leases

RUN apt update && apt install -y isc-dhcp-server

COPY dhcpd.conf /etc/dhcp/dhcpd.conf

EXPOSE 67/udp

ENTRYPOINT ["dhcpd", "-f", "-d", "--no-pid"]
CMD ["tail","-f","/dev/null"]

# 2. Configuração do Servidor DNS (Bind9)
Arquivo de Configuração (named.conf.options)
named.conf.options
Copy code
acl goodclients {
    192.168.0.0/16;
    localhost;
    localnets;
};

options {
    directory "/var/cache/bind";
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };
    allow-query { 
        any; 
    };
    recursion yes;
    dnssec-validation auto;
    listen-on-v6 { any; };
}

## Dockerfile

- Dockerfile
Copy code
FROM ubuntu:latest

RUN apt-get update && apt-get install -y bind9

COPY named.conf.options /etc/bind/named.conf.options

EXPOSE 53/udp
EXPOSE 53/tcp

ENTRYPOINT ["/usr/sbin/named", "-g", "-c", "/etc/bind/named.conf", "-u", "bind"]
CMD ["tail","-f","/dev/null"]

# 3. Configuração do Firewall
Script de Firewall (firewall.sh)
firewall.sh
Copy code
#Limpando todas as regras existentes
iptables -F
iptables -X

#definindo a política padrão como DROP (bloquear tudo)
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

#permitindo conexões de loopback
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

#Permitindo tráfego relacionado e estabelecido
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

#permitindo tráfego DHCP
iptables -A INPUT -p udp --dport 67:68 --sport 67:68 -j ACCEPT

#permitindo tráfego DNS
iptables -A INPUT -p udp --dport 53 -j ACCEPT
iptables -A INPUT -p tcp --dport 53 -j ACCEPT

#bloqueado o trem 
iptables -A INPUT -s 200.18.168.100 -p icmp --icmp-type echo-request -j DROP

## Dockerfile

- Dockerfile

Copy code
FROM ubuntu:latest

RUN apt-get update && apt-get install -y iptables

COPY firewall.sh /root/firewall.sh

RUN chmod 777 /root/firewall.sh

CMD ["/root/firewall.sh"]

# Implantação e Testes

## Iniciar os Containers Docker:

- Para cada serviço, execute o comando docker build -t <nome_imagem> . dentro do diretório contendo o Dockerfile correspondente.

- Em seguida, execute os contêineres Docker usando docker run -d --name <nome_contêiner> <nome_imagem>.

## Testar a Atribuição de Endereços IP pelo Servidor DHCP:

- Configure uma máquina cliente para obter endereços IP automaticamente.

- Verifique se a máquina cliente recebe um endereço IP dentro da faixa especificada no dhcpd.conf.

## Testar a Resolução de Nomes de Domínio pelo Servidor DNS:

- Configure a máquina cliente para usar o servidor DNS configurado.

- Teste a resolução de nomes de domínio usando comandos como nslookup ou dig.
 
## Testar a Conectividade entre Dispositivos na Rede Considerando as Regras do Firewall:

- Tente estabelecer conexões entre dispositivos na rede e verifique se as regras do firewall estão permitindo ou bloqueando o tráfego conforme esperado.

# Considerações Finais
Este documento fornece uma visão geral da configuração dos serviços de rede DHCP, DNS e Firewall usando contêineres Docker. Certifique-se de entender as configurações e realizar testes adequados para garantir o funcionamento correto dos serviços.