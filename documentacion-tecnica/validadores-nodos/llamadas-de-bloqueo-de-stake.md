---
description: Referencia de las llamadas de bloqueo de staking
---

# Llamadas de Bloqueo de Stake

### Bloqueo de staking

La recompensa por un staking sin bloqueo es el 30% (tasa base) de la recompensa total por un staking con bloqueo.&#x20;

Tenga en cuenta que el staking del validador debe estar bloqueado antes de que las delegaciones del validador puedan bloquear sus staking. El periodo de bloqueo especificado de una delegación no debe superar el periodo de bloqueo actual del validador.&#x20;

La delegación puede bloquearse parcialmente según la cantidad especificada.

`lockupDuration` es la duración del bloqueo en segundos. Debe ser >= 14 días, <= 365 días

`lockupDuration` aumenta proporcionalmente la `tasa de bloqueo` de las recompensas. Para 14 días, las recompensas son`30% + 2.684931%` de la tasa de recompensa total. Para 365 días, las recompensas son el `30% + 70%` de la tasa total de recompensas.

```
sfcc.lockStake(validatorID, lockupDuration, web3.toWei("amount", "vc"), {from: "0xAddress"})
```

**Comprobaciones:**

* El importe es inferior o igual al `stake desbloqueada`&#x20;
* El periodo de bloqueo anterior (si existe) debe finalizar antes de que comience el nuevo periodo.
* `lockupDuration` >= 14 días
* `lockupDuration` <= 365 días
* El periodo de bloqueo del Validador debe finalizar después de que expire el periodo de bloqueo de la delegación.
* El validador está activo

### Re-bloqueo de staking

Amplía el periodo de bloqueo o aumenta el stake bloqueado.&#x20;

Si el usuario ya tiene bloqueado, la llamada creará una nueva entrada de bloqueo con los parámetros: lockedStake =`prevLockedStake + monto` y lockupduration=`newLockupDuration`.

Ejemplo:

1. Hace un mes, Alicia había bloqueado 10 VC durante 3 meses, y tiene que esperar 2 meses hasta que expire el bloqueo.
2. Ha llamado a relockStake(validatorID, 4 meses, 5 VC)
3. Como resultado, ahora tiene 15 VC bloqueados, y tiene que esperar 4 meses hasta que expire el bloqueo (2 meses más, a pesar de que la duración del nuevo bloqueo es sólo 1 mes más larga).

```
sfcc.relockStake(validatorID, newLockupDuration, web3.toWei("amount", "vc"), {from: "0xAddress"})
```

**Comprobaciones:**

* El importe es inferior o igual al `stake desbloqueado`
* `lockupDuration` >= `lockupDuration anterior` (si está bloqueado). Tenga en cuenta que `lockupDuration anterior` no es el `tiempo restante de bloqueo` sino la `duración del bloqueo` existente.
* `lockupDuration` >= 14 días
* `lockupDuration` <= 365 días
* El periodo de bloqueo del validador no debe terminar antes de que expire el periodo de bloqueo de la delegación.
* El validador está activo

### Desbloqueo prematuro del staking

Desbloquea el staking antes de que haya transcurrido la duración del bloqueo.&#x20;

Se retendrá la siguiente penalización de la cantidad desbloqueada:

* `(ratio base = 30%)/2 + ratio de bloqueo` de las recompensas recibidas por épocas durante el periodo de bloqueo

```
sfcc.unlockStake(validatorID, web3.toWei("amount", "vc"), {from: "0xAddress"})
```

**Comprobaciones**:

* El importe es mayor que cero
* El importe es inferior o igual al `stake bloqueada`
* La delegación está bloqueada (total o parcialmente)
