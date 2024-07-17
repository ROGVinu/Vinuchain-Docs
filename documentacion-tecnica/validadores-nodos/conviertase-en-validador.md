---
description: Como convertirse en Validador / Instalar Nodo
---

# Conviértase en Validador

## Parámetros del Validador

* Tamaño Mínimo de Staking:&#x20;

```
100,000 VC
```

* Tamaño máximo del Validador:&#x20;

```
15x the self-stake amount
```

* Requisitos mínimos de hardware:

```
m5.xlarge (4 CPUs and 16GB), 500GB SSD
```

* Recompensas:

```
Actualmente ~6% APY (APY normal sobre el staking propio + 100% de las recompensas de los delegados).
El APY varía en función del % stakeado.
```

## Lanzar una instancia en la nube <a href="#launch-cloud-instance" id="launch-cloud-instance"></a>

Puede ejecutar un nodo en su propio hardware o utilizar un proveedor en la nube.&#x20;

Le recomendamos que elija uno de los grandes proveedores de servicios en la nube, como Amazon AWS.

### Especificaciones del nodo  <a href="#node-specifications" id="node-specifications"></a>

Recomendamos las siguientes caracteristicas o mejores:

Instancia de propósito general m5.xlarge con 4 vCPUs (3,1 GHz), 16 GB de memoria, hasta 10 Gbps de ancho de banda de red y al menos 500 GB de espacio en disco.&#x20;

AWS m6i.2xlarge, c6i.4xlarge pueden proporcionar un mejor rendimiento.&#x20;

Recomendamos Ubuntu Server 22.04 LTS (64 bits).

