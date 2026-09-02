# Plataforma Didáctica para la Enseñanza de PLC

**Diseño de una plataforma didáctica para la enseñanza de PLC de la Escuela de Ingeniería Electrónica del Campus Tecnológico Local San Carlos**

Proyecto de Graduación para optar por el título de Ingeniera en Electrónica, grado de Licenciatura.
Instituto Tecnológico de Costa Rica — Campus Tecnológico Local San Carlos.

| | |
|---|---|
| **Autora** | María del Milagro Rojas Sánchez |
| **Asesor** | Ing. Pablo César Rodríguez Vargas |
| **Estado** | Etapa 1 concluida — estructura de soporte construida |
| **Última actualización** | 1 de setiembre de 2026 |

> **Nota sobre este repositorio.** Es un repositorio de seguimiento: documenta el estado del
> proyecto, no lo contiene. Las fuentes LaTeX del informe, la presentación, el guion y la guía
> del estudiante, junto con los modelos CAD y los proyectos del Arduino PLC IDE, se mantienen
> fuera de aquí. Ver [Documentación del proyecto](#documentación-del-proyecto).

---

## Resumen

El programa de Ingeniería Electrónica del Campus Tecnológico Local San Carlos no cuenta con
infraestructura dedicada para la enseñanza de automatización industrial con PLC. Este proyecto
diseña y construye una plataforma didáctica de bajo costo basada en el **Arduino Opta**, un
micro-PLC industrial programable con los lenguajes de la norma IEC 61131-3, que permite a
estudiantes y docentes desarrollar competencias prácticas de programación y de conexión de
señales industriales a 24 V DC.

La plataforma se diseñó bajo dos criterios que la distinguen de un banco de pruebas cerrado:
es **modular** —los componentes pueden reubicarse o ampliarse sin rehacer la estructura— y es
**reutilizable**, en el sentido de que el núcleo de control es independiente de los dispositivos
de campo que se le conecten, de modo que cada práctica puede definir su propio conjunto de
sensores y actuadores sin intervenir el montaje interno.

---

## Meta y objetivos

**Meta.** Que estudiantes y docentes del Campus Tecnológico Local San Carlos cuenten con un
recurso experimental funcional para la enseñanza y el aprendizaje de automatización industrial
con PLC.
*Indicador:* la plataforma es utilizada en al menos una actividad académica con estudiantes del Campus.

**Objetivo general.** Diseñar una plataforma didáctica modular para la enseñanza de PLC,
utilizando la tecnología Arduino Opta y dispositivos de instrumentación de bajo costo, que
garantice la integridad eléctrica del sistema y permita el desarrollo de competencias prácticas
en automatización industrial.

**Objetivos específicos**

1. **Diseñar el montaje del sistema de control** que aloje el PLC y sus módulos de expansión,
   exponiendo terminales de señal accesibles para la conexión segura de dispositivos de campo a 24 V DC.
   Incluye código de colores por tipo de señal, etiquetado completo, verificación de continuidad
   y separación física entre las terminales de alimentación y las de señal.
2. **Verificar el funcionamiento** de la plataforma mediante experimentos que demuestren la
   adquisición de señales de campo y la ejecución de lógica de control programada, con una guía
   de práctica de laboratorio estructurada y validación de cada tipo de señal.
3. **Comparar el costo-beneficio** de la plataforma frente a PLC industriales convencionales
   (Siemens, Festo y/o Allen-Bradley), en costos de hardware y licencias y en capacidades técnicas equivalentes.

---

## Arquitectura de la solución

La plataforma se articula en tres componentes diferenciados:

- **Núcleo de control.** El Arduino Opta, sus módulos de expansión montados en riel DIN, la
  alimentación de 24 V DC y el cableado interno, empotrados sobre un panel único y rígido de
  acrílico transparente que se presenta inclinado hacia el estudiante. El acceso se limita a las
  borneras de señal, lo que protege el hardware durante la experimentación y estandariza el punto
  de conexión sea cual sea la práctica.
- **Dispositivos de campo.** Sensores, actuadores e instrumentación de bajo costo que el estudiante
  conecta a las borneras. Al estar desacoplados del núcleo de control, pueden variarse, sustituirse
  o ampliarse sin intervenir el montaje.
- **Interfaz de acoplamiento con circuitos de 5 V.** Tarjeta optoacopladora PC817 de cuatro canales,
  alimentada de forma independiente a 5 V por cable USB. Permite integrar a un sistema gobernado por
  PLC los circuitos electrónicos que el estudiante construye en los demás laboratorios de la carrera,
  de modo que la electrónica de baja tensión y el control industrial dejen de ser prácticas separadas.

El enfoque es análogo al principio de separación entre gabinete de control y tablero de campo de las
instalaciones industriales reales, lo que añade valor formativo más allá de la programación: el
estudiante interioriza desde temprano la lógica de protección, identificación y segmentación de
sistemas de control.

---

## Hardware

### Arduino PLC Starter Kit (AKX00051)

Desarrollado por Arduino Education en colaboración con Finder.

**Arduino Opta WiFi** — micro-PLC industrial:

- 8 entradas digitales (0–24 V DC), compatibles NPN y PNP
- 4 salidas tipo relé (contacto seco, NO, hasta 250 VAC / 5 A)
- WiFi 802.11 b/g/n + Bluetooth LE + Ethernet
- Puerto USB-C para programación; puerto AUX lateral para encadenar módulos
- Programable en Arduino IDE y en Arduino PLC IDE (LD, FBD, ST, IL, SFC)

**Módulos de expansión** (encadenados por bus AUX):

| Módulo | Función | Terminales |
|--------|---------|------------|
| **DIN Simul8** | 8 switches manuales que simulan señales digitales de 24 V hacia I1–I8. Permite probar lógica sin sensores físicos. | 8 canales digitales |
| **DIN Celsius** | Entradas de termopar tipo K/J/T para las prácticas de temperatura. | Canales de termopar |
| **AFX00007** (Opta Ext A0602) | Módulo analógico: 6 entradas programables (0–10 V / 4–20 mA / PT100), 2 salidas analógicas (0–10 V o 4–20 mA), 4 salidas PWM. | I1–I6, O1–O2, P1–P4 |

Accesorios: fuente de 24 V DC, cables de bus AUX y cableado general.

### Señales disponibles para los dispositivos de campo

**Entradas al PLC**

- **I1–I8** — digitales 24 V, NPN o PNP (inductivos, fotoeléctricos, pulsadores)
- **AI1–AI4** — analógicas 0–10 V o 4–20 mA (presión, nivel, caudal)
- **AI5–AI6** — analógicas + PT100 de 2 o 3 hilos
- **TC+/TC−** — termopar tipo K/J/T por el módulo DIN Celsius

**Salidas del PLC**

- **O1–O4** — relés de contacto seco NO, 250 VAC / 5 A (electroválvulas, contactores, pilotos)
- **AO1–AO2** — analógicas 0–10 V o 4–20 mA (válvulas proporcionales, variadores)
- **P1–P4** — PWM (motores DC, dimmers)

### Código de colores del cableado interno

| Color | Señal |
|-------|-------|
| Rojo | +24 V DC |
| Negro | GND (0 V) |
| Verde | Entradas digitales I1–I8 |
| Amarillo | Salidas relé O1–O4 |
| Azul claro | Entradas analógicas AI1–AI6 |
| Naranja | Salidas analógicas AO1–AO2 |
| Blanco | Salidas PWM P1–P4 |

---

## Estructura de soporte

Es el primer producto físico del proyecto y la base sobre la que se instala el panel con el
sistema de control.

### Selección del material

Se evaluaron cinco alternativas de material estructural frente a cinco parámetros derivados de
los requerimientos:

| Parámetro | Perfil Al. T-slot | Madera / MDF | Acero | Impresión 3D | PVC |
|---|---|---|---|---|---|
| Rigidez y resistencia-peso | Alta | Media | Muy alta, con alto peso | Baja | Baja |
| Modularidad y reconfigurabilidad | Muy alta | Nula | Baja | Media | Nula |
| Facilidad de fabricación y ensamble | Alta | Alta | Baja | Media | Media |
| Costo y disponibilidad local | Media | Muy alta | Alta | Alta | Muy alta |
| Acabado, seguridad y presentación | Alta | Media | Baja | Media | Media |

Se seleccionó el **perfil de aluminio ranurado 2020 T-slot**: fue la única alternativa que
satisface simultáneamente los dos requerimientos irrenunciables del proyecto, la modularidad y la
facilidad de fabricación con los recursos disponibles en el Campus. La madera y el PVC se
descartaron por carecer de todo mecanismo de fijación reconfigurable. Su sobrecosto quedó acotado
por el propio diseño: la geometría se optimizó hasta requerir solo seis perfiles, por
**₡30 358,77** según la cotización CB2026050402 de CRCibernética.

### Geometría y plan de corte

El diseño se modeló de forma paramétrica en **Autodesk Fusion 360**. El elemento que gobierna la
geometría es la inclinación del panel: los dos perfiles que sostienen el acrílico se disponen a
**70° respecto de la base**, valor que acerca el panel a la vertical lo suficiente para que los
indicadores del Opta y la identificación de las borneras queden a la vista del estudiante de pie,
sin desplazar el centro de masa fuera de la base.

| Elemento | Longitud final | Perfiles de origen | ¿Requiere corte? |
|---|---|---|---|
| Montantes inclinados a 70° (soporte del panel) | 500 mm | 2 | No |
| Largueros longitudinales de la base | 500 mm | 2 | No |
| Soportes laterales inferiores | 250 mm | 1 (seccionado a la mitad) | Sí |
| Soporte trasero | 435 mm | 1 (recortado 6,5 cm) | Sí |
| **Total de perfiles de 500 mm** | | **6** | |

### Piezas de unión

Las uniones inferiores no pueden resolverse con las escuadras normalizadas del sistema T-slot,
porque en ellas concurren tres perfiles y, en las frontales, uno llega con la inclinación de 70°.
Se diseñaron por eso **cuatro piezas propias en Fusion 360** —dos delanteras con angulación y dos
codos traseros— fabricadas por **impresión 3D en PLA**. El material es suficiente porque el
esfuerzo en las uniones es fundamentalmente de compresión y no de tracción: la carga del panel se
transmite a la base a través de los propios perfiles.

### Montaje

- **14 tuercas T M5×16**, elemento normalizado del sistema T-slot: la tuerca entra en la ranura,
  gira un cuarto de vuelta hasta quedar retenida por los labios del perfil y recibe el tornillo
  desde el exterior. La unión se reposiciona a lo largo del perfil sin practicar ningún agujero.
- **Arandelas de acrílico negro cortadas a láser** (diseño en RDWorks) entre la cabeza de cada
  tornillo y la pieza impresa. Distribuyen la presión sobre una superficie mayor del PLA, evitando
  la marca por aplastamiento, y unifican el acabado.
- **Panel de acrílico transparente de 5 mm.** El espesor se determinó a partir de la geometría del
  perfil: la apertura de la ranura del 20×20 mm es de 6 mm, de modo que la lámina encaja y queda
  retenida por los labios, con holgura para deslizarla durante el montaje. El panel se sostiene por
  la propia ranura, sin escuadras ni tornillería adicional.

---

## Interfaz de acoplamiento con circuitos de 5 V

Se evaluaron tres alternativas del proveedor nacional consultado:

| Alternativa | Aislamiento | Rangos | Resultado |
|---|---|---|---|
| **4N25** en DIP-6 | 5 300 V rms | Componente individual | Descartada: exige diseñar y fabricar la placa de soporte |
| **Módulo MOSFET** con aislamiento óptico | — | Entrada 3–20 V, salida 5–36 V, 10 A, PWM ≤ 20 kHz | Descartada: es etapa de potencia, sobredimensionada como interfaz de señal |
| **Tarjeta PC817 de 4 canales** | 5 000 V rms, CTR ≥ 50 % | Entrada 3,6–24 V, salida 3,6–30 V | **Seleccionada** |

La tarjeta PC817 cubre de forma directa tanto el nivel del controlador (24 V) como el de la lógica
de 5 V, sin electrónica intermedia. Trae borneras atornillables en ambos lados, puentes de
configuración de tierra por canal y cuatro perforaciones de sujeción.

---

## Estado del proyecto

**Etapa 1 — Diseño y construcción de la estructura** ✅

- [x] Arquitectura de la plataforma definida
- [x] Hardware del kit identificado y caracterizado
- [x] Requerimientos de la estructura de soporte derivados
- [x] Selección del material estructural (5 alternativas evaluadas)
- [x] Geometría optimizada y plan de corte definido
- [x] Piezas de unión diseñadas e impresas en PLA
- [x] Modelo de ensamble verificado en Fusion 360
- [x] Montaje físico de la estructura concluido
- [x] Selección de la interfaz de acoplamiento a 5 V (3 alternativas evaluadas)

**Etapa 2 — Verificación funcional** ⏳

- [ ] Montaje del sistema de control sobre el panel de acrílico
- [ ] Cableado interno con código de colores, etiquetado y verificación de continuidad
- [ ] Lógica de control en Ladder Diagram, Function Block Diagram y Structured Text
- [ ] Validación con al menos un dispositivo de campo por tipo de señal (digital, analógica, temperatura)
- [ ] Valores medidos en bornera, documentados
- [ ] Guía de práctica de laboratorio terminada

**Etapa 3 — Comparación costo-beneficio** ⏳

- [ ] Tabla comparativa de costos de hardware y licencias frente a Siemens, Festo y/o Allen-Bradley
- [ ] Análisis de características técnicas equivalentes que respalde la comparabilidad

---

## Presupuesto

Total estimado: **₡475 745,92**

| Rubro | Fuente | Total (₡) |
|---|---|---|
| Kit Arduino PLC Starter Kit AKX00051 (431 EUR) | Arduino Store | 223 470,00 |
| Módulo analógico de expansión AFX00007 (237 EUR) | Arduino Store | 122 882,00 |
| Perfilería aluminio 2020 T-slot, escuadras, tuercas T, tornillos M5 y conectores | CRCibernética, cot. CB2026050402 | 30 358,77 |
| Rieles DIN y ductos ranurados 25×25 mm, borneras WAGO push-button 2,5 mm², cable de control 1 mm² 105 °C, etiquetas SPB-01, cinta 3M Super 33, prensacables PG09 | Electro Beyco, cot. N° 196164 | 99 035,15 |
| Arduino PLC IDE | Arduino | Gratuito |
| **Total estimado** | | **475 745,92** |

Los montos en colones de los rubros Arduino son aproximados y deben actualizarse al tipo de cambio
vigente al momento de la adquisición. Las cotizaciones nacionales tienen validez hasta las fechas
indicadas en cada documento.

> **Pendiente.** La tarjeta optoacopladora PC817 (≈ $5,95) entró al diseño después de elaborado el
> presupuesto y **aún no tiene fila propia** en esta tabla; el total todavía no la incluye.

---

## Documentación del proyecto

Ninguno de estos documentos vive en este repositorio.

| Documento | Descripción |
|---|---|
| **Informe final** | 67 páginas. Marco teórico, procedimiento metodológico, descripción de la solución y presupuesto. Sigue el formato oficial de la Escuela. |
| **Presentación de defensa** | Beamer, 25 diapositivas. Corresponde todavía al anteproyecto. |
| **Guion de presentación** | Libreto para la defensa, 20 minutos. |
| **Guía del estudiante** | Guía de laboratorio para aprender a programar el Opta. Cubre **ST, LD y FBD**; IL y SFC quedan fuera del alcance. En redacción. |

### Guía del estudiante

Documento en IEEEtran a una columna, en español, con los nombres de la interfaz en inglés porque el
Arduino PLC IDE solo existe en ese idioma. Secciones: entorno del PLC IDE, fundamentos comunes
(POU, variables, tareas y ciclo de scan), Ladder Diagram, Structured Text, Function Block Diagram,
depuración y verificación, prácticas de laboratorio y tres apéndices.

Se redacta con **texto enteramente original**. El manual del Arduino PLC IDE distribuido con el
programa no tiene licencia Creative Commons y no puede copiarse ni traducirse; el manual web de
`docs.arduino.cc` es CC BY-SA 4.0, y reutilizarlo impondría *share-alike* sobre la guía. Escribir
original evita esa obligación y conserva la autoría completa de los ejercicios. Ambos manuales se
citan en la bibliografía.

---

## Notas técnicas

1. **Las salidas del Opta son contacto seco.** El relé no alimenta la carga: la energía viene de
   los +24 V externos, en serie con la carga.
2. **El DIN Simul8 opera de forma independiente.** Sus switches van directo a I1–I8 sin pasar por
   las borneras de campo.
3. **El bus AUX es solo comunicación.** Cada módulo necesita su propia alimentación de 24 V/GND.
   El AFX00007 en particular no se alimenta por el bus.
4. **Máximo 5 módulos** encadenables al Opta.
5. **El Arduino PLC IDE solo corre en Windows 10+ de 64 bits.**
6. **El `Oscilloscope` del PLC IDE es una herramienta de software**, un trazador de variables
   interno, no un instrumento de banco. No debe confundirse con el osciloscopio físico que se usa
   sobre las terminales de 24 V.

---

## Derechos

El contenido de este repositorio y los documentos del proyecto son obra de la autora, salvo lo
citado explícitamente. Las marcas Arduino, Opta y los nombres de los módulos pertenecen a
Arduino S.r.l. El manual del Arduino PLC IDE es propiedad de Arduino S.r.l., todos los derechos
reservados, y no se redistribuye aquí.
