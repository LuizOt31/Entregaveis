# Experimentos

Este arquivo tem como finalidade descrever testes para entender o funcionamento de load balancers de camada 4 e camada 7, preferencialmente IPVS (camada 4) com Keepalived e Envoy (camada 7). A ideia é estender para outros load balancers ao longo do tempo.

## IPVS + Keepalived

### 1. Teste simples com três servidores HTTPS (nos pamonhas)

- Objetivo: Verificar se o IPVS (no host *Curau*) consegue rotacionar conexões entre três servidores HTTPS utilizando diferentes algoritmos de balanceamento (como round-robin, weighted round robin, least-connections etc).
- Setup: 
    - Curau com Ipvs (gerenciado pelo keepalived) em Bare Metal.
    - Três servidores http python rodando em cada nó (bare metal)

### 2. Teste com servidores HTTP em VMs em rede OVN com Floating IP

- Objetivo: Testar se o IPVS funciona corretamente com VMs em rede OVN utilizando Floating IPs.

### 3. Teste de migração de VM entre compute nodes

- Objetivo: Entender como o IPVS se comporta ao mover uma VM de um compute node para outro.
- Justificativa: O IPVS localiza a VM por algum mecanismo, possivelmente via ARP, e o OVN responde com a localização correta, estabelecendo a rota. Queremos verificar se essa rota é atualizada automaticamente após a migração da VM.
- Procedimento:
  - Migrar a VM entre nós de computação.
  - Capturar pacotes com `tcpdump` tanto no host *Curau* quanto nos nós onde a VM estiver hospedada.
  - Observar como o tráfego e as rotas se comportam após a migração.

---

Outros testes e observações poderão ser adicionados conforme a evolução dos experimentos.
