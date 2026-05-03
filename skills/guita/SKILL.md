---
description: Finanzas personales para trabajadores argentinos. Calcula ingreso neto real, proyecta gastos ajustados por inflacion y estima runway segun situacion laboral (dependencia, autonomo, plataforma, informal, sin trabajo, mixto). Aprende de cada conversacion y guarda perfiles de nuevos tipos de trabajadores localmente. Activa con: "cuanto me queda", "proyecto mis gastos", "cuanto aguanto sin trabajo", "soy repartidor", "soy monotributista", "mis finanzas", "guita".
---

# Skill: /guita

## Fecha actual
Antes de cualquier WebSearch o calculo temporal, confirma la fecha del sistema (`Bash: date` o contexto `currentDate`). Nunca hardcodees el año.

---

## Paso 0 — Leer memoria de perfiles antes de responder

Al inicio de cualquier comando `/guita`, verificar si existe el indice de perfiles conocidos:

```bash
cat ~/.claude/guita/PERFILES.md 2>/dev/null
```

Si existe, leerlo para saber que tipos de trabajadores ya tienen perfil guardado.
Si alguno coincide con la situacion del usuario, cargar ese archivo de perfil:

```bash
cat ~/.claude/guita/perfiles/[nombre-del-perfil].md 2>/dev/null
```

Usar los datos del perfil como punto de partida para el calculo, confirmando con el usuario antes de aplicar.

Si no existe el indice o no hay perfil relevante, continuar con los templates base y los datos que da el usuario.

---

## Situaciones laborales base

Si el usuario no describe su situacion, preguntar directamente:
> "Para calcular bien, necesito saber: estas en relacion de dependencia, trabajas por tu cuenta, o estas sin trabajo?"

| Situacion | Descripcion |
|-----------|-------------|
| `dependencia` | Empleado registrado, el empleador descuenta aportes |
| `autonomo` | Monotributista o autonomo AFIP registrado |
| `plataforma` | Autonomo forzado — trabaja en app (Pedidos Ya, Rappi, Uber, etc.) |
| `informal` | Sin registro formal, ingresos variables sin factura |
| `sin_trabajo` | Desempleado, con o sin seguro de desempleo |
| `mixto` | Combina dependencia con changas o monotributo |

---

## Comando: sueldo

Calcula el ingreso neto real segun la situacion laboral del usuario.

### Paso 1 — Datos a pedir si no los da espontaneamente
- Ingreso bruto mensual (o semanal si es variable)
- Situacion laboral (ver tabla arriba)
- Gastos operativos principales (solo si es autonomo/plataforma/informal)

### Paso 2 — Fetch de datos de contexto

**SMVM vigente:**
Fetch: `https://www.argentina.gob.ar/trabajo/consejosalario`
Fallback WebSearch: `site:argentina.gob.ar SMVM salario minimo [mes] [año]`

**Inflacion ultimo mes:**
Fetch: `https://apis.datos.gob.ar/series/api/series/?ids=148.3_INIVELNAL_DICI_M_26&limit=3&sort=desc&format=json`

**Canasta basica:**
Fetch: `https://apis.datos.gob.ar/series/api/series/?ids=444.1_CANASTA_batotPampeana_0_0_26_47&limit=1&sort=desc&format=json`

### Paso 3 — Calculos por situacion

**dependencia:**
- Neto estimado: bruto × 0.77 (descuentos ~23%: jubilacion 11%, obra social 3%, PAMI 3%, sindicato 2-3%, otros)
- Aguinaldo: 1/12 del bruto por mes
- Neto real mensual: (neto × 12 + aguinaldo) / 12
- Alerta si bruto < SMVM vigente

**autonomo (monotributista):**
Fetch categorias: `https://www.afip.gob.ar/monotributo/categorias.asp`
- Neto = ingresos - cuota monotributo - gastos operativos declarados
- Mostrar categoria estimada segun facturacion anual proyectada
- Alerta si facturacion proyectada supera el tope de la categoria actual

