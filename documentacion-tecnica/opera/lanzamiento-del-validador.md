# Lanzamiento del Validador

## Ejecutar el validador Opera

### 1. Configurar un nodo AWS

* Ubuntu Server 20.04 LTS 64-bit
* Recomendamos lo siguiente (o mejor):

```
m5.xlarge (4 CPUs and 16GB), 500GB SSD
```

* Abra el puerto 22 para SSH y el puerto 5050 para el tráfico TCP y UDP.

### 2. Inicie un nodo Opera de sólo lectura

* Siga las instrucciones en [Nodo de sólo-lectura ](nodo-solo-lectura.md)con limitaciones:
  * Utilice sólo la opción `--syncmode full`.
  * Utilice sólo la opción `--db.preset ldb-1` o `--db.preset legacy-ldb`.

Espere a que su nodo se sincronice. Puede añadir el flag  `--exitwhensynced.age 10s` para detener automáticamente el nodo cuando se sincronice.&#x20;

### 3. Ejecute el validador Opera

* Detener nodo de sólo lectura

```
killall opera
```

* Espere hasta que el nodo de sólo lectura se haya detenido&#x20;
* A continuación, ejecute su nodo validador

```
nohup opera --syncmode full --cache $CACHE --validator.id ID --validator.pubkey 0xPubkey --validator.password /path/to/password &
```

* `ID` es su ID de validador (por ejemplo, 25)
* `0xPubkey` es tu clave pública del validador. Has generado tu clave con  `opera validator new`.
* `/path/to/password` es una ruta a un archivo que contiene la contraseña para descifrar la clave del validador (opcional). Si ha omitido el flag `--validator.password`, se le pedirá la contraseña en el terminal.
* `$CACHE` es la cantidad de memoria asignada para go-opera. Sustitúyalo por la mitad de la capacidad RAM del servidor en MB.
* Utilice sólo la opción `--syncmode full` .

Ya está completo. ¡Su nodo está funcionando!
