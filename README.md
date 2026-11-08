

## 📋 Descripción General

Este repositorio contiene el proyecto de electrónica industrial que documenta la **validación experimental de dos topologías de regulación de voltaje** para alimentar una carga de **5 V hasta 1 A** desde una fuente rectificada de **8–12 VDC con rizado de línea a 120 Hz**.

| Topología | Regulador | Eficiencia medida | Rizado de salida |
|-----------|-----------|-------------------|------------------|
| Lineal | LM7805 | 49.7%* | < 10 mV pico (cumple) |
| Conmutada Buck | LM2596 | 82.8% | 40 mV pico (cumple) |

> *Valor que excede la cota física teórica (39.6%), indicando incertidumbre en la medición de corriente de entrada.

---

## 👥 Autores

- **Carlos Stiven Roa Martinez** — [carlos2201948@correo.uis.edu.co](mailto:carlos2201948@correo.uis.edu.co)
- **Juan David Ramírez** — [juan2181510@correo.uis.edu.co](mailto:juan2181510@correo.uis.edu.co)

**Institución:** Universidad Industrial de Santander (UIS) — Bucaramanga, Colombia  
**Escuela:** Ingenierías Eléctrica, Electrónica y de Telecomunicaciones (E3T)

---

## 🎯 Objetivos

### Objetivo General

Evaluar y comparar experimentalmente las topologías de regulación de voltaje **lineal (LM7805)** y **conmutada tipo buck (LM2596)** para alimentar una carga de 5 V hasta 1 A, con base en la regulación de voltaje, el rizado de salida y la eficiencia.

### Objetivos Específicos

- Medir la tensión de salida de cada regulador bajo carga.
- Medir y comparar el rizado de salida frente al requisito de **< 50 mV pico**.
- Calcular la eficiencia de cada topología.
- Contrastar mediciones con simulaciones en **LTspice**.
- Estimar la distribución de pérdidas del módulo LM2596 mediante análisis del ciclo de trabajo.
- Identificar ventajas y limitaciones de cada opción.

---

## 🔧 Especificaciones del Proyecto

| Parámetro | Valor requerido |
|-----------|-----------------|
| Tensión de entrada (Vin) | 8 – 12 VDC con rizado a 120 Hz |
| Tensión de salida (Vout) | 5 VDC |
| Corriente de salida (Iout) | Hasta 1 A |
| Rizado de salida (Vr) | < 50 mV pico |
| Topologías evaluadas | Lineal (LM7805) y conmutada buck (LM2596) |

---

## 🧪 Metodología

### Etapas de Validación

1. **Diseño y montaje** de la etapa de rectificación y filtrado común.
2. **Implementación y medición** del regulador lineal LM7805.
3. **Implementación y medición** del regulador conmutado buck LM2596.

### Instrumentación Utilizada

- **Multímetros:** UNI-T UT58A y UT33C+
- **Osciloscopio:** GWINSTEK GDS-1062 (acoplamiento AC)
- **Fuente de alimentación:** 12 V DC
- **Simulaciones:** LTspice

### Mejoras Implementadas

- **LM7805:** Capacitor electrolítico de 4700 μF / 25 V en entrada + disipador de mayor tamaño.
- **LM2596:** Dos capacitores de 4700 μF (entrada y salida) para reducir rizado.

---

## 📊 Resultados Principales

### LM7805 (Carga: 21 Ω)

| Parámetro | Valor |
|-----------|-------|
| Vin | 12 V |
| Vout (medido) | 4.75 V |
| Iin (medida) | 0.18 A* |
| Iout (calculada) | 0.226 A |
| Eficiencia | 49.7% |
| Cota teórica máxima | 39.6% |

> *Medida en escala de 10 A con resolución de 0.01 A — alta incertidumbre.

**Potencia disipada como calor:** ≈ 1.6 W

### LM2596 (Carga: 15 Ω)

| Parámetro | Valor |
|-----------|-------|
| Vin | 12 V |
| Vout (medido) | 5.02 V |
| Iin (medida) | 169.0 mA |
| Iout (calculada) | 0.335 A |
| Rizado de salida (pico) | 40 mV ✅ |
| Eficiencia | 82.8% |

**Pérdidas totales:** ≈ 0.35 W

### Comparación Directa

| Métrica | LM7805 | LM2596 |
|---------|--------|--------|
| Vout medido | 4.75 V | 5.02 V |
| Iin medida | 0.18 A | 0.169 A |
| Iout (Vout/RL) | 0.226 A | 0.335 A |
| Pout | 1.07 W | 1.68 W |
| Rizado de salida (pico) | — | 40 mV |
| Eficiencia | 49.7% (cota 39.6%) | 82.8% |
| Complejidad | Baja | Media (inductor y filtro) |

