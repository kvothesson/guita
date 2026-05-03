# guita — Finanzas personales para trabajadores argentinos

Calcula tu ingreso neto real, proyecta tus gastos ajustados por inflacion y te dice cuanto aguantas sin trabajo. Funciona para cualquier situacion laboral: dependencia, monotributo, plataforma, informal, sin trabajo o mixto. Aprende de cada conversacion y guarda perfiles nuevos localmente.

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

## Ejemplos reales — mayo 2026

### `/guita repartidor`

> "Trabajo en Pedidos Ya en moto propia, unas 12 horas por dia, 6 dias. Gano unos 20.000 por semana."

```
## Ingreso neto real — Repartidor de plataforma

Ingreso bruto:               $80.000/mes (4 semanas x $20.000)
  Combustible:               -$14.000/mes
  Mantenimiento moto:         -$6.000/mes
  Cuota unipersonal:         -$10.000/mes
  Internet celular:           -$2.000/mes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto estimado:               $48.000/mes
En dolares blue:             u$s 34/mes (blue $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            11.5% de una canasta

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso representa:       13.2% del SMVM

Sin cobertura automatica:
  - Sin ART: un accidente es gasto propio
  - Sin obra social
  - Sin seguro de desempleo
  - La cuenta puede suspenderse sin indemnizacion

Paso siguiente: si tenes prepaga (~$40.000-60.000/mes), tu neto
real cae a $0. Vale revisar cobertura publica como alternativa.

Fuente: Secretaria de Trabajo / INDEC / dolarapi.com
Dato al: mayo 2026
```

---

### `/guita sueldo dependencia`

> "Soy maestra de primaria, cobro $850.000 brutos."

```
## Ingreso neto real — Relacion de dependencia

Ingreso bruto:               $850.000/mes
  Aportes (~23%):            -$195.500
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto mensual:                $654.500/mes
+ Aguinaldo prorrateado:     +$70.833/mes
Neto real mensual:           $725.333/mes
En dolares blue:             u$s 518/mes (blue $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            1.74 canastas

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso equivale a:       2.0 salarios minimos

Incluye: jubilacion, obra social DOSEP/IOMA segun provincia,
ART, seguro de desempleo si te desvinculan.

Fuente: Secretaria de Trabajo / INDEC / dolarapi.com
Dato al: mayo 2026
```

---

### `/guita sueldo autonomo`

> "Soy plomero, trabajo por mi cuenta, facturo unos 1.700.000 por mes."

```
## Ingreso neto real — Autonomo (Monotributista)

Facturacion mensual:         $1.700.000
Facturacion anual proyectada: $20.400.000

Categoria estimada: C (tope $21.113.696/año — vigente desde feb 2026)
  Cuota mensual Cat. C:      -$15.616
  Gastos operativos
  (materiales, transporte):  -$380.000 (estimados, confirmar)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto estimado:               $1.304.384/mes
En dolares blue:             u$s 932/mes (blue $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            3.12 canastas

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso equivale a:       3.59 salarios minimos

Alerta: con inflacion del 3.05%/mes, en 2 meses tu facturacion
en pesos podria superar el tope de Cat. C ($21.1M). Conviene
revisar si subis precios para no quedar en Cat. D de golpe
(cuota salta a $25.495/mes).

Fuente: AFIP Monotributo / INDEC / dolarapi.com
Dato al: mayo 2026
```

---

### `/guita sueldo informal`

> "Cuido adultos mayores, cobro 400.000 por mes en negro entre tres familias."

```
## Ingreso neto real — Informal

Ingreso declarado:           $400.000/mes
  Descuentos formales:       ninguno
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto disponible:             $400.000/mes
En dolares blue:             u$s 286/mes (blue $1.400)

Canasta basica (referencia): $417.680 (INDEC, dic 2025)
Tu ingreso cubre:            95.8% de una canasta

SMVM vigente:                $363.000 (mayo 2026)
Tu ingreso equivale a:       1.10 salarios minimos

Sin cobertura:
  - Sin aportes jubilatorios (cada mes informal es un mes
    sin historia laboral previsional)
  - Sin obra social ni ART
  - Sin seguro de desempleo

Costo de regularizarte como Monotributo Cat. A:
  Cuota: $4.780/mes — neto quedaria en $395.220
  Incluye: jubilacion + obra social PAMI/OSDE basica

Paso siguiente: la diferencia entre informal y Cat. A es de
$4.780/mes. A cambio tenes cobertura de salud y estas
aportando para tu jubilacion.

Guarde tu situacion como nuevo perfil laboral para proximas consultas.

Fuente: AFIP Monotributo / INDEC / dolarapi.com
Dato al: mayo 2026
```

---

### `/guita gastos`

> "Alquiler 350.000, luz y gas 45.000, comida 180.000, transporte 35.000, internet y cel 18.000, prepaga 60.000."

```
## Estructura de gastos — mayo 2026

FIJOS NECESARIOS
  Alquiler:               $350.000/mes
  Prepaga:                 $60.000/mes
  Subtotal:               $410.000/mes

VARIABLES NECESARIOS
  Comida:                 $180.000/mes
  Transporte:              $35.000/mes
  Luz y gas:               $45.000/mes
  Internet y celular:      $18.000/mes
  Subtotal:               $278.000/mes

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOTAL GASTOS:             $688.000/mes

Proyeccion con inflacion 3.05%/mes (promedio ene-mar 2026):
  En 3 meses (ago 2026):  $753.000/mes
  En 6 meses (nov 2026):  $823.000/mes

El alquiler es el 50.9% de tus gastos totales — cualquier
aumento de alquiler tiene impacto alto en tu presupuesto.

Fuente: INDEC IPC — apis.datos.gob.ar
Dato al: marzo 2026 (ultimo disponible)
```

---

### `/guita runway`

> "Tengo u$s 3.000 ahorrados. Mis gastos son 688.000 por mes."

```
## Runway — cuanto aguantas

Ahorros:                  u$s 3.000
                          = $4.200.000 al blue ($1.400)
                          = $4.315.500 al MEP ($1.438)
Gastos mensuales:         $688.000/mes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Runway nominal:           6.1 meses (hasta noviembre 2026)
Runway real (c/inflacion 3.05%/mes): 5.6 meses (hasta octubre 2026)

La inflacion te quita 0.5 meses de runway real.
Tus gastos en noviembre van a ser ~$823.000/mes si no
ajustas nada — el mismo dinero alcanza menos cada mes.

Recomendacion: con 5-6 meses de colchon tenes margen para
buscar trabajo sin urgencia extrema. Conviene activar la
busqueda en el primer mes, no en el tercero.

Fuente: INDEC IPC / dolarapi.com
Dato al: mayo 2026
```

---

## Fuentes

- SMVM: [argentina.gob.ar/trabajo/consejodelsalario](https://www.argentina.gob.ar/trabajo/consejodelsalario)
- IPC / Inflacion: [apis.datos.gob.ar](https://apis.datos.gob.ar/series/api/series/?ids=148.3_INIVELNAL_DICI_M_26&limit=13&sort=desc&format=json)
- Canasta basica: [apis.datos.gob.ar](https://apis.datos.gob.ar/series/api/series/?ids=444.1_CANASTA_batotPampeana_0_0_26_47&limit=1&sort=desc&format=json)
- Dolar: [dolarapi.com](https://dolarapi.com/v1/dolares)
- Monotributo categorias: [afip.gob.ar/monotributo](https://www.afip.gob.ar/monotributo/categorias.asp)
- Seguro de desempleo: [anses.gob.ar](https://www.anses.gob.ar/prestacion/seguro-de-desempleo)
