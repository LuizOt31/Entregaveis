Caso seu setup do Open Virtual Network tenha dado certo em todos os passos, mas quando tenta criar uma rede ela nunca é inicializada (por sinal, se der `Ctrl+C` e fizer `incus network list` verá que está em `errored`), faça o primeiro e mais importante passo: olhe o log:
```sh
cat /var/log/ovn/{log-archive}
```

Caso esteja aparecendo um problema com **Raft**, o problema pode ser a versão do seu ovn. Para isso veja a versão, caso seja 22 ou menor, muito provavelmente este é o problema!
```sh
ovn-nbctl --version
```
Isso se deve ao problema de conectividade que há nesta versão.
Para resolver é problematica