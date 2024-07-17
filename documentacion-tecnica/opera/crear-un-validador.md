# Crear un Validador

## Crear una cuenta nueva

Primero tenemos que crear una nueva cuenta en la que recibir fondos. Se puede hacer en un monedero UI o usando go-opera:

```
opera account new
```

Siga las instrucciones y proporcione la contraseña, recibirá un mensaje;

```
Your new key was generated

Public address of the key:   0xAddress
Path of the secret key file:

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

```
Su nueva clave ha sido generada

Dirección pública de la clave: 0xAddress
Ruta del archivo de la clave secreta:

- Puede compartir su dirección pública con cualquiera. Los demás la necesitan para interactuar contigo.
- NUNCA debes compartir la clave secreta con nadie. La clave controla el acceso a sus fondos.
- Debes hacer una copia de seguridad del archivo de la clave. Sin la clave, es imposible acceder a los fondos de la cuenta.
- Debe RECORDAR su contraseña. Sin la contraseña, es imposible descifrar la clave.
```

## Creación de una nueva clave de validador

Tenemos que crear una clave privada de validador con la que firmar los mensajes de consenso. Sólo se puede hacer usando go-opera:

```
opera validator new
```

Siga las instrucciones y proporcione la contraseña, recibirá un mensaje;

```
Your new key was generated

Public key:                  0xYourPubkey
Path of the secret key file:

- You can share your public key with anyone. Others need it to validate messages from you.
- You must NEVER share the secret key with anyone! The key controls access to your validator!
- You must BACKUP your key file! Without the key, it's impossible to operate the validator!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

```
Su nueva clave ha sido generada

Clave pública: 0xYourPubkey
Ruta del archivo de clave secreta:

- Puede compartir su clave pública con cualquiera. Los demás la necesitan para validar sus mensajes.
- NUNCA debes compartir la clave secreta con nadie. La clave controla el acceso a tu validador.
- Debes hacer una copia de seguridad de tu clave. Sin la clave, es imposible utilizar el validador.
- Debes RECORDAR la contraseña. Sin la contraseña, es imposible descifrar la clave.
```

## Crear un validador en el SFC

### Inicialización del SFC

Siga las instrucciones del [Contrato SFC.](contrato-sfc.md)

### Crear un validador

Crear validador

* Introduzca la clave pública del validador **entre comillas** en lugar de "0xYourPubkey". \
  _Ejemplo_: `"0xc004b81423f875a056d31e8779e2e9fb88f63e826bbe25a15dd00327622828a951aa5f7cc7ffd027b34b25a53ab64d1fbf6ccc2685ef893f36f814ee0d6b90cc5f39"`\
  Asegúrate de especificar una clave pública correcta, ¡ya que es imposible cambiarla después!\

* Introduzca su dirección **entre comillas** en lugar de "0xYourAddress". _Ejemplo_: `"0xfE19B9Ae8b056eE11d20A8F530326a2C3b99ADca"`\
  Esta dirección se utilizará para la autenticación del validador en el contrato inteligente (como cobrar recompensas o votar en el contrato de Gobernanza)
* Sustituir la cantidad de auto-stake del validador en VC en lugar de 500000.0

```
personal.unlockAccount("0xYourAddress", "password", 60) // asegúrese de que la cuenta está desbloqueada
tx = sfcc.createValidator("0xYourPubkey", {from:"0xYourAddress", value: web3.toWei("500000.0", "vc")}) // 500000.0 VC
```

Comprueba que la tx está confirmada

```
ftm.getTransactionReceipt(tx) 
```

Cuando la tx se confirma, debe ser distinto de cero

```
sfcc.getValidatorID("0xYourAddress")
```

## Puesta en marcha como validador

Siga las instrucciones de [Lanzamiento del Validador](lanzamiento-del-validador.md).
