Caso esteja configurando o **Open Virtual Network (OVN)** e, no passo 6, receba a seguinte mensagem de erro:

```sh
# incus config set network.ovn.northbound_connection <ovn-northd-nb-db>
Error: failed to notify peer <your-ip>: Failed to connect to OVS: failed to connect to unix://run/openvswitch/db.sock: listdbs failure - unexpected EOF
```

Esse erro costuma ocorrer devido a uma **incompatibilidade de versão entre o Incus e o OVN**, especialmente se o **Incus foi instalado via APT** (repositório padrão do sistema) sem usar a versão mais recente. O pacote padrão esta pelo menos 10 versões atrasadas de hoje em dia

### Como corrigir:

Para resolver o problema, é recomendável reinstalar o **Incus** utilizando a versão mais recente. Você pode seguir as instruções oficiais de atualização do repositório, disponíveis na página do projeto no GitHub:

🔗 [https://github.com/zabbly/incus](https://github.com/zabbly/incus)