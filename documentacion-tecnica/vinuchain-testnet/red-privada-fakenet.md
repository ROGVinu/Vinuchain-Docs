---
description: Crea tu propia Red Privad (Fakenet)
---

# Red Privada (Fakenet)

Fakenet es una red privada optimizada para sus pruebas privadas. Generará una génesis que contendrá N validadores con staking iguales. Para lanzar un validador en esta red, todo lo que necesitas hacer es especificar un ID de validador que estás dispuesto a lanzar.

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

#### Scripts

* iniciar red: `./start.sh`;
* detener red `./stop.sh`;
* limpiar datos y registros: `./clean.sh`;

Puede especificar el número de validadores genesis configurando la variable de entorno N.

#### Ejemplo de transferencia de saldo

* Iniciar red:

```
N=3 ./start.sh
```

* Adjuntar js-console al node0 en ejecución:

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

* Obtén la dirección de nodo1:

```
go run ../cmd/opera attach --exec "ftm.accounts[0]" http://localhost:4001
```

la salida muestra la dirección:

```
"0x02aff1d0a9ed566e644f06fcfe7efe00a3261d03"
```

* Transferir cierta cantidad desde el nodo0 a la dirección del nodo1 como receptor (node0 js-console):

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

salidas:

```
9.99999999978999e+26
1.000000000000001e+27
```
