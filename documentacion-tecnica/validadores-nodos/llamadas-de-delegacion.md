---
description: Referencia de las llamadas de delegación de VC a llamadas de validadores
---

# Llamadas de Delegación

### Delegar

Delega una cantidad de VC a un validador.

Puede utilizarse para crear una nueva delegación o aumentar una delegación existente.

La participación delegada siempre está desbloqueada a menos que se bloquee explícitamente con `lockStake`.

```
sfcc.delegate(validatorID, {from: "0xAddress", value: web3.toWei("amount", "vc")})
```

**Comprobaciones:**

* El validador debe existir
* El validador está activo
* El importe es mayor que cero
* El `staking del validator` es menos o igual que  `15.0` \* `auto-staking del validator`

### Undelegar

Realiza una solicitud para retirar una cantidad delegada de staking.

Una vez transcurridos una serie de segundos y épocas desde la llamada a la función siguiente (denominada período de retirada), podrá llamar a `withdraw` con éxito.

`requestID` es cualquier número que no esté ocupado por una solicitud de retirada existente para esta delegación.

```
sfcc.undelegate(validatorID, requestID, web3.toWei("amount", "vc"), {from: "0xAddress"})
```

**Comprobaciones:**

* El importe es mayor que cero
* El `stake desbloqueado` de la delegación es mayor o igual que el importe que se retira de delegar
* `requestID` no está ocupado por una solicitud de retirada existente para esta delegación
* Si se solicita para la autodelegación del validador, después de la operación se cumple lo siguiente: o bien el `staking del validador` es menor o igual que `15.0` \* `self-stake del validador` o bien el `self-stake` es `0`.&#x20;

El periodo de retirada en segundos y épocas puede obtenerse mediante

```
sfcc.withdrawalPeriodTime()
sfcc.withdrawalPeriodEpochs()
```

#### Retiros

Finaliza la solicitud de retirada.&#x20;

Borra el objeto de solicitud y retira el stake solicitado, transfiere el stake solicitado a una dirección de cuenta.&#x20;

Tenga en cuenta que debe transcurrir un número de segundos y épocas desde la llamada `undelegate` (denominado período de retirada).&#x20;

Si el validador es un tramposo (es decir, con doble firma), el stake puede ser penalizado total o parcialmente según el `slashingRefundRatio` del validador.

```
sfcc.withdraw(validatorID, requestID, {from: "0xAddress"})
```

**Comprobaciones:**

* Existe una solicitud de retirada con la dirección, el ID de validador y el ID de solicitud indicados
* Han transcurrido al menos `sfcc.withdrawalPeriodTime()` segundos desde la llamada a `undelegate`&#x20;
* Han transcurrido al menos `sfcc.withdrawalPeriodEpochs()` épocas desde la llamada a la función de `undelegate`&#x20;
* La parte no-penalizada del stake es superior a cero
