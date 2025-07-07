# Teste de Balanceamento de Carga com IPVS em Servidores HTTP

## Objetivo
Verificar o funcionamento do balanceador de carga IPVS (IP Virtual Server) no host *Curau*, configurado para distribuir conexões HTTP entre três servidores backend utilizando diferentes algoritmos de balanceamento de carga, como Round-Robin, Weighted Round-Robin e Least-Connections. O teste avalia a capacidade do IPVS, gerenciado pelo Keepalived, de rotacionar conexões de forma eficiente, garantindo alta disponibilidade e distribuição equitativa ou ponderada do tráfego.

## Setup do Experimento

### Infraestrutura
- **Host Curau**: Servidor bare metal configurado como balanceador de carga.
  - Sistema operacional: Linux (distribuição Ubuntu 22.04 LTS recomendada para suporte a IPVS e Keepalived).
  - Software:
    - **IPVS**: Utilizado para balanceamento de carga no nível 4 (camada de transporte).
    - **Keepalived**: Gerencia o IPVS e fornece alta disponibilidade através do protocolo VRRP (Virtual Router Redundancy Protocol).
    - IP virtual configurado para receber tráfego HTTP (porta 8001).
- **Servidores Backend**: Três servidores bare metal, cada um executando um servidor HTTP Python simples para responder a requisições HTTPS.
  - **Nó 1**: IP `192.168.35.1`, 
  - **Nó 2**: IP `192.168.35.2`,
  - **Nó 3**: IP `192.168.35.3`,
  - Sistema operacional: Linux (Ubuntu 24.04 LTS).
  - Software: Servidor HTTP Python (usando `http.server`).
  - Porta: 8001 (HTTP).

### Configuração do Ambiente
1. **Instalação e Configuração do IPVS no Curau**:
   - Instalar pacotes necessários: `sudo apt install ipvsadm keepalived`.
   - Configurar o Keepalived para gerenciar o IP virtual e o IPVS:
     - Arquivo de configuração: `/etc/keepalived/keepalived.conf`.
     - Exemplo de configuração para Round-Robin:
       ```plaintext
       vrrp_instance VI_1 {
           state MASTER
           interface enp3s0
           virtual_router_id 51
           priority 100
           advert_int 1
           virtual_ipaddress {
               192.168.1.100
           }
       }
       virtual_server 192.168.1.100 8001 {
           delay_loop 6
           lb_algo rr
           lb_kind NAT
           protocol TCP
           real_server 192.168.35.1 8001 {
               weight 1
               HTTP_GET {
                url {
                    path /
                }
           }
           real_server 192.168.35.2 8001 {
               weight 1
               HTTP_GET {
                url {
                    path /
                }
           }
           real_server 192.168.35.3 8001 {
               weight 1
               HTTP_GET {
                url {
                    path /
                }
           }
       }

## Experimentos Realizados
1. **Verificação do Funcionamento do Balanceador de Carga**:
   - Testado o balanceamento de conexões HTTP no host *Curau* com algoritmos Round-Robin, Weighted Round-Robin e Least-Connections.
   - Confirmada a correta operação do IPVS com Keepalived, mantendo o IP virtual (`192.168.35.100`) acessível e respostas consistentes dos servidores backend.

2. **Teste de Failover com Derrubada de Hosts**:
   - Simulada a falha de um servidor backend (desativando a porta 8001 ou desligando o nó) para avaliar o comportamento do IPVS.
   - Verificado o tempo de reconvergência e a redistribuição automática de tráfego para os nós ativos, sem interrupção no acesso ao IP virtual.
   - Monitorado o comportamento do Keepalived via VRRP para garantir continuidade do serviço.

## O Que Mais Pode Ser Feito
- **Teste de Carga Extrema**: Aumentar o volume de requisições (e.g., 10.000 requisições com alta concorrência) para avaliar limites de desempenho do IPVS e dos servidores backend.
- **Teste de Alta Disponibilidade**: Configurar um segundo host *Curau* em modo BACKUP no Keepalived para testar failover do balanceador em caso de falha do nó primário.
- **Análise de Latência**: Medir o impacto de cada algoritmo no tempo de resposta sob diferentes condições de carga e latência artificial nos servidores.
- **Segurança HTTPS**: Testar com certificados SSL/TLS de uma CA confiável e avaliar o impacto de verificações rigorosas no desempenho.
- **Monitoramento Avançado**: Integrar ferramentas como Prometheus e Grafana para visualização em tempo real das métricas do IPVS e dos servidores backend.
