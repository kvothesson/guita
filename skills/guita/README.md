# guita — Finanzas personales para trabajadores argentinos

Calcula tu ingreso neto real, proyecta tus gastos ajustados por inflacion y te dice cuanto aguantas sin trabajo. Funciona para cualquier situacion laboral: dependencia, monotributo, plataforma, informal, sin trabajo o mixto.

## Instalacion

```bash
claude --plugin-dir /ruta/a/guita
```

## Comandos

| Comando | Que hace |
|---------|----------|
| `/guita sueldo [situacion]` | Calcula tu neto real segun tu situacion laboral |
| `/guita gastos` | Estructura tus gastos y los proyecta con inflacion real |
| `/guita runway` | Cuanto tiempo aguantas con tus ahorros actuales |
| `/guita repartidor` | Template preconfigurado para trabajadores de plataformas |

---

## Ejemplos reales

### `/guita repartidor`

> "Trabajo en Pedidos Ya, en moto propia, unas 12 horas por dia, 6 dias. Gano unos 20.000 por semana."

```
## Ingreso neto real — Repartidor de plataforma

Ingreso bruto declarado:     $80.000/mes (4 semanas x $20.000)
Costos operativos:
  Combustible:               -$14.000/mes
  Mantenimiento moto:        -$6.000/mes
  Cuota unipersonal:         -$10.000/mes
  Internet celular:          -$2.000/mes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto estimado:               $48.000/mes
En dolares blue:             u$s 34/mes (blue venta $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            11% de una canasta

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso representa:       13% del SMVM

Sin cobertura automatica:
  - Sin ART (un accidente es gasto propio)
  - Sin obra social
  - Sin seguro de desempleo
  - La cuenta puede suspenderse sin indemnizacion

Fuente: INDEC / Secretaria de Trabajo — argentina.gob.ar / apis.datos.gob.ar
Dato al: mayo 2026
```

---

### `/guita sueldo dependencia`

> "Trabajo en relacion de dependencia, cobro $700.000 brutos por mes."

```
## Ingreso neto real — Relacion de dependencia

Ingreso bruto declarado:     $700.000/mes
Descuentos aportes (~23%):  -$161.000
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto mensual estimado:       $539.000/mes
+ Aguinaldo prorrateado:    +$58.333/mes
Neto real mensual:           $597.333/mes
En dolares blue:             u$s 427/mes (blue venta $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            1.43 canastas

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso equivale a:       1.64 salarios minimos

Fuente: INDEC / Secretaria de Trabajo — argentina.gob.ar
Dato al: mayo 2026
```

---

### `/guita gastos`

> "Alquiler $250.000, luz y gas $40.000, comida $120.000, internet y cel $18.000, transporte $30.000."

```
## Estructura de gastos — mayo 2026

FIJOS NECESARIOS
  Alquiler:               $250.000/mes
  Servicios (luz/gas):     $40.000/mes
  Subtotal:               $290.000/mes

VARIABLES NECESARIOS
  Comida:                 $120.000/mes
  Transporte:              $30.000/mes
  Internet y celular:      $18.000/mes
  Subtotal:               $168.000/mes

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOTAL GASTOS:             $458.000/mes

Proyeccion con inflacion 3.1%/mes (promedio ene-mar 2026):
  En 3 meses (ago 2026):  $501.000/mes
  En 6 meses (nov 2026):  $549.000/mes

Fuente: INDEC IPC — apis.datos.gob.ar
Dato al: marzo 2026 (ultimo disponible)
```

---

### `/guita runway`

> "Tengo u$s 2.000 ahorrados y gasto $458.000 por mes."

```
## Runway — cuanto aguantas

Ahorros disponibles:      u$s 2.000 = $2.800.000 (blue venta $1.400)
Gastos mensuales:         $458.000/mes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Runway nominal:           6.1 meses (hasta noviembre 2026)
Runway real (c/inflacion 3.1%/mes): 5.4 meses (hasta octubre 2026)

La inflacion te quita 0.7 meses de runway real.

Paso siguiente: con 5 meses de colchon, tenes tiempo para buscar trabajo
sin urgencia extrema, pero conviene activar la busqueda ya.

Fuente: INDEC IPC / dolarapi.com
Dato al: mayo 2026
```

---

## Fuentes

- SMVM: [argentina.gob.ar/trabajo/consejodelsalario](https://www.argentina.gob.ar/trabajo/consejodelsalario)
- IPC / Inflacion: [apis.datos.gob.ar](https://apis.datos.gob.ar/series/api/series/?ids=148.3_INIVELNAL_DICI_M_26&limit=13&sort=desc&format=json)
- Canasta basica: [apis.datos.gob.ar](https://apis.datos.gob.ar/series/api/series/?ids=444.1_CANASTA_batotPampeana_0_0_26_47&limit=1&sort=desc&format=json)
- Dolar: [dolarapi.com](https://dolarapi.com/v1/dolares)
- Monotributo: [afip.gob.ar/monotributo](https://www.afip.gob.ar/monotributo/categorias.asp)
- Seguro de desempleo: [anses.gob.ar](https://www.anses.gob.ar/prestacion/seguro-de-desempleo)
