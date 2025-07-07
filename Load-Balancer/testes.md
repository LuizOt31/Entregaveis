# Testes

Esse arquivo tem como finalidade pensar em alguns testes para entender load balancers layer 4 e 7, preferencialmente IPVS (layer 4) e envoy (layer 7), mas queremos testar outros ao longo do tempo.

## IPVS Keepalive

1) Testar abrir nos pamonhas dois servers https e ver se o o IPVS no Curau consegue rotacionar com vários algoritmos (rr, etc.) nos server

2) Testar Abrir server http em vms em rede OVN com lfoating ip e ver se da certo

3) Testar abrir server http e trocar a vm de lugar (trocando de compute node)

- Justificativa: Para achar a VM
