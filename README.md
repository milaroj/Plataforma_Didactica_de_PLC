# Plataforma Didáctica PLC — Arduino Opta
**Proyecto de automatización industrial con fines educativos y demostrativos**

---

## Resumen del proyecto

Se está diseñando una **plataforma didáctica de aprendizaje** basada en el Arduino Opta (micro-PLC industrial). El sistema se divide en dos plataformas físicas separadas:

- **Plataforma 1 — Gabinete PLC:** contiene todo el hardware de control, cerrado y protegido. Solo expone borneras y ventanas de acceso controlado al usuario.
- **Plataforma 2 — Tablero de campo:** contiene sensores y actuadores industriales a 24V DC, conectados manualmente a la Plataforma 1 mediante cableado por las borneras expuestas. (Pendiente de diseño.)

La Plataforma 1 está pensada para ser **reutilizable** — cualquier docente o estudiante puede conectarle su propia plataforma de campo sin modificar el gabinete.

---

## Kit de hardware disponible

**Arduino PLC Starter Kit (AKX00051)** — desarrollado por Arduino Education en colaboración con Finder. Incluye:

### Unidad principal
- **Arduino Opta WiFi** — micro-PLC con:
  - 8 entradas digitales (0–24V DC), compatibles NPN y PNP
  - 4 salidas tipo relé (contacto seco, NA, hasta 250VAC/5A por relé)
  - WiFi 802.11 b/g/n + Bluetooth LE + Ethernet
  - Puerto USB-C para programación
  - Puerto AUX lateral para encadenar módulos de expansión
  - Programable en Arduino IDE y PLC IDE (LADDER, LBD, Structured Text)

### Módulos de expansión (encadenados por bus AUX)
El orden en el riel DIN es fijo: **Opta → SIMUL8 → A0602**

| Módulo | Función | Terminales |
|--------|---------|------------|
| **SIMUL8** | 8 switches manuales que simulan señales digitales de 24V hacia I1–I8 del Opta. Herramienta didáctica — permite probar lógica sin sensores físicos. | 8 canales digitales |
| **A0602 (AFX00007)** | Módulo analógico completo. 6 entradas programables (**I1–I2:** 0–10V / 4–20mA / **PT100**; **I3–I6:** solo 0–10V / 4–20mA, sin PT100), 2 salidas analógicas O1–O2 (0–10V o 4–20mA), 4 salidas PWM P1–P4. | AI1–AI6, AO1–AO2, P1–P4 |

> **Nota:** El módulo **CELSIUS** (entradas de termopar tipo K/J/T) forma parte del kit pero se ubicará en la **Plataforma 2**, junto a los sensores de campo, no en el gabinete.

### Accesorios incluidos
- Fuente de alimentación 24V DC (conecta al tomacorriente 110VAC)
- Cables para bus AUX entre módulos
- Cables de conexión generales

---

## Distribución física de las borneras de fábrica (para cablear Riel 1)

Esto es lo que realmente ves en el cuerpo del Opta y del A0602 — necesario antes de bajar cable hacia las borneras de paso de Riel 3. **No confundir el bloque físico (el conector que se puede desenchufar) con el número de señal (O1, I1, etc.); no siempre coinciden 1 a 1.**

### Arduino Opta (unidad principal)

| Fila | Bloque | Terminales |
|------|--------|-----------|
| Superior izquierda | Alimentación | `+  +  −  −` (12–24V DC, dos pares para puentear) |
| Superior derecha | Entradas | `I1 I2 I3 I4 I5 I6 I7 I8` (0–24V DC) |
| Inferior izquierda | **Salidas relé 1–2** | 4 tornillos: común + NA de O1, común + NA de O2 — rotulado "Output 10A" |
| Inferior derecha | **Salidas relé 3–4** | 4 tornillos: común + NA de O3, común + NA de O4 — rotulado "Output 10A" |

**Por qué "O1" parece uno solo pero hay 2 bloques de salida:** el Opta tiene 4 relés (O1–O4), pero el conector físico de salidas viene partido en **2 piezas extraíbles**: una lleva O1+O2, la otra lleva O3+O4. Dentro de cada bloque, cada relé individual (O1, por ejemplo) usa **2 tornillos propios** (común y normalmente abierto) — no dos bloques. Es decir: 4 salidas × 2 tornillos = 8 tornillos, repartidos en 2 conectores de 4 tornillos cada uno.