---

## 📉 Análisis de Pérdidas del LM2596 (Basado en Ciclo de Trabajo)

**Punto de operación:** Vin = 12 V, Vout = 5.02 V, RL = 15 Ω, L = 47 μH, f = 150 kHz, C1 = C2 = 100 μF

| Elemento | Mecanismo de pérdida | P (W) | % del total |
|----------|---------------------|-------|-------------|
| Interruptor (conducción) | Vsat·Isw,prom | 0.019 | 7.9% |
| Interruptor (conmutación) | ½·Vin·Iout·(tr+tf)·fs | 0.030 | 12.4% |
| Control interno (reposo) | Vin·Iq | 0.060 | 24.7% |
| Diodo Schottky D1 | VF·ID,prom | 0.075 | 30.8% |
| Inductor L1 | I²·DCR + núcleo | 0.031 | 12.9% |
| Capacitor entrada C1 | I²rms·ESR | 0.011 | 4.6% |
| Capacitor salida C2 | I²rms·ESR | 0.005 | 2.2% |
| LED indicador | VLED·ILED | 0.011 | 4.5% |
| **Pérdidas totales** | | **0.241** | **100%** |

**Eficiencia estimada:** 87.4%  
**Conclusión:** El diodo de rueda libre (31%) y el consumo interno del controlador (25%) dominan las pérdidas. **No es posible superar el 90% sin rectificación síncrona.**

---

## ✅ Conclusiones

1. **Regulación de voltaje:** Ambos reguladores mantienen la salida cerca de 5 V. El LM2596 entregó 5.02 V (cumple), mientras que el LM7805 entregó 4.75 V (≈5% por debajo del nominal).

2. **Eficiencia:** El LM2596 alcanzó 82.8%, mientras que el LM7805 dio 49.7% (valor que excede su cota física del 39.6%, indicando incertidumbre en la medición de corriente de entrada).

3. **Rizado de salida:** El LM2596 inicialmente no cumplía el requisito. Tras añadir dos capacitores de 4700 μF, el rizado se redujo a **80 mV pico a pico (40 mV pico)**, cumpliendo el requisito de < 50 mV pico.

4. **Selección de topología:** Para una carga de 5 V a 1 A desde 12 V, **el LM2596 es la opción más adecuada** por su eficiencia y menor disipación, con los capacitores de 4700 μF que mantienen su rizado dentro del requisito.

5. **Discrepancias simulación vs. medición:** Se explican por modelos ideales, valores de componentes y puntos de operación distintos a los reales, y efectos no ideales del protoboard.

---

## 📹 Evidencias en Video

<!-- ============================================================ -->
<!-- INSERTAR AQUÍ LOS ENLACES A LOS VIDEOS DE EVIDENCIA          -->
<!-- ============================================================ -->

### 1. Montaje y medición del LM7805



https://github.com/user-attachments/assets/833c5518-4beb-4623-a960-cfdfed4dd817



### 2. Montaje y medición del LM2596



https://github.com/user-attachments/assets/67e948f3-6915-4d66-a084-bb976c23ab5f



### 3. Simulaciones en LTspice

<!-- Reemplazar con enlace al video de las simulaciones -->


---

## 🖼️ Figuras del Proyecto
| LM7805 | LM2596 |
|:------:|:------:|
| <img src="https://github.com/user-attachments/assets/a1146125-4de9-4187-9172-d87ae454ec94" width="300" alt="Montaje del LM7805 en protoboard"> | <img src="https://github.com/user-attachments/assets/a463f2d7-e565-49ac-9d6d-56c3b2ea8253" width="300" alt="Montaje del LM2596 en protoboard"> |
| **Fig. 1.** Montaje del regulador lineal LM7805 con capacitor de 4700 μF y disipador. | **Fig. 2.** Montaje del regulador conmutado buck LM2596 con capacitores adicionales de 4700 μF. |




---

## 📁 Estructura del Repositorio

```text
📦 Proyecto-Electronica-Industrial
├── 📄 README.md
├── 📂 Informe/
│   └── Proyecto_de_electronica_industrial_v2.pdf
├── 📂 Simulaciones/
│   ├── LM7805.asc
│   └── LM2596.asc
├── 📂 Evidencias/
│   ├── 📂 Fotos/
│   │   ├── montaje_lm7805.jpg
│   │   ├── montaje_lm2596.jpg
│   │   └── ...
│   └── 📂 Videos/
│       └── (enlaces a videos)
└── 📂 Datos/
    └── mediciones.xlsx