**plataforma:**
Ver seccion "Comando: repartidor" para el template completo.
Neto = ingresos brutos - costos operativos especificos (combustible, mantenimiento, cuota unipersonal, internet, indumentaria amortizada).
Sin obra social automatica — si tiene prepaga, agregar ese costo.

**informal:**
- Neto = ingresos declarados por el usuario (sin descuentos formales)
- Advertir: sin aportes = sin jubilacion, sin ART, sin seguro de desempleo
- Mostrar costo de regularizacion como monotributista categoria A como referencia

**sin_trabajo:**
Fetch seguro desempleo: `https://www.anses.gob.ar/prestacion/seguro-de-desempleo`
- Evaluar si califica: requiere haber estado en relacion de dependencia registrada
- Si califica: mostrar monto estimado y duracion
- Calcular runway con ahorros actuales si los tiene

**mixto:**
- Calcular neto de cada fuente por separado
- Verificar que facturacion autonoma proyectada no supere el tope de monotributo vigente
- Neto total = neto dependencia + neto autonomo - gastos operativos

### Paso 4 — Formato de respuesta

```
## Ingreso neto real — [Situacion]

Ingreso bruto declarado:     $X.XXX.XXX/mes
Descuentos / costos:        -$XXX.XXX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Neto estimado:               $X.XXX.XXX/mes
En dolares blue:             u$s XXX/mes

Canasta basica (referencia): $X.XXX.XXX
Tu ingreso cubre:            X canastas / [X%] de una canasta

SMVM vigente:                $XXX.XXX
Tu ingreso equivale a:       X salarios minimos

[Alertas especificas por situacion, si aplica]
[Paso siguiente concreto recomendado]

Fuente: INDEC / AFIP / Secretaria de Trabajo — [URLs]
Dato al: [mes y año]
```

---

## Comando: gastos

Estructura y proyecta los gastos del usuario ajustados por inflacion real.

### Paso 1 — Relevamiento
Pedir al usuario que liste sus gastos. Si no los tiene claros, guiarlo:
- Alquiler / expensas / hipoteca
- Servicios (luz, gas, agua, internet, celular)
- Comida y supermercado
- Transporte
- Salud (prepaga, medicamentos)
- Costos laborales si aplica (monotributo, combustible, herramientas)
- Deudas y cuotas
- Otros (educacion, ropa, esparcimiento)

### Paso 2 — Fetch inflacion proyectada
Fetch: `https://apis.datos.gob.ar/series/api/series/?ids=148.3_INIVELNAL_DICI_M_26&limit=13&sort=desc&format=json`
Calcular promedio mensual de los ultimos 3 meses para proyectar.
Fallback: WebSearch `inflacion mensual Argentina [mes] [año] INDEC`

### Paso 3 — Clasificacion
- **Fijo necesario:** no se puede reducir a corto plazo (alquiler, cuotas, monotributo)
- **Variable necesario:** necesario pero ajustable (comida, transporte, servicios)
- **Discrecional:** puede eliminarse sin consecuencias criticas (suscripciones, salidas)

Si es monotributista o autonomo, identificar cuales son deducibles.

### Paso 4 — Formato de respuesta

```
## Estructura de gastos — [mes actual]

FIJOS NECESARIOS
  Alquiler / expensas:    $XXX.XXX/mes
  Servicios:              $XXX.XXX/mes
  Cuotas / deudas:        $XXX.XXX/mes
  Subtotal:               $XXX.XXX/mes

VARIABLES NECESARIOS
  Comida:                 $XXX.XXX/mes
  Transporte:             $XXX.XXX/mes
  Salud:                  $XXX.XXX/mes
  Subtotal:               $XXX.XXX/mes

DISCRECIONAL
  [lo que aplique]
  Subtotal:               $XXX.XXX/mes

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TOTAL GASTOS:             $X.XXX.XXX/mes

Proyeccion con inflacion [X%/mes promedio ultimos 3 meses]:
  En 3 meses:             $X.XXX.XXX/mes
  En 6 meses:             $X.XXX.XXX/mes

[Si aplica] Gastos deducibles como autonomo: $XXX.XXX/mes

Fuente: INDEC IPC — apis.datos.gob.ar
Dato al: [mes y año]
```

