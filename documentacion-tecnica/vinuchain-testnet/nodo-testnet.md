---
description: Ejecutar un Nodo Testnet
---

# Nodo Testnet

La red sólo se especifica mediante su fichero génesis, por lo que ejecutar un nodo testnet equivale a utilizar un fichero génesis testnet en lugar de un fichero génesis mainnet:

```
$ opera --genesis /path/to/testnet.g # launch node
```

Puede ser conveniente utilizar un datadir separado para su nodo testnet para evitar colisiones con otras redes:

```
$ opera --genesis /path/to/testnet.g --datadir /path/to/datadir # launch node
$ opera --datadir /path/to/datadir account new # create new account
$ opera --datadir /path/to/datadir attach # attach to IPC
```

#### Probando

Lachesis tiene una amplia unidad de pruebas. Utilice la herramienta Go para ejecutar pruebas:

```
go test ./...
```

Si todo va bien, debería salir algo parecido a esto:

```
ok  	github.com/VinuChain/VinuChain/app	0.033s
?   	github.com/VinuChain/VinuChain/cmd/cmdtest	[no test files]
ok  	github.com/VinuChain/VinuChain/cmd/opera	13.890s
?   	github.com/VinuChain/VinuChain/cmd/opera/metrics	[no test files]
?   	github.com/VinuChain/VinuChain/cmd/opera/tracing	[no test files]
?   	github.com/VinuChain/VinuChain/crypto	[no test files]
?   	github.com/VinuChain/VinuChain/debug	[no test files]
?   	github.com/VinuChain/VinuChain/ethapi	[no test files]
?   	github.com/VinuChain/VinuChain/eventcheck	[no test files]
?   	github.com/VinuChain/VinuChain/eventcheck/basiccheck	[no test files]
?   	github.com/VinuChain/VinuChain/eventcheck/gaspowercheck	[no test files]
?   	github.com/VinuChain/VinuChain/eventcheck/heavycheck	[no test files]
?   	github.com/VinuChain/VinuChain/eventcheck/parentscheck	[no test files]
ok  	github.com/VinuChain/VinuChain/evmcore	6.322s
?   	github.com/VinuChain/VinuChain/gossip	[no test files]
?   	github.com/VinuChain/VinuChain/gossip/emitter	[no test files]
ok  	github.com/VinuChain/VinuChain/gossip/filters	1.250s
?   	github.com/VinuChain/VinuChain/gossip/gasprice	[no test files]
?   	github.com/VinuChain/VinuChain/gossip/occuredtxs	[no test files]
?   	github.com/VinuChain/VinuChain/gossip/piecefunc	[no test files]
ok  	github.com/VinuChain/VinuChain/integration	21.640s
```

