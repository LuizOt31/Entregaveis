## Objetivo
Entender como o IPVS se comporta ao mover uma VM de um compute node para outro. Quais são suas ações para conseguir encontrar de novo a instância.

### Justificativa
Julgamos este teste pertinente, pois em ambientes Cloud, caso um *compute node* tenha problemas, a vm precisa ser migrada para outro computador. E para que o serviço continue de pé precisamos saber se o IPVS (nosso load balancer) consegue se sair bem com essas trocas

#### Metodologia
**Primeiro passo**:
* Criar servidor http em uma VM no pamonha2
* Criar rota no keepalive para fazer dnat entre IP publico e Floating IP pra quem fizer curl de fora
* Abrir tcpdump na interface da rede interna (10.10.35.0/24) para ver os pacotes
* Fazer *curl* de um computador de fora

**Segundo passo**:
* Migrar a VM para o pamonha1
* Abrir tcpdump na interface do pamonha1 e do curau para ver os pacotes
* Fazer *curl* de um computador de fora
	* Precisa ser em um intervalo de 10 minutos do primeiro passo pra esse (intervalo do ARP)
##### Scripts keepalive.conf
```sh

```
##### Scripts tcpdump
```sh

```

---
### Primeiro passo