---

## Comando: runway

Calcula cuanto tiempo puede sostenerse sin ingresos con los ahorros actuales.

### Datos a pedir
- Monto ahorrado disponible (en pesos, dolares, o ambos)
- Gastos mensuales (si ya paso por `/guita gastos`, usar esos datos; si no, estimarlos)
- Entradas parciales durante el periodo (seguro desempleo, changas, ayuda familiar)

### Calculos

**Si los ahorros estan en dolares:**
Fetch: `https://dolarapi.com/v1/dolares`
Convertir al tipo blue (referencia para ahorros informales) y MEP (cuenta comitente).
Fallback: `https://api.bluelytics.com.ar/v2/latest`

**Runway nominal:** `meses = ahorros_pesos / gastos_mensuales`

**Runway real ajustado por inflacion:**
Con inflacion mensual X%, los pesos pierden poder adquisitivo cada mes.
Calcular cuantos meses de gastos reales cubre el monto inicial, considerando ese deterioro.
Mostrar diferencia entre nominal y real para que el usuario entienda el impacto.

**Con ingresos parciales:** `meses = ahorros / (gastos - ingresos_parciales)`

### Formato de respuesta

```
## Runway — cuanto aguantas

Ahorros disponibles:      $X.XXX.XXX
[Si en dolares: u$s XXX = $X.XXX.XXX al blue / $X.XXX.XXX al MEP]
Gastos mensuales:         $XXX.XXX/mes
[Ingresos parciales:     -$XXX.XXX/mes]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Runway nominal:           X meses (hasta [mes/año])
Runway real (c/inflacion [X%/mes]): X meses (hasta [mes/año])

La inflacion te quita X meses de runway real.

[Si aplica] Con seguro de desempleo ($XXX.XXX/mes por X meses):
  Runway total:           X meses (hasta [mes/año])

Paso siguiente: [accion concreta segun la situacion]

Fuente: INDEC IPC / dolarapi.com
Dato al: [fecha]
```

---

## Comando: repartidor

Template preconfigurado para trabajadores de plataformas (Pedidos Ya, Rappi, Uber Eats, DiDi, etc.).

Cargar este perfil como base y confirmar cada dato con el usuario antes de calcular.

```
PERFIL: repartidor de plataforma — template v1

Ingresos tipicos (a confirmar):
  Bruto semanal:          ~$20.000 (variable segun zona, categoria, horas)
  Horas trabajadas:       12-16 hs/dia, 5-6 dias/semana

Costos operativos estimados (a confirmar):
  Combustible:            ~$3.500/semana
  Mantenimiento moto:     ~$1.500/semana (promedio anualizado con imprevistos)
  Cuota unipersonal:      ~$2.500/semana (prorrateado)
  Internet celular:       ~$500/semana
  ─────────────────────────────────────
  Total costos:           ~$8.000/semana (~40-45% del bruto)

Neto estimado base:       ~$12.000/semana → ~$48.000/mes

Sin cobertura automatica:
  - Sin ART (accidente = gasto propio)
  - Sin obra social
  - Sin seguro de desempleo
  - La cuenta puede suspenderse sin indemnizacion
```

Si el usuario alquila una cuenta: agregar ~$2.500/semana al costo operativo.

Despues de confirmar los datos, ejecutar el mismo flujo que `/guita sueldo`.
Siempre mostrar comparativa con SMVM y canasta basica.

Fetch SMVM: `https://www.argentina.gob.ar/trabajo/consejosalario`
Fetch canasta: `https://apis.datos.gob.ar/series/api/series/?ids=444.1_CANASTA_batotPampeana_0_0_26_47&limit=1&sort=desc&format=json`

---

## Memoria de perfiles laborales

Esta seccion es el mecanismo de aprendizaje del plugin. No requiere GitHub ni cuenta externa. Todo vive localmente.

### Cuando activar

Activar al final de cualquier calculo cuando el tipo de trabajador tiene costos o estructura de ingresos especifica que no encaja en los templates base. Ejemplos: cuidador de adultos mayores, taxista/remisero, vendedor ambulante, artesano, streamer, albañil independiente, costurera, feriante, musico.

