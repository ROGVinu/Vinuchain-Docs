---
description: Referencia de las llamadas de recompensa
---

# Llamadas de Recompensas

## Reclamar recompensas

Reclame las recompensas ganadas. Las recompensas se transferirán a la cuenta.

```
// comprueba que tienes recompensas que reclamar:
sfcc.pendingRewards("0xAddress", validatorID) // returns: rewards amount
// reclamar recompensas:
sfcc.claimRewards(validatorID, {from: "0xAddress"})
```

**Comprobaciones:**

* Delegación `pendingRewards` es mayor que cero

#### Claim rewards <a href="#claim-rewards-1" id="claim-rewards-1"></a>

Reclama las recompensas obtenidas.&#x20;

Las recompensas se añadirán al monto de staking. Si una parte de la recompensa se ha recibido por una staking bloqueado, esta recompensa se añadirá al `stake bloqueado`.

```
// comprueba que tienes recompensas pendientes de reclamar:
sfcc.pendingRewards("0xAddress", validatorID) // returns: rewards amount
// reclama recompensas:
sfcc.claimRewards(validatorID, {from: "0xAddress"})
```

**Comprobaciones:**

* Delegación `pendingRewards` es mayor que cero.
* El `stake del validador` es menor o igual a `15.0` \* `validator's self-stake`
