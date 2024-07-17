---
description: Cadena compatible con EVM asegurada por el algoritmo de consenso Lachesis.
---

# Opera

## Construir el código fuente <a href="#building-the-source" id="building-the-source"></a>

Para construir `opera` se requieren ambos compiladores Go (versión 1.14 or mayor) y un compilador C. Puedes instalarlos utilizando tu gestor de paquetes favorito. Una vez instaladas las dependencias, ejecute:

```
make opera
```

El resultado de la construcción es el ejecutable `build/opera`.

## Ejecutar `Opera` <a href="#running-opera" id="running-opera"></a>

Repasar todas las posibles opciones de línea de comandos está fuera de nuestro alcance aquí, pero hemos enumerado algunas combinaciones de parámetros comunes para que te pongas al día rápidamente sobre cómo puedes ejecutar tu propia instancia de `opera`.

### Lanzamiento de una red

Necesitará un archivo genesis para unirse a una red.

Lanzamiento del nodo `opera` de solo lectura (no validador) para la red especificada por el fichero genesis:

```
$ opera --genesis file.g
```

### **Configuración**

Como alternativa a pasar las numerosas flags al binario de opera, también puede pasar un fichero de configuración mediante:

```
$ opera --config /path/to/your_config.toml
```

Para hacerse una idea de cómo debería ser el fichero puede utilizar el sub-comando `dumpconfig` para exportar su configuración existente:

```
$ opera --your-favourite-flags dumpconfig
```

### **Validador**

Se puede crear una nueva clave privada de validador con el comando `opera validator new`.

Para lanzar un validador, tienes que utilizar las flags `--validator.id` y `--validator.pubkey` para habilitar el emisor de eventos.

```
$ opera --nousb --validator.id YOUR_ID --validator.pubkey 0xYOUR_PUBKEY
```

`opera` le pedirá una contraseña para descifrar su clave privada del validador. Opcionalmente, puede especificar la contraseña con un archivo utilizando el flag  `--validator.password` .

### Participación en el descubrimiento

Opcionalmente puede especificar su IP pública para encaminar la conectividad de la red. Asegúrese de que su puerto TCP/UDP p2p (5050 por defecto) no está bloqueado por su cortafuegos.

```
$ opera --nat extip:1.2.3.4
```

## Ejecutar Testnet

La red se especifica sólo por su fichero génesis, por lo que ejecutar un nodo testnet es equivalente a utilizar un fichero génesis testnet en lugar de un fichero génesis mainnet:

```
$ opera --genesis /path/to/testnet.g #lanzar nodo
```

Puede ser conveniente utilizar un `datadir` separado para su nodo testnet para evitar colisiones con otras redes:

```
$ opera --genesis /path/to/testnet.g --datadir /ruta/a/datadir # lanzar nodo
$ opera --datadir /path/to/datadir account new # crear nueva cuenta
$ opera --datadir /path/to/datadir attach # adjuntar a IPC
```

### **Probando**

Lachesis tiene una amplia pruebas-unitarias. Utilice la herramienta Go para ejecutar pruebas:

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

## Red privada (Fakenet)

Fakenet es una red privada optimizada para sus pruebas privadas. Generará una archivo génesis que contendrá N validadores con staking iguales. Para lanzar un validador en esta red, todo lo que necesitas hacer es especificar un ID de validador que estás dispuesto a lanzar.&#x20;

Presta atención a que las claves privadas de los validadores se generan de forma determinista en esta red, por lo que debes utilizarla sólo para pruebas privadas.

Mantener tu propia red privada es más complicado, ya que muchas de las configuraciones que se dan por sentadas en las redes oficiales necesitan ser configuradas manualmente.

Para ejecutar la fakenet con un solo validador (que funcionará prácticamente como una blockchain PoA), utilice:

```
$ opera --fakenet 1/1
```

Para ejecutar la fakenet con 5 validadores, ejecute el comando para cada validador:

```
$ opera --fakenet 1/5 # first node, use 2/5 for second node
```

Si tiene que lanzar un nodo no validador en fakenet, utilice 0 como ID:

```
$ opera --fakenet 0/5
```

Después de eso, tienes que conectar tus nodos. Conéctelos estáticamente o especifique un bootnode:

```
$ opera --fakenet 1/5 --bootnodes "enode://verylonghex@1.2.3.4:5050"
```

### **Scripts**

* iniciar red: `./start.sh`;
* detener red: `./stop.sh`;
* limpiar datos y registros: `./clean.sh`;

Puede especificar el número de validadores genesis configurando la variable de entorno N.

#### Ejemplo de transferencia de saldo

* Iniciar red:

```
N=3 ./start.sh
```

* Adjuntar js-console al nodo0 en ejecución:

```
go run ../cmd/opera attach http://localhost:4000
```

* Comprueba el saldo para asegurarte de que node0 tiene algo que transferir (node0 js-console):

```
ftm.getBalance(ftm.accounts[0]);
```

la salida muestra el valor del saldo:

```
1e+27
```

* Obtén la dirección de nodo1::

```
go run ../cmd/opera attach --exec "ftm.accounts[0]" http://localhost:4001
```

la salida muestra la dirección:

```
"0x02aff1d0a9ed566e644f06fcfe7efe00a3261d03"
```

* Transferir cierta cantidad desde el nodo0 a la dirección del nodo1 como receptor (nodo0 js-console):

```
ftm.sendTransaction(
	{from: ftm.accounts[0], to: "0x02aff1d0a9ed566e644f06fcfe7efe00a3261d03", value:  "1000000000"},
	function(err, transactionHash) {
        if (!err)
            console.log(transactionHash + " success");
    });
```

la salida muestra el hash único de la transacción saliente:

```
0x68a7c1daeee7e7ab5aedf0d0dba337dbf79ce0988387cf6d63ea73b98193adfd success
```

* Comprueba el estado de la transacción por su hash único (js-console):

```
ftm.getTransactionReceipt("0x68a7c1daeee7e7ab5aedf0d0dba337dbf79ce0988387cf6d63ea73b98193adfd").blockNumber
```

la salida muestra el número de bloque en el que se incluyó la transacción:

```
174
```

* Tan pronto como la transacción se incluya en un bloque, verá el nuevo saldo de ambas direcciones de nodo:

```
go run ../cmd/opera attach --exec "ftm.getBalance(ftm.accounts[0])" http://localhost:4000
go run ../cmd/opera attach --exec "ftm.getBalance(ftm.accounts[0])" http://localhost:4001
```

resultados:

```
9.99999999978999e+26
1.000000000000001e+27
```