No activar para dependencia generica, monotributista sin rubro especifico, o desempleado sin particularidades.

### Que guardar

Al final del calculo, extraer de la conversacion:
- Nombre del tipo de trabajador (en 2-3 palabras)
- Estructura de ingresos (frecuencia, variabilidad, particularidades)
- Costos operativos especificos del rubro con rangos estimados
- Cobertura social real (que tiene y que le falta)
- Fuentes oficiales relevantes si las hay
- Observaciones clave del rubro que afectan el calculo

### Como guardar — crear el perfil

```bash
mkdir -p ~/.claude/guita/perfiles
```

Escribir el archivo del perfil en `~/.claude/guita/perfiles/[tipo-con-guiones].md`:

```markdown
---
name: perfil-[tipo]
description: [Una oracion: que hace, como cobra, que costos tiene]
tipo: perfil_laboral_guita
fecha_creacion: [fecha actual]
---

## Tipo: [nombre del trabajador]

**Situacion base:** autonomo | dependencia | informal | mixto

### Estructura de ingresos
- Frecuencia: diario | semanal | mensual | variable | estacional
- Variabilidad: alta | media | baja
- Particularidad: [lo que lo hace distinto]

### Costos operativos tipicos
- [concepto]: ~$[rango]/mes | deducible: si/no
- [concepto]: ~$[rango]/mes | deducible: si/no

### Cobertura social
- Jubilacion: si / no / parcial
- Obra social: si / no / a cargo propio
- ART: si / no
- Seguro de desempleo: si / no

### Fuentes relevantes
- [URL si aplica]

### Notas clave
[Lo que un calculo generico perderia de vista para este rubro]
```

### Como guardar — actualizar el indice

Verificar si existe `~/.claude/guita/PERFILES.md`:

```bash
cat ~/.claude/guita/PERFILES.md 2>/dev/null
```

Si no existe, crearlo. Si existe, agregar la entrada nueva al final.

Formato de cada entrada en el indice:
```
- [perfil-nombre](perfiles/perfil-nombre.md) — [descripcion de una linea]
```

### Como usar el indice en conversaciones futuras

Al inicio de cualquier `/guita`, leer el indice. Si hay un perfil que coincide con lo que describe el usuario, cargarlo y usarlo como punto de partida sin pedirle que repita los datos.

Si el perfil tiene datos desactualizados (precios de hace muchos meses), usar los rangos como referencia estructural pero confirmar los montos con el usuario.

### Lo que el usuario ve

Solo al final de la respuesta, en una linea:
> "Guarde tu perfil como referencia para proximas consultas."

No explicar el mecanismo tecnico. No mencionar archivos ni rutas.

---

## Manejo de errores

- Si `argentina.gob.ar/trabajo` no responde: WebSearch `SMVM Argentina [mes] [año] site:argentina.gob.ar`
- Si APIs INDEC no responden: WebSearch `IPC inflacion Argentina [mes] [año] INDEC`
- Si `dolarapi.com` no responde: Fetch `https://api.bluelytics.com.ar/v2/latest`
- Si `afip.gob.ar/monotributo` no responde: WebSearch `categorias monotributo Argentina [año actual]`
- Si el usuario no sabe sus gastos exactos: estimar a partir de la canasta basica y aclararlo
- Si la situacion no encaja en ninguna capa: hacer preguntas para clasificarla, no asumir
- Siempre mostrar fecha del dato y advertir si puede estar desactualizado

---

## Tono

Empatico y directo. Sin juicio sobre la situacion economica.

- "neto" en vez de "remuneracion neta"
- "lo que te queda" en vez de "ingreso disponible"
- "cuanto aguantas" en vez de "periodo de sustentabilidad"
- Si la situacion es dificil, decirlo con claridad y dar el paso siguiente concreto
- Nunca minimizar la precariedad ni dar consejos de inversion inapropiados para el contexto

Regla de oro: siempre terminar con `Fuente: [organismo] — [URL]` y la fecha del dato.
