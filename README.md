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
  - 4 salidas tipo relé (contacto seco, NO, hasta 250VAC/5A)
  - WiFi 802.11 b/g/n + Bluetooth LE + Ethernet
  - Puerto USB-C para programación
  - Puerto AUX lateral para encadenar módulos de expansión
  - Programable en Arduino IDE y PLC IDE (LADDER, LBD, Structured Text)

### Módulos de expansión (encadenados por bus AUX)
El orden en el riel DIN es fijo: **Opta → SIMUL8 → A0602**

| Módulo | Función | Terminales |
|--------|---------|------------|
| **SIMUL8** | 8 switches manuales que simulan señales digitales de 24V hacia I1–I8 del Opta. Herramienta didáctica — permite probar lógica sin sensores físicos. | 8 canales digitales |
| **A0602 (AFX00007)** | Módulo analógico completo. 6 entradas programables (0–10V / 4–20mA / PT100), 2 salidas analógicas (0–10V o 4–20mA), 4 salidas PWM. | I1–I6, O1–O2, P1–P4 |

> **Nota:** El módulo **CELSIUS** (entradas de termopar tipo K/J/T) forma parte del kit pero se ubicará en la **Plataforma 2**, junto a los sensores de campo, no en el gabinete.

### Accesorios incluidos
- Fuente de alimentación 24V DC (conecta al tomacorriente 110VAC)
- Cables para bus AUX entre módulos
- Cables de conexión generales

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
| Salidas relé | O1–O4 (COM+NO por relé) | 8 | Amarillo |
| Entradas analógicas | AI1–AI6 + GND | 7 | Azul |
| Salidas analógicas | AO1, AO2 + GND | 3 | Naranja |
| Salidas PWM | P1–P4 + GND | 5 | Blanco |
| **Total** | | **~33** | |

---

## Señales disponibles para Plataforma 2

### Entradas al PLC (sensores → Opta)
- **I1–I8:** digitales 24V — NPN o PNP (inductivos, fotoeléctricos, pulsadores)
- **AI1–AI4:** analógicas 0–10V o 4–20mA (presión, nivel, caudal)
- **AI5–AI6:** analógicas + PT100 2/3 hilos
- **TC+/TC−:** termopar tipo K/J/T — leídos por el módulo CELSIUS ubicado en Plataforma 2, que retorna datos al Opta por bus AUX extendido

### Salidas del PLC (actuadores ← Opta)
- **O1–O4:** relés contacto seco NO, 250VAC/5A (electroválvulas, contactores, pilotos)
- **AO1–AO2:** analógicas 0–10V o 4–20mA (válvulas proporcionales, variadores)
- **P1–P4:** PWM (motores DC, dimmers)

---

## Estado del proyecto

- [x] Arquitectura de dos plataformas definida
- [x] Hardware del kit identificado completamente
- [x] Diseño gabinete: 3 rieles DIN, ventanas, canaletas
- [x] Borneras de paso especificadas
- [x] Mapa completo de terminales
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
