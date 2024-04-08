# Exercicio_Docker_DHCP_DNS_Firewall

Documentação de Configuração de Serviços de Rede com Docker.
Nesta documentação, abordaremos a configuração de três serviços de rede essenciais: DHCP (Dynamic Host Configuration Protocol), DNS (Domain Name System) e Firewall. Cada serviço será configurado em um contêiner Docker separado para facilitar a implantação e o gerenciamento.

# Implantação e Testes

## Iniciar os Containers Docker:

- Para cada serviço, execute o comando **"docker build -t <nome_imagem> ."** dentro do diretório contendo o Dockerfile correspondente.

- Em seguida, execute os contêineres Docker usando **"docker run -d --name <nome_contêiner> <nome_imagem>."**

## Testar a Atribuição de Endereços IP pelo Servidor DHCP:

- Para testar se o servidor DHCP está atribuindo endereços IP, execute um cliente DHCP e verifica se ele recebe um endereço IP, usar o utilitário dhclient em sistemas Linux, **"sudo dhclient -v <interface>"**.

- Trocar o nome **"<interface>"** pelo nome da sua interface de rede.

- Solicitação DHCP à rede e o cliente DHCP deverá responder atribuindo um endereço IP ao seu dispositivo, **"sudo dhclient -v eth0 "**.

- Verifica os logs do servidor DHCP para garantir que não haja erros, **"docker logs dhcp-container"**.

## Testar a Resolução de Nomes de Domínio pelo Servidor DNS:
- Construir a Imagem Docker, **"docker build -t dns-server ."**.

- Iniciar o Container DNS, **"docker run -d --name dns-container -p 53:53/tcp -p 53:53/udp dns-server"**, este comando iniciará o container DNS em modo detached (-d) com o nome dns-container. As opções -p mapeiam a porta 53 do host para a porta 53 do container, permitindo a comunicação com o servidor DNS.

- Após iniciar o container DNS, você pode testar sua configuração executando consultas DNS usando ferramentas como dig, comando necessario **"dig example.com @localhost"**, substitua **"example.com"** pelo domínio que você deseja consultar, e substitua **"localhost"** pelo endereço IP do seu servidor DNS se você não estiver executando as consultas no próprio host.

- Verifica os logs do servidor DNS para garantir que não haja erros, **"docker logs dns-container"**.

 
## Testar a Conectividade entre Dispositivos na Rede Considerando as Regras do Firewall:

- Construir a Imagem Docker, **"docker build -t firewall ."**.

- Iniciar o Container Firewall, **"docker run -d --name firewall-container --cap-add=NET_ADMIN firewall"**, comando para iniciar o container Firewall em modo detached (-d) com o nome firewall-container, e está opção **"--cap-add=NET_ADMIN"** é necessária para conceder ao container as permissões necessárias para configurar o firewall.

- Testar a configuração, **"docker exec -it firewall-container iptables -L"**, isso exibirá as regras atualmente configuradas no firewall.

- Testar a conectividade usando **"ping, telnet ou nc"**, para verificar se o tráfego está sendo permitido ou bloqueado conforme esperado.

# Considerações Finais
Este documento fornece uma visão geral da configuração dos serviços de rede DHCP, DNS e Firewall usando contêineres Docker. Certifique-se de entender as configurações e realizar testes adequados para garantir o funcionamento correto dos serviços.
