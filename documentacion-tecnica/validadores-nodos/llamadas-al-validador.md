---
description: Referencia de las llamadas al validador
---

# Llamadas al Validador

## Crear validador&#x20;

Crea un nuevo validador

El stake mínimo actual es `sfcc.minSelfStake()`

`pubkey` es una clave pública utilizada para autenticar los futuros mensajes de consenso del validador. `pubkey` no puede cambiarse después de la llamada.

La llamada crea una autodelegación con el importe especificado. El validador utiliza las mismas llamadas que otros delegadores. Visita [Llamadas de Delegación](llamadas-de-delegacion.md), [Llamadas de Recompensas](llamadas-de-recompensas.md), [Llamadas de bloqueo de Stake](llamadas-de-bloqueo-de-stake.md) para obtener más detalles.

```
sfcc.createValidator("0xPubkey", {from:"0xAddress", value: web3.toWei("amount", "vc")})
```

**Comprobaciones:**

* El monto de la auto-stake es mayor o igual que `sfcc.minSelfStake()`
* Esta dirección no se ha utilizado para otro validador.
* `pubkey` no está vacío