​[Genesis Files](https://drive.google.com/drive/folders/1\_LKq9ljXYwH4LkxO-6e8UnVgiWncGCBH?usp=sharing)​

### Configuración de red  <a href="#network-settings" id="network-settings"></a>

* Abrir **puerto 22** para SSH
* Abrir **puerto 5050** para ambos traficos TCP y UDP.
* Abrir **puerto 4000**

Se puede utilizar un puerto personalizado con el flag "--port " cuando ejecute su nodo opera.

### Configurar usuario no root  <a href="#set-up-non-root-user" id="set-up-non-root-user"></a>

Si ya hay un usuario no-root disponible, puede saltarse este paso.

```
# SSH en su máquina
(local)$ ssh root@{VALIDATOR_IP_ADDRESS}
# Actualizar el sistema
(validator)$ sudo apt-get update && sudo apt-get upgrade -y
# Crear un usuario no root
(validator)$ USER={USERNAME}
(validator)$ sudo mkdir -p /home/$USER/.ssh
(validator)$ sudo touch /home/$USER/.ssh/authorized_keys
(validator)$ sudo useradd -d /home/$USER $USER
(validator)$ sudo usermod -aG sudo $USER
(validator)$ sudo chown -R $USER:$USER /home/$USER/
(validator)$ sudo chmod 700 /home/$USER/.ssh
(validator)$ sudo chmod 644 /home/$USER/.ssh/authorized_keys
```

* Para `(validator)$ USER={USERNAME}` escribe tu nombre de usuario no root.
* Asegúrese de pegar su clave SSH pública en el archivo `authorized_keys` filedel usuario recién creado para poder iniciar sesión a través de SSH.

```
# Habilitar sudo sin contraseña para el usuario
(validator)$ sudo vi /etc/sudoers
```

Añade la siguiente línea al final del fichero:

```
{USERNAME} ALL=NOPASSWD: ALL
```

* Si esto no funciona para usted, intente`sudo passwd nonrootusername`
* No es necesario en Ubuntu 22.04

Ahora cierre la conexión SSH raíz a la máquina e inicie sesión como su usuario recién creado:

```
# Cerrar la conexión SSH root
(validator)$ exit
# Inicie sesión como nuevo usuario
(local)$ ssh {USERNAME}@{VALIDATOR_IP_ADDRESS}
```

## Instalar las herramientas necesarias

Usted todavía está conectado como el nuevo usuario a través de SSH.&#x20;

Ahora vamos a instalar **Go** y **Opera**.&#x20;

En primer lugar, instale las herramientas de compilación necesarias

```
# Instalar build-essential
(validador)$ sudo apt-get install -y build-essential
```

### **Instalar Go** <a href="#install-go" id="install-go"></a>

```
# Instalar go
(validator)$ wget https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
(validator)$ sudo tar -xvf go1.19.3.linux-amd64.tar.gz
(validator)$ sudo mv go /usr/local
```

Si eso no funcionó, también puedes probar:

```
# Instalar go
(validator)$ sudo snap install go --classic
```

Exporte las rutas Go necesarias:

```
# Export go paths
(validator)$ vi ~/.bash_aliases
# Añade las siguientes líneas
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
(validator)$ source ~/.bash_aliases
```

Si la última línea no te ha funcionado, prueba con `(validator)$ . ~/.bash_aliases`

Valida tu instalación de Go

```
go version
```

### **Instalar Opera** <a href="#install-opera" id="install-opera"></a>

```
# Install Opera
(validator)$ git clone https://github.com/VinuChain/VinuChain
(validator)$ cd VinuChain/
(validator)$ make
```

Valida tu instalación de Opera:

```
$./build/opera version
VERSION:
1.1.2-rc.3
```

## Registrar el validador VinuChain

Después de instalar todo lo que necesitamos, puedes continuar registrando tu nodo validador VinuChain en la cadena. Para ello, es necesario crear una billetera validador. La billetera es la identidad del validador en la red que utiliza para autenticarse, firmar mensajes, etc.

### Iniciar el nodo de sólo lectura Opera

En primer lugar, inicia el **nodo Opera de sólo lectura** para interactuar con él y crear una billetera validador:

```
# Iniciar nodo opera
(validator)$ cd build/
(validator)$ nohup ./opera --port 3000 --nat any 
--genesis ./genesis-testnet.g --genesis.allowExperimental 
--bootnodes enode://3c4da2358ce3c3e117b03e4c87dff1d8d767a684e3c94f5eb29a4e88f5
49ba2f5a458eab60df637417411bb59b52f94542cf7d22f0dd1a10e45d5ae71c66e334
@54.203.151.219:3000 > opera.log &
```

​[Genesis Files](https://drive.google.com/drive/folders/1\_LKq9ljXYwH4LkxO-6e8UnVgiWncGCBH?usp=sharing)​

Hay diferentes maneras de Ejecutar su nodo de sólo lectura.&#x20;

Tenga en cuenta que **https** y **ws** **NO DEBEN** estar habilitados en un servidor que almacena cuenta billetera.&#x20;

Iniciando su nodo se verá algo como esto:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2Fttk2mhbEPzGVtQQRFna9%2Fasasf.png?alt=media&#x26;token=b5256ba8-9266-44cf-b330-1dd0d681ef1a" alt=""><figcaption></figcaption></figure>

El nodo debería empezar a sincronizar los datos de red:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2Fe6ygNMRSOZbfwNp0zJSN%2Fsagag.png?alt=media&#x26;token=7fcf0877-0824-4906-a350-46d3fac782d6" alt=""><figcaption></figcaption></figure>

Una vez que esté funcionando debes esperar a que se sincronice con el último bloque antes de proceder al siguiente paso.

### Crear una billetera validador

El nodo está ahora funcionando y sincronizando la red en tu consola actual, así que necesitas abrir una nueva ventana de consola, conectarte vía **SSH** al servidor e introducir los siguientes comandos para crear **una billetera validador**:

```
# Create validator wallet
(validator)$ ./opera account new
```

Después de introducir el comando, se te pedirá que introduzcas una contraseña para la cuenta (= billetera) - ¡utiliza una fuerte! Por ejemplo, puedes utilizar un gestor de contraseñas para generar una contraseña de más de 20 dígitos para proteger tu billetera. Será algo parecido a esto:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2F2JCsyWTiq2YwZN7VDIZb%2Fgdhdsf.png?alt=media&#x26;token=f6be5bd9-75e8-4e27-9532-91e7cc397375" alt=""><figcaption></figcaption></figure>

NUNCA compartas tu clave privada o almacén de claves con nadie.&#x20;

_Por cierto: La billetera de arriba no es una billetera real que utilizamos, es sólo para fines de demostración._

#### Financiar tu billetera validador  <a href="#fund-your-validator-wallet" id="fund-your-validator-wallet"></a>

El siguiente paso es financiar tu billetera validador con suficiente VC para convertirte en validador. Eso significa que necesitas tener al menos **100.000 VC** en la billetera que acabas de crear (envía un poco más para cubrir las tasas de transacción). Después de intercambiar con éxito los VC a su recién creada billetera Opera, puede **registrar su validador a través del Contrato Inteligente SFC**.&#x20;

Asegúrate de esperar a que tu nodo esté completamente sincronizado, ¡de lo contrario tu VC no aparecerá en tu billetera!

### Crear una nueva clave de validador

Tenemos que crear la clave privada del validador para firmar los mensajes de consenso. Sólo se puede hacer usando go-opera

```
(validator)$ ./opera validator new
```

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FMa2iTVaRlmJuPETaizMK%2Fasdasdas.png?alt=media&#x26;token=eb974bf1-a368-4af0-a0d1-6ba5fa50e4b5" alt=""><figcaption></figcaption></figure>

Siga las instrucciones y proporcione la contraseña, la salida es la siguiente:

```
Your new key was generated

Public key: 0xPubkey
Path of the secret key file:

- You can share your public key with anyone. Others need it to validate
messages from you.
- You must NEVER share the secret key with anyone! The key controls
access to your validator!
- You must BACKUP your key file! Without the key, it's impossible to
operate the validator!
- You must REMEMBER your password! Without the password, it's
impossible to decrypt the key!
```

### Crea tu validador a través del SFC

Debe esperar a que su nodo se sincronice con el último bloque de la red antes de proceder.

#### Adjuntar a consola opera  <a href="#attach-to-opera-console" id="attach-to-opera-console"></a>

Para proceder, abra la consola donde introdujo los comandos para crear la billetera validador anteriormente y adjúntelo a la consola del nodo Opera:

```
# Adjuntar a consola opera
(validador)$ ./opera attach
```

Al hacerlo, obtendrá una consola JavaScript donde podrá interactuar directamente con el nodo Opera y, por ejemplo, enviar transacciones (lo que hará en un momento):

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2F9jo08TfVrWvJE4OUK1wB%2Fasdasdass.png?alt=media&#x26;token=44c27a1b-0e57-4af5-9f12-e3975da91eba" alt=""><figcaption></figcaption></figure>

#### Inicializar SFC <a href="#initialize-sfc" id="initialize-sfc"></a>

Ahora inicializar el **contrato SFC ABI variable**&#x20;

Parsee hacia el terminal de javascript todo desde [https://notepad.pw/raw/QIMEW3BawgTp06zTxzEy](https://notepad.pw/raw/QIMEW3BawgTp06zTxzEy) o introduzca lo siguiente:

```
(https://github.com/VinuChain/VinuChain):
abi = JSON.parse
(https://notepad.pw/raw/QIMEW3BawgTp06zTxzEy)
abi = JSON.parse("[{\"anonym…able\",\"type\":\"function\"}]")
```

Aquí está el código en bruto desde el enlace en caso de que se rompe:

```
abi = JSON.parse("[{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"status\",\"type\":\"uint256\"}],\"name\":\"ChangedValidatorStatus\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"lockupExtraReward\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"lockupBaseReward\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"unlockedReward\",\"type\":\"uint256\"}],\"name\":\"ClaimedRewards\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":true,\"internalType\":\"address\",\"name\":\"auth\",\"type\":\"address\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"createdEpoch\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"createdTime\",\"type\":\"uint256\"}],\"name\":\"CreatedValidator\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"deactivatedEpoch\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"deactivatedTime\",\"type\":\"uint256\"}],\"name\":\"DeactivatedValidator\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"Delegated\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"receiver\",\"type\":\"address\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"string\",\"name\":\"justification\",\"type\":\"string\"}],\"name\":\"InflatedFTM\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"duration\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"LockedUpStake\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"previousOwner\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"address\",\"name\":\"newOwner\",\"type\":\"address\"}],\"name\":\"OwnershipTransferred\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"RefundedSlashedLegacyDelegation\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"lockupExtraReward\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"lockupBaseReward\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"unlockedReward\",\"type\":\"uint256\"}],\"name\":\"RestakedRewards\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"wrID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"Undelegated\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"penalty\",\"type\":\"uint256\"}],\"name\":\"UnlockedStake\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"UpdatedBaseRewardPerSec\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"blocksNum\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"period\",\"type\":\"uint256\"}],\"name\":\"UpdatedOfflinePenaltyThreshold\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"refundRatio\",\"type\":\"uint256\"}],\"name\":\"UpdatedSlashingRefundRatio\",\"type\":\"event\"},{\"anonymous\":false,\"inputs\":[{\"indexed\":true,\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"indexed\":true,\"internalType\":\"uint256\",\"name\":\"wrID\",\"type\":\"uint256\"},{\"indexed\":false,\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"Withdrawn\",\"type\":\"event\"},{\"constant\":true,\"inputs\":[],\"name\":\"baseRewardPerSecond\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"contractCommission\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"currentSealedEpoch\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getEpochSnapshot\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"endTime\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"epochFee\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"totalBaseRewardWeight\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"totalTxRewardWeight\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"baseRewardPerSecond\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"totalStake\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"totalSupply\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getLockupInfo\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"lockedStake\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"fromEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"endTime\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"duration\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getStashedLockupRewards\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"lockupExtraReward\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupBaseReward\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"unlockedReward\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getValidator\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"status\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"deactivatedTime\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"deactivatedEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"receivedStake\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"createdEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"createdTime\",\"type\":\"uint256\"},{\"internalType\":\"address\",\"name\":\"auth\",\"type\":\"address\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"}],\"name\":\"getValidatorID\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getValidatorPubkey\",\"outputs\":[{\"internalType\":\"bytes\",\"name\":\"\",\"type\":\"bytes\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"getWithdrawalRequest\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"time\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"isOwner\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"lastValidatorID\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"maxDelegatedRatio\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"maxLockupDuration\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"minLockupDuration\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"minSelfStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"owner\",\"outputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[],\"name\":\"renounceOwnership\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"slashingRefundRatio\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"stakeTokenizerAddress\",\"outputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"stakes\",\"outputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint96\",\"name\":\"timestamp\",\"type\":\"uint96\"},{\"internalType\":\"uint256\",\"name\":\"validatorId\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"name\":\"stashedRewardsUntilEpoch\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"totalActiveStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"totalSlashedStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"totalStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"totalSupply\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"newOwner\",\"type\":\"address\"}],\"name\":\"transferOwnership\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"unlockedRewardRatio\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"validatorCommission\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"version\",\"outputs\":[{\"internalType\":\"bytes3\",\"name\":\"\",\"type\":\"bytes3\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"withdrawalPeriodEpochs\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"withdrawalPeriodTime\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"pure\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"currentEpoch\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"}],\"name\":\"getEpochValidatorIDs\",\"outputs\":[{\"internalType\":\"uint256[]\",\"name\":\"\",\"type\":\"uint256[]\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochReceivedStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochAccumulatedRewardPerToken\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochAccumulatedUptime\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochAccumulatedOriginatedTxsFee\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochOfflineTime\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getEpochOfflineBlocks\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"rewardsStash\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"getLockedStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"offset\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"limit\",\"type\":\"uint256\"}],\"name\":\"getStakes\",\"outputs\":[{\"components\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint96\",\"name\":\"timestamp\",\"type\":\"uint96\"},{\"internalType\":\"uint256\",\"name\":\"validatorId\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"internalType\":\"structSFC.Stake[]\",\"name\":\"\",\"type\":\"tuple[]\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"offset\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"limit\",\"type\":\"uint256\"}],\"name\":\"getWrRequests\",\"outputs\":[{\"components\":[{\"internalType\":\"uint256\",\"name\":\"epoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"time\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"internalType\":\"structSFC.WithdrawalRequest[]\",\"name\":\"\",\"type\":\"tuple[]\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"sealedEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"_totalSupply\",\"type\":\"uint256\"},{\"internalType\":\"address\",\"name\":\"nodeDriver\",\"type\":\"address\"},{\"internalType\":\"address\",\"name\":\"owner\",\"type\":\"address\"}],\"name\":\"initialize\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"auth\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"internalType\":\"bytes\",\"name\":\"pubkey\",\"type\":\"bytes\"},{\"internalType\":\"uint256\",\"name\":\"status\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"createdEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"createdTime\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"deactivatedEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"deactivatedTime\",\"type\":\"uint256\"}],\"name\":\"setGenesisValidator\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"stake\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockedStake\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupFromEpoch\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupEndTime\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupDuration\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"earlyUnlockPenalty\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"rewards\",\"type\":\"uint256\"}],\"name\":\"setGenesisDelegation\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"bytes\",\"name\":\"pubkey\",\"type\":\"bytes\"}],\"name\":\"createValidator\",\"outputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"getSelfStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"delegate\",\"outputs\":[],\"payable\":true,\"stateMutability\":\"payable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"undelegate\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"}],\"name\":\"isSlashed\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"wrID\",\"type\":\"uint256\"}],\"name\":\"withdraw\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"status\",\"type\":\"uint256\"}],\"name\":\"deactivateValidator\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"pendingRewards\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"stashRewards\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"claimRewards\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"restakeRewards\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"internalType\":\"bool\",\"name\":\"syncPubkey\",\"type\":\"bool\"}],\"name\":\"_syncValidator\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"offlinePenaltyThreshold\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"blocksNum\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"time\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"value\",\"type\":\"uint256\"}],\"name\":\"updateBaseRewardPerSecond\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"blocksNum\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"time\",\"type\":\"uint256\"}],\"name\":\"updateOfflinePenaltyThreshold\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"validatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"refundRatio\",\"type\":\"uint256\"}],\"name\":\"updateSlashingRefundRatio\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"address\",\"name\":\"addr\",\"type\":\"address\"}],\"name\":\"updateStakeTokenizerAddress\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"int256\",\"name\":\"diff\",\"type\":\"int256\"}],\"name\":\"updateTotalSupply\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"addresspayable\",\"name\":\"receiver\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"},{\"internalType\":\"string\",\"name\":\"justification\",\"type\":\"string\"}],\"name\":\"mintFTM\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256[]\",\"name\":\"offlineTime\",\"type\":\"uint256[]\"},{\"internalType\":\"uint256[]\",\"name\":\"offlineBlocks\",\"type\":\"uint256[]\"},{\"internalType\":\"uint256[]\",\"name\":\"uptimes\",\"type\":\"uint256[]\"},{\"internalType\":\"uint256[]\",\"name\":\"originatedTxsFee\",\"type\":\"uint256[]\"}],\"name\":\"sealEpoch\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256[]\",\"name\":\"nextValidatorIDs\",\"type\":\"uint256[]\"}],\"name\":\"sealEpochValidators\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"isLockedUp\",\"outputs\":[{\"internalType\":\"bool\",\"name\":\"\",\"type\":\"bool\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[{\"internalType\":\"address\",\"name\":\"delegator\",\"type\":\"address\"},{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"}],\"name\":\"getUnlockedStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupDuration\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"lockStake\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"lockupDuration\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"relockStake\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":false,\"inputs\":[{\"internalType\":\"uint256\",\"name\":\"toValidatorID\",\"type\":\"uint256\"},{\"internalType\":\"uint256\",\"name\":\"amount\",\"type\":\"uint256\"}],\"name\":\"unlockStake\",\"outputs\":[{\"internalType\":\"uint256\",\"name\":\"\",\"type\":\"uint256\"}],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"}]")
```

**Inicializar el propio objeto de contrato SFC**:

```
sfcc = web3.ftm.contract(abi).at("0xfc00face00000000000000000000000000000000")
```

Tras inicializar ambas variables, ya puede interactuar con el SFC de la red.&#x20;

#### Chequeo de Sanidad <a href="#sanity-check" id="sanity-check"></a>

Introduce el siguiente comando para comprobar que todo funciona como se espera:

```
// Sanity check
sfcc.lastValidatorID()
// si todo está bien, devolverá un valor distinto de cero
```

Si tiene este aspecto, todo está bien (no deberías obtener un error aquí):

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FEFneP4yG0MBdNq55CujN%2Fsvsdb.png?alt=media&#x26;token=2820490b-525e-41bf-a7d5-91fef915ff8e" alt=""><figcaption></figcaption></figure>

### Obtenga su ID de validador

A continuación, intenta obtener tu `ValidatorID` del SFC utilizando tu dirección de billetera validador generada previamente:

```
# Obtenga su identificador de validador
sfcc.getValidatorID("VALIDATOR_WALLET_ADDRESS")
```

* Utilice comillas para  "VALIDATOR\_WALLET\_ADDRESS".

Esto debería devolver **0**, ya que aún no está registrado como validador:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FtZXodFQD5liniZhywVK2%2Fnndbd.png?alt=media&#x26;token=986608fc-5d6f-433d-9b58-4112cf52b775" alt=""><figcaption></figcaption></figure>

### Desbloquear la billetera validador

A continuación, desbloquea tu billetera de validador para poder ejecutar la transacción de registro (asegúrate de utilizar la contraseña que estableciste anteriormente). Ten en cuenta que puedes realizar este paso (unlockAccount + createValidator) en una máquina separada (diferente de la máquina en la que ejecutarás tu nodo validador), o sería incluso más seguro si utilizas un monedero hardware para hacerlo.

```
# Unlock validator wallet
personal.unlockAccount("VALIDATOR_WALLET_ADDRESS", "PASSWORD", 300)
```

* Utilice comillas para "VALIDATOR\_WALLET\_ADRESS" and "PASSWORD".

Esto devolverá "**true**" si el desbloqueo de la cartera se ha realizado correctamente:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FgrjogoZ0qxr6tiLtZ43z%2Fldalal.png?alt=media&#x26;token=c1a08eef-16bf-4499-9656-6769123a1e62" alt=""><figcaption></figcaption></figure>

### Registre su validador

A continuación, envíe la transacción `createValidator` para registrar su validador **(el valor es la representación de la unidad VC más pequeña, por lo que dividiéndolo por 1e18 obtendrá 100.000 VC**.&#x20;

Como alternativa, puede utilizar `web3.toWei("100000.0", "vc")).`

```
# Registre su validasor
tx = sfcc.createValidator("0xYOUR_PUBKEY", {from:"0xYOUR_ADDRESS", value:
web3.toWei("100000.0", "ftm")}) // 100000.0 VC
```

* Utilice comillas para "0xYOUR\_PUBKEY" y "0xYOUR\_ADDRESS".
* 0XYOUR\_ADDRESS se refiere al monedero creado en el validador.

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2Fl6gyGPDMwzHCyZeAy9Av%2Fbbadaa.png?alt=media&#x26;token=9e41b5e3-6a03-4638-8ce1-5fcaa33a43ea" alt=""><figcaption></figcaption></figure>

### Compruebe su transacción de registro

Asegúrese de comprobar su transacción de registro (puede tardar unos instantes en confirmarse):

```
# Compruebe su transacción de registro
ftm.getTransactionReceipt(tx)
```

Busque el **estado: "0x1"** en la parte inferior, lo que significa que la transacción se ha realizado correctamente:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FHiM4mAMvILKMaf87VmQo%2Fbbabdba.png?alt=media&#x26;token=e00874a9-53c8-4882-af6d-7916a5007bbb" alt=""><figcaption></figcaption></figure>

También puede copiar el `transactionHash` e ir al VinuChain [BlockScanner](https://vinuscan.com/) y comprobar su transacción allí:

> https://vinuscan.com/transactions/\[YOURTX]

Esto se vería algo como lo siguiente:

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2Fng97IocqvVyrl0x6QzNf%2Fbjkdbgjk.png?alt=media&#x26;token=b65253c7-5107-45e6-b28d-c56fbd572809" alt=""><figcaption></figcaption></figure>

#### Compruebe su ID de validador

Finalmente, ejecute de nuevo el siguiente comando para comprobar su `validatorID`:

```
# Obtenga su ID validator 
sfc.getValidatorID("{VALIDATOR_WALLET_ADDRESS}")
```

Ahora debería devolver algo distinto de "0":

<figure><img src="https://1328849348-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FhbaQDLv6NPQV090j80u8%2Fuploads%2FsUy5qI2kNwG14hymGHEE%2Fubsbjs.png?alt=media&#x26;token=9ac67de6-6d26-4b21-a374-438e0b9b6053" alt=""><figcaption></figcaption></figure>

¡Felicidades, ya es un validador VinuChain!&#x20;

Cierre la ventana de la consola de Opera tecleando "exit".

### Ejecute su nodo validador VinuChain

#### Reinicie su nodo en modo validador

Antes de salir de la celebración, debe **reiniciar su nodo en modo validador**!

Asegúrate de que tu nodo ya está sincronizado con el último bloque en modo lectura, y que está sincronizado en --syncmode full.

* Detén el proceso de opera (modo lectura):

```
pkill opera
```

o

```
(validator)$ sudo killall opera
```

* Crea un **archivo vacío** y pega en él la **contraseña del nodo validator**.&#x20;

A continuación, vuelve a la ventana de la consola donde iniciaste tu nodo con el siguiente comando:

```
nohup ./opera --bootnodes enode://3c4da2358ce3c3e117b03e4c87dff1d8d767a684e3c9
4f5eb29a4e88f549ba2f5a458eab60df637417411bb59b52f94542cf7d22f0dd1a10e45d5ae71c
66e334@54.203.151.219:3000 --validator.id ID --validator.pubkey VALIDATOR_PUBKEY 
--validator.password PATH_TO_PASSWORDFILE > validator.log &
```

* Debes sustituir las variables `VALIDATOR_PUBKEY` & `PASSWORD_FILE` por tus propios valores.

### Actualizar la información del validador

#### Nombre y Logo <a href="#name-and-logo" id="name-and-logo"></a>

Si quieres configurar un nombre y un logotipo para tu nodo, ve a [Actualizar información del validador](actualizar-informacion-del-validador.md)