### Módulo A0602 / AFX00007

| Fila | Bloque | Terminales |
|------|--------|-----------|
| Superior izquierda | Alimentación propia | `+  +  −  −` (12–24V DC, independiente del Opta) |
| Superior derecha | Entradas I1/I2 | `+I1− IC1   +I2− IC2` (0–10V/4–20mA/PT100) |
| Media izquierda | PWM | `P1+  P2+  V_PWM` |
| Media derecha | PWM | `−  P3+  P4+` |
| Inferior izquierda | **I3, I4 + salida O1** | `+I3− +I4− +O1−` (6 tornillos) |
| Inferior derecha | **I5, I6 + salida O2** | `+I5− +I6− +O2−` (6 tornillos) |

**Aquí está la verdadera fuente de la confusión:** en el A0602, O1 y O2 **no comparten bloque** — cada salida analógica vive en su propio conector, mezclada con dos entradas del mismo lado (O1 junto a I3/I4; O2 junto a I5/I6). Por eso "O1" aparece una única vez (un solo par +/−, en el bloque izquierdo) y sin embargo el módulo tiene 2 bloques de salida en esa fila: no es que O1 tenga 2 bloques, es que **cada bloque aporta una salida distinta** (uno da O1, el otro da O2). No hay que buscar un segundo par de terminales para O1 en el otro bloque — ese bloque es el de O2.

---

## Diseño del Gabinete — Plataforma 1

### Dimensiones
**500mm ancho × 600mm alto × 160mm profundidad**

### Distribución interna — 3 rieles DIN
```
┌─────────────────────────────────────────────┐
│  CANALETA SUPERIOR                          │
├─────────────────────────────────────────────┤
│  RIEL 1 — Componentes activos               │
│  [Fuente 24V][Opta][A0602][ libre ][ libre ] │
│  (no accesible al usuario)                  │
├─────────────────────────────────────────────┤
│  CANALETA MEDIA 1                           │
├─────────────────────────────────────────────┤
│  RIEL 2 — Interacción usuario               │
│  [SIMUL8 ←switches→][8 pares +24V/GND]      │
│  ↑ VENTANA 1 en tapa frontal                │
├─────────────────────────────────────────────┤
│  CANALETA MEDIA 2                           │
├─────────────────────────────────────────────┤
│  RIEL 3 — Borneras de paso señales campo    │
│  [I1–I8+GND][O1–O4][AI1–AI6][AO+PWM]       │
│  ↑ VENTANA 2 en tapa frontal                │
├─────────────────────────────────────────────┤
│  Prensaestopa 110VAC │ USB-C extensor        │
└─────────────────────────────────────────────┘
```

### Ventanas en tapa frontal
- **Ventana 1** (~alineada con Riel 2): expone los switches del SIMUL8 (accionables con el dedo) y los tornillos externos de las borneras de alimentación 24V/GND.
- **Ventana 2** (~alineada con Riel 3): expone los tornillos externos de las borneras de paso de señales. El usuario conecta su cable de campo aquí con destornillador plano.

### Cableado interno
- **Canaleta ranurada 25×25mm** en bordes y entre rieles
- Cables bajan de componentes → canaleta → borneras de paso
- Al cablear Riel 1 → Riel 3, seguir la distribución física de la sección anterior (bloque por bloque), no solo la lista lógica de señales — así se evita landear un cable en el bloque equivocado.

#### Código de colores del cableado interno
| Color | Señal |
|-------|-------|
| Rojo | +24V DC |
| Negro | GND (0V) |
| Verde | Entradas digitales I1–I8 |
| Amarillo | Salidas relé O1–O4 |
| Azul claro | Entradas analógicas AI1–AI6 |
| Naranja | Salidas analógicas AO1–AO2 |
| Blanco | Salidas PWM P1–P4 |

---

## Borneras de paso — Especificación

**UK-2.5B / AWG 14** — bornera de paso de tornillo estándar, 2.5mm², 20A/600V.
- 100 grises para señales de campo (Riel 3)
- Kit Dinkle DK4N rojo/negro AWG 14 para alimentación (Riel 2)

