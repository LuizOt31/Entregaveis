# Objetivo
Estabelecer um túnel direto entre o load balancer (IPVS) e uma VM. 


## Justificativa
Esse método é significativamente mais eficiente para tráfego vindo de fora, pois elimina a necessidade de NAT no load balancer. Em protocolos como HTTP, onde as respostas costumam ser muito maiores que as requisições, evitar o NAT reduz o tráfego de retorno pelo load balancer, economizando recursos e melhorando o desempenho.