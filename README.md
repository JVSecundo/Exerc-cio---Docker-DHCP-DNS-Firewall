# Exercicio_Docker_DHCP_DNS_Firewall

Documentação de Configuração de Serviços de Rede com Docker.
Nesta documentação, abordaremos a configuração de três serviços de rede essenciais: DHCP (Dynamic Host Configuration Protocol), DNS (Domain Name System) e Firewall. Cada serviço será configurado em um contêiner Docker separado para facilitar a implantação e o gerenciamento.

# Implantação e Testes

## Iniciar os Containers Docker:

- Para cada serviço, execute o comando docker build -t <nome_imagem> . dentro do diretório contendo o Dockerfile correspondente.

- Em seguida, execute os contêineres Docker usando docker run -d --name <nome_contêiner> <nome_imagem>.

## Testar a Atribuição de Endereços IP pelo Servidor DHCP:


## Testar a Resolução de Nomes de Domínio pelo Servidor DNS:


 
## Testar a Conectividade entre Dispositivos na Rede Considerando as Regras do Firewall:


# Considerações Finais
Este documento fornece uma visão geral da configuração dos serviços de rede DHCP, DNS e Firewall usando contêineres Docker. Certifique-se de entender as configurações e realizar testes adequados para garantir o funcionamento correto dos serviços.