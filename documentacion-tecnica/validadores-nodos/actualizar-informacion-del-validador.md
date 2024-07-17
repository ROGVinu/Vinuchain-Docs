---
description: Cómo actualizar la información del Validador
---

# Actualizar información del Validador

### Archivo de configuración

Cree un archivo de configuración en formato JSON que contenga los siguientes parámetros (también puede dejar los parámetros vacíos):

```
{  "name": "VALIDATOR_NAME", /* Name of the validator */
  "logoUrl": "LOGO_URL", /* Validator logo (PNG|JPEG|SVG) - 100px x 100px is enough */
  "website": "WEBSITE_URL", /* Website icon on the right */
  "contact": "CONTACT_URL" /* Contact icon on the right */
}
```

Ejemplo:​

```
{
  "name": "any",
  "logoUrl": "https://any.site/vinu/any.png",
  "website": "https://any.site",
  "contact": "https://t.me/any_vinu"
}
```

A continuación, alojar en algún lugar de acceso público.

### Actualiza tu información en el contrato inteligente

1. Conéctate a tu nodo validador
2. Abra una sesión de consola go-opera a través de go-opera attach
3. go-opera attach Cargar el contrato StakerInfo ABI e instanciar el contrato:

```
abi = JSON.parse('[{"inputs":[{"internalType":"address","name":"_stakerContractAddress","type":"address"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":false,"internalType":"uint256","name":"stakerID","type":"uint256"}],"name":"InfoUpdated","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"previousOwner","type":"address"},{"indexed":true,"internalType":"address","name":"newOwner","type":"address"}],"name":"OwnershipTransferred","type":"event"},{"constant":true,"inputs":[],"name":"isOwner","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"internalType":"address","name":"","type":"address"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[],"name":"renounceOwnership","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"stakerInfos","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"newOwner","type":"address"}],"name":"transferOwnership","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"_stakerContractAddress","type":"address"}],"name":"updateStakerContractAddress","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"string","name":"_configUrl","type":"string"}],"name":"updateInfo","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"uint256","name":"_stakerID","type":"uint256"}],"name":"getInfo","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"}]')
stakerInfoContract = web3.ftm.contract(abi).at("0x17e3c9bFc3eFad6DA41277733Cc3d70441a96737")
```

4. Llama a la función `updateInfo` del contrato StakerInfo (asegúrate de que tienes suficiente VC en tu billetera para cubrir la comisión de la transacción)

```
stakerInfoContract.updateInfo("CONFIG_URL", { from: "VALIDATOR_ADDRESS" })
```

Ejemplo:

```
stakerInfoContract.updateInfo("https://any.site/vinu/config.json", { from: "address" })
```

5. Valide si ha actualizado correctamente la información

```
stakerInfoContract.getInfo(STAKER_ID)
```

Ejemplo:

```
stakerInfoContract.getInfo(14)
```