### Lista de compra accesorios
| Ítem | Cantidad |
|------|----------|
| Borneras UK-2.5B grises | 35 uds |
| Kit Dinkle DK4N rojo/negro | 8 pares |
| End stops / topes finales | 12 uds |
| Divisores de grupo | 16 uds |
| Puentes de cortocircuito (jumper bar) | 4 tiras |
| Marcadores de cable numerados (clip) | 1 juego |

---

## Mapa completo de terminales expuestos

### Riel 2 — Ventana 1
| Grupo | Terminales | Color |
|-------|-----------|-------|
| SIMUL8 | S1–S8 (switches directos con el dedo) | — |
| Alimentación campo | +24V / GND × 8 pares | Rojo / Negro |

### Riel 3 — Ventana 2
| Grupo | Terminales | Cant. | Color |
|-------|-----------|-------|-------|
| Entradas digitales | I1–I8 + GND×2 | 10 | Verde |
| Salidas relé | O1–O4 (COM+NA por relé) | 8 | Amarillo |
| Entradas analógicas | AI1–AI6 + GND | 7 | Azul |
| Salidas analógicas | AO1, AO2 + GND | 3 | Naranja |
| Salidas PWM | P1–P4 + GND | 5 | Blanco |
| **Total** | | **~33** | |

---

## Señales disponibles para Plataforma 2

### Entradas al PLC (sensores → Opta)
- **I1–I8:** digitales 24V — NPN o PNP (inductivos, fotoeléctricos, pulsadores)
- **AI1–AI2:** analógicas 0–10V, 4–20mA **o PT100** (únicos dos canales del A0602 con entrada PT100)
- **AI3–AI4:** analógicas 0–10V o 4–20mA (presión, nivel, caudal)
- **AI5–AI6:** analógicas 0–10V o 4–20mA
- **TC+/TC−:** termopar tipo K/J/T — leídos por el módulo CELSIUS ubicado en Plataforma 2, que retorna datos al Opta por bus AUX extendido

### Salidas del PLC (actuadores ← Opta)
- **O1–O4:** relés contacto seco NA, 250VAC/5A por relé (electroválvulas, contactores, pilotos)
- **AO1–AO2:** analógicas 0–10V o 4–20mA (válvulas proporcionales, variadores)
- **P1–P4:** PWM (motores DC, dimmers)

---

## Estado del proyecto

- [x] Arquitectura de dos plataformas definida
- [x] Hardware del kit identificado completamente
- [x] Diseño gabinete: 3 rieles DIN, ventanas, canaletas
- [x] Borneras de paso especificadas
- [x] Mapa completo de terminales
- [x] Distribución física de terminales de fábrica (Opta y A0602)
- [ ] **PENDIENTE: Diseño Plataforma 2 (sensores y actuadores)**
- [ ] Pendiente: Diagrama eléctrico cableado interno
- [ ] Pendiente: Lista de materiales completa
- [ ] Pendiente: Programación PLC (ejemplos didácticos)

---

## Notas técnicas importantes

1. **Salidas Opta = contacto seco** — el relé no alimenta la carga. La energía viene de los +24V externos en serie con la carga.
2. **SIMUL8 opera independiente** — sus switches van directo a I1–I8 sin pasar por borneras de campo.
3. **Bus AUX = solo comunicación** — cada módulo necesita su propia alimentación 24V/GND.
4. **Máximo 5 módulos** encadenables al Opta.
5. **PLC IDE** solo funciona en Windows 10+ 64 bits.
6. **El bloque físico de un conector no siempre agrupa la misma señal.** En el Opta, un bloque de salidas trae 2 relés completos (O1+O2 o O3+O4). En el A0602, en cambio, un bloque de salida trae **una sola** salida analógica mezclada con dos entradas del mismo lado (O1 con I3/I4; O2 con I5/I6). Verificar siempre contra la sección "Distribución física de las borneras de fábrica" antes de atornillar un cable, no solo contra el mapa lógico de señales.
7. **Solo I1 e I2 del A0602 aceptan PT100.** I3–I6 aceptan 0–10V/4–20mA únicamente — si se necesita una segunda entrada de temperatura resistiva, debe ir en I1 o I2, no en cualquier canal libre.
