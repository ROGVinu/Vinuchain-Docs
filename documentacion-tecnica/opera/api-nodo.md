# API Nodo

## Configurar el nodo API

### 1. Inicie un nodo Opera de sólo lectura

* Siga las instrucciones en [Nodo de sólo lectura.](nodo-solo-lectura.md)

### Migrar un nodo API existente

Ejecutar con `'--db.preset legacy-ldb --db.migration.mode reformat'` no necesita una resincronización.

### Iniciar un nuevo nodo API

Puede utilizar `--db.preset legacy-ldb` o `--db.preset ldb-1` or `--db.preset pbl-1`.

### Activar la API de seguimiento

Para habilitar el rastreo, puedes añadir la flag `--tracenode`.



### 2. Habilitar los API endpoints

Por defecto, la única forma de enviar peticiones API al nodo es a través del archivo `.ipc` dentro del datadir. &#x20;

Para habilitar los puntos finales HTTP y/o WS, añada las flags:

```
--http --ws
```

Los puertos por defecto para HTTP y WS son 18545 y 18546. Si es necesario, puede anularlos con las flags `--http.port` y `--ws.port`&#x20;

Por defecto, los endpoints sólo son accesibles por localhost. Para permitir peticiones externas, añada las flags:

```
--http --http.vhosts="*" --http.corsdomain="*" --ws --ws.origins="*"
```

Los namespaces predeterminados se limitan a `vc, eth, abft, dag, rpc, web3`. Para permitir todos los espacios de nombres, añada las flags:

```
--http --http.api="vc,eth,debug,admin,web3,personal,net,txpool,sfc" --ws --ws.api="vc,eth,debug,admin,web3,personal,net,txpool,sfc"
```

Preste atención a que la lista completa de namespaces proporcione acceso directo al nodo.&#x20;

Permita el menor número posible de namespaces y limite el acceso al menor número posible de hosts.
