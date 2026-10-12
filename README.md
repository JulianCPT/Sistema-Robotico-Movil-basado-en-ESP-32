<div align="center">

[![Header](https://capsule-render.vercel.app/api?type=waving&color=0:2C5364,50:203A43,100:0F2027&height=220&section=header&text=Robot%20M%C3%B3vil%20Aut%C3%B3nomo%20con%20ESP32&fontSize=40&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Br%C3%BAjula%20digital%2C%20obst%C3%A1culos%2C%20colores%20y%20dashboard%20en%20Streamlit&descAlignY=58&descSize=18)](#)

*Universidad Militar Nueva Granada · Ingeniería Mecatrónica · Proyecto final*

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&duration=3000&pause=800&color=3AAFFF&center=true&vCenter=true&width=640&lines=%22Br%C3%BAjula+%E2%86%92+rumbo+actual+vs+deseado%22;%22Sensores+%E2%86%92+obst%C3%A1culos+y+colores%22;%22ESP32+%E2%86%92+control+aut%C3%B3nomo%22;%22Dashboard+en+tiempo+real%22)](#)

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](#)
[![Streamlit](https://img.shields.io/badge/Streamlit-Dashboard-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](#)
[![ESP32](https://img.shields.io/badge/ESP32-En%20desarrollo-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](#)
[![Status](https://img.shields.io/badge/estado-acad%C3%A9mico-6E40C9?style=for-the-badge)](#)

</div>

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## ✨ ¿Qué hace este proyecto?

Un **robot móvil autónomo basado en ESP32** que recorre una pista, **mantiene un rumbo** con una brújula digital, **detecta y esquiva obstáculos**, **identifica los colores** del recorrido y se comunica de forma inalámbrica con una **aplicación web en Streamlit**. Desde la app se puede controlar el robot de forma manual y ver en tiempo real la **dirección polar actual** frente a la **dirección polar deseada**. El sistema también se conecta con un **chatbot** en tiempo real.

La meta final es superar **tres niveles de dificultad**, con pistas cada vez más complejas.

| 🧭 Orientación | 🚧 Obstáculos | 🎨 Color | 📡 Comunicación |
| --- | --- | --- | --- |
| MPU6050 (giro) + QMC5883L (norte) con filtro complementario | Sensores de distancia al frente y a los lados | Sensor RGB con iluminación propia, clasificador de 3 a 6 colores | ESP32 ↔ dashboard Streamlit y chatbot |

### 📊 Estado del proyecto

| Módulo | Estado |
| --- | --- |
| Requerimientos funcionales y no funcionales | ✅ Listo |
| Diagramas de bloques por módulo | ✅ Listo |
| Selección y justificación de materiales | ✅ Listo (compras pendientes, ver tabla) |
| Chasis 4WD con motores TT | ✅ Disponible |
| Modelos CAD y planos mecánicos | 🚧 En desarrollo |
| Esquemas eléctricos | 🚧 En desarrollo |
| Firmware del ESP32 (brújula, obstáculos, color) | 🚧 En desarrollo |
| Aplicación Streamlit (control manual y gráfico polar) | 🚧 En desarrollo |
| Conexión con el chatbot | 🚧 Pendiente |
| Pruebas en los 3 niveles de pista | 🚧 Pendiente |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 📑 Contenido

- [🏁 Meta final: tres niveles](#-meta-final-tres-niveles)
- [📋 Requerimientos](#-requerimientos)
- [📐 Arquitectura general](#-arquitectura-general)
- [🧱 Diagramas de bloques por módulo](#-diagramas-de-bloques-por-módulo)
- [🛒 Selección y justificación de materiales](#-selección-y-justificación-de-materiales)
- [📁 Estructura del repositorio](#-estructura-del-repositorio)
- [⚙️ Requisitos](#️-requisitos)
- [▶️ Cómo correrlo](#️-cómo-correrlo)
- [📡 Formato de datos (propuesta)](#-formato-de-datos-propuesta)
- [🔌 Integración con el ESP32](#-integración-con-el-esp32)
- [🧠 Conceptos clave](#-conceptos-clave)
- [🛠️ Solución de problemas](#️-solución-de-problemas)
- [👤 Autor](#-autor)

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🏁 Meta final: tres niveles

El sistema autónomo debe superar tres pistas. **El diseño se dimensiona desde ahora para el nivel 3**, el más exigente.

| Nivel | Obstáculos mínimos | Colores diferentes | Forma de la pista |
| --- | --- | --- | --- |
| 1️⃣ Primero | 4 | 3 | Curva suave y alargada |
| 2️⃣ Segundo | 6 | 4 | Estrella de 5 puntas |
| 3️⃣ Tercero | 8 | 6 | Forma irregular con curvas cerradas |

> 💡 Que el nivel 3 pida **6 colores** es lo que más pesa en la elección del sensor de color, y las **curvas cerradas** son lo que más pesa en el diseño mecánico y en el control de rumbo.

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 📋 Requerimientos

### Requerimientos funcionales

| ID | Requerimiento |
| --- | --- |
| RF-01 | El robot se desplaza de forma autónoma sobre la pista siguiendo una dirección deseada |
| RF-02 | Mide su orientación con una brújula digital (dirección polar actual) |
| RF-03 | Corrige su rumbo comparando la dirección actual con la deseada |
| RF-04 | Detecta obstáculos al frente y a los lados y los evita |
| RF-05 | Identifica los colores de la pista (3, 4 o 6 según el nivel) |
| RF-06 | Ejecuta una acción distinta según el color detectado |
| RF-07 | Envía telemetría (rumbo, distancia, color, estado) en tiempo real |
| RF-08 | Recibe comandos de control manual desde la app |
| RF-09 | La app muestra en tiempo real la dirección polar actual y la deseada |
| RF-10 | Se conecta con el chatbot en tiempo real |
| RF-11 | Tiene modo manual y modo autónomo, con parada de emergencia desde la app |

### Requerimientos no funcionales

> ⚠️ Los valores numéricos son **metas de diseño** y se validan con pruebas.

| ID | Categoría | Requerimiento |
| --- | --- | --- |
| RNF-01 | Precisión | Error de rumbo menor a ±5° tras calibrar la brújula |
| RNF-02 | Precisión | Clasificación de color correcta en al menos 90 % de las lecturas |
| RNF-03 | Tiempo de respuesta | Detección de obstáculo y reacción en menos de 200 ms |
| RNF-04 | Latencia | Telemetría hacia la app con retardo menor a 500 ms |
| RNF-05 | Autonomía | Batería suficiente para completar el nivel 3 (mínimo 30 min) |
| RNF-06 | Robustez | Funcionar con variaciones de luz (sensor de color con iluminación propia) |
| RNF-07 | Mecánico | Radio de giro compatible con las curvas cerradas del nivel 3 y ancho menor al de la pista |
| RNF-08 | Escalabilidad | Hardware dimensionado para el nivel 3 (8 obstáculos, 6 colores) |
| RNF-09 | Seguridad | Parada segura si se pierde la comunicación |
| RNF-10 | Mantenibilidad | Componentes comerciales, fáciles de reemplazar, y código modular |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 📐 Arquitectura general

```
                    ┌──────────────┐
  [Batería] ──────► │   POTENCIA   │──► 5 V / 3.3 V (lógica y sensores)
                    │  (regulador) │──► Vbat (motores)
                    └──────────────┘

 SENSADO                 CONTROL                 ACTUACIÓN
┌────────────┐        ┌─────────────┐        ┌──────────────┐   ┌───────────┐
│ Brújula    │──I2C──►│             │──PWM──►│ Driver motor │──►│ 4 motores │
│ Obstáculos │──────► │    ESP32    │        └──────────────┘   │    TT     │
│ Color      │──I2C──►│             │                           └───────────┘
└────────────┘        └──────┬──────┘
                             │ Wi-Fi / Bluetooth / ESP-NOW
                      ┌──────▼──────┐
                      │ COMUNICACIÓN│──► App Streamlit + Chatbot
                      └─────────────┘
```

| Módulo | Función |
| --- | --- |
| **Potencia** | Alimenta motores y lógica por separado |
| **Control** | ESP32 con máquina de estados |
| **Sensado** | Brújula, obstáculos y color |
| **Locomoción** | Driver y 4 motores TT, giro diferencial por lados |
| **Comunicación** | Telemetría y comandos con la app |
| **Interfaz** | Dashboard en Streamlit y chatbot |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🧱 Diagramas de bloques por módulo

### 1. Potencia

```
[Batería] ──► [Interruptor] ──► [Fusible] ──┬──► [Driver motores] ──► Motores (Vbat)
                                            └──► [Buck 5 V] ──┬──► ESP32 (VIN)
                                                              └──► Sensores (3.3 V del ESP32)
             Condensadores de desacople en el driver y en el ESP32
```

### 2. Control (ESP32)

```
 Entradas                      Proceso                       Salidas
┌─────────────────┐     ┌──────────────────────┐     ┌──────────────────┐
│ Rumbo actual    │     │ 1. Leer sensores     │     │ PWM motores      │
│ Distancias      │────►│ 2. Estimar rumbo     │────►│ Telemetría       │
│ Color           │     │ 3. Decidir (FSM)     │     │ Estado del robot │
│ Comandos app    │     │ 4. Control de rumbo  │     └──────────────────┘
└─────────────────┘     └──────────────────────┘
```

> La decisión se plantea como **máquina de estados**: avanzar, evitar obstáculo, girar, reaccionar a color y parada.

### 3. Orientación (brújula digital)

```
MPU6050 (giro) ──┐
                 ├─I2C─► ESP32 ──► Filtro complementario ──► Rumbo actual (°)
QMC5883L (norte)─┘                 (corrige la deriva)             │
                                                                   ▼
                                       Error = Rumbo deseado − Rumbo actual
```

### 4. Detección de obstáculos

```
Sensor izq. ─┐
Sensor frente┼──► ESP32 ──► Filtrado ──► ¿Distancia < umbral? ──► Evento "obstáculo"
Sensor der. ─┘                                                    + lado (izq/frente/der)
```

### 5. Detección de color

```
LED blanco ──► Superficie de pista ──► TCS34725 ──I2C──► ESP32
                                                          │
                                      RGB → HSV → Clasificador (3, 4 o 6 colores)
                                                          │
                                                Color detectado ──► Acción asociada
```

### 6. Locomoción

```
Orden (velocidad, giro) ─► ESP32 ─PWM+DIR─► Driver ─► Lado izquierdo (2 motores TT)
                                                  └─► Lado derecho  (2 motores TT)
```

### 7. Comunicación e interfaz

```
ESP32 ◄──Wi-Fi / BT / ESP-NOW──► App Streamlit ◄──► Chatbot
  │                                   │
  └─ Telemetría: rumbo, color,        └─ Control manual, gráfico polar
     distancia, estado                   (actual vs deseada)
```

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🛒 Selección y justificación de materiales

| Módulo | Componente | Estado | Justificación |
| --- | --- | --- | --- |
| Control | **ESP32 DevKit** | ✅ Disponible | Wi-Fi y Bluetooth integrados, doble núcleo, suficientes pines y bajo costo |
| Locomoción | **Chasis 4WD + 4 motores TT** | ✅ Disponible | Base mecánica económica y estable; motores en pares por lado |
| Driver | **Driver de motores** (L298N o TB6612FNG) | ✅ Disponible | Controla los dos lados con PWM y dirección |
| Potencia | **Batería + regulador buck a 5 V** | ✅ Disponible | Separa alimentación de motores y lógica para evitar reinicios del ESP32 |
| Orientación (giro) | **MPU6050** | ✅ Disponible | Giroscopio preciso para giros y corrección de rumbo; no se afecta por campos magnéticos |
| Orientación (norte) | **QMC5883L** | 🛒 Por comprar | El MPU6050 no tiene magnetómetro; este da el norte absoluto y corrige la deriva |
| Obstáculos | **VL53L0X ×3** o **HC-SR04 ×3** | 🛒 Por definir | ToF es compacto y preciso en curvas; el ultrasónico es más barato pero más ancho y ruidoso |
| Color | **TCS34725 + LED propio** | 🛒 Por comprar | Lectura RGB estable con luz controlada, necesaria para separar hasta 6 colores |

### 🧭 Por qué MPU6050 + QMC5883L y no uno solo

| Opción | Ventaja | Limitación |
| --- | --- | --- |
| MPU6050 solo | Ya está disponible, excelente en giros | Sin magnetómetro: el rumbo es relativo y acumula deriva |
| QMC5883L solo | Da el norte absoluto | Los motores DC lo perturban y responde lento |
| **MPU6050 + QMC5883L** | Giro rápido y norte absoluto, ambos por I2C | Requiere un filtro complementario y calibración |

> 💡 Los motores TT **no tienen encoder**, así que no hay odometría: el rumbo es la referencia principal del robot. Conviene montar el QMC5883L **lejos de motores y cables de potencia** (por ejemplo, en un mástil elevado).

> ⚠️ Con el chasis 4WD el giro es **tipo skid steer**: las ruedas patinan al girar. Para las curvas cerradas del nivel 3 conviene girar casi sobre el propio eje (un lado adelante y el otro atrás).

> ⚠️ El MPU6050 debe estar **quieto unos segundos al encender** para calibrar el sesgo del giroscopio; si no, el rumbo se desvía solo.

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 📁 Estructura del repositorio

> 📝 Estructura **propuesta**; se completa a medida que avance el proyecto.

```
Robot-Movil-ESP32/
├── app_streamlit.py            # Dashboard: control manual y gráfico polar
├── requirements.txt            # Dependencias de Python
├── esp32_firmware/             # Firmware del ESP32 (pendiente)
│   └── robot_movil/
│       └── robot_movil.ino
├── docs/
│   ├── imagenes/               # Diagramas, capturas y fotos
│   ├── cad/                    # Modelos CAD y planos mecánicos (pendiente)
│   └── esquemas/               # Esquemas eléctricos (pendiente)
└── README.md
```

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## ⚙️ Requisitos

[![](https://skillicons.dev/icons?i=python,arduino,cpp&theme=dark)](#)

- ✅ Python 3.10+
- ✅ Un navegador (la app corre en un computador; el celular debe estar en la **misma red Wi-Fi**)
- 🚧 Arduino IDE con el paquete de placas **esp32 by Espressif Systems**
- 🚧 Librerías de Arduino para los sensores (MPU6050, QMC5883L, sensor de distancia y TCS34725)

**Instalación de dependencias de Python:**

```
pip install -r requirements.txt
```

### 📦 Librerías de Python (propuesta)

| Librería | Para qué sirve | Dónde se usa |
| --- | --- | --- |
| **Streamlit** | Dashboard web con control y actualización automática | `app_streamlit.py` |
| **Plotly** | Gráfico polar con la dirección actual y la deseada | `app_streamlit.py` |
| **Pandas** | Tablas e historial de telemetría | `app_streamlit.py` |
| **Requests** | Leer el JSON del ESP32 por HTTP (si se usa Wi-Fi) | `app_streamlit.py` |

**`requirements.txt` propuesto**

```
streamlit>=1.37
plotly
pandas
requests
```

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## ▶️ Cómo correrlo

> 🚧 Esta sección se completa cuando el firmware y la app estén listos.

1. **Instala** las dependencias (`pip install -r requirements.txt`).
2. **Carga el firmware** en el ESP32 desde Arduino IDE.
3. **Lanza el dashboard:**

```
streamlit run app_streamlit.py
```

> No lo ejecutes con `python app_streamlit.py`: Streamlit debe lanzarse con `streamlit run`.

4. **Para verlo en el celular** (misma red Wi-Fi):

```
streamlit run app_streamlit.py --server.address 0.0.0.0
```

Luego abre `http://IP-DEL-COMPUTADOR:8501` en el celular.

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 📡 Formato de datos (propuesta)

JSON que el ESP32 envía varias veces por segundo. Los campos son una **propuesta inicial** (valores de ejemplo):

```json
{
  "modo": "autonomo",
  "rumbo_actual": 87.5,
  "rumbo_deseado": 90.0,
  "distancia_izq": 42,
  "distancia_frente": 18,
  "distancia_der": 55,
  "color": "rojo",
  "estado": "evitando_obstaculo"
}
```

| Campo | Significado |
| --- | --- |
| `modo` | `manual` o `autonomo` |
| `rumbo_actual` | Orientación medida, en grados (0–360) |
| `rumbo_deseado` | Orientación objetivo, en grados |
| `distancia_*` | Distancia a obstáculos por lado, en cm |
| `color` | Último color detectado |
| `estado` | Estado actual de la máquina de estados |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🔌 Integración con el ESP32

La comunicación con la app puede hacerse por tres vías. **Aún no está definida**, y la propuesta inicial es Wi-Fi:

| Opción | Ventajas | Limitaciones |
| --- | --- | --- |
| **Wi-Fi** (HTTP, WebSocket o MQTT) | Se integra directo con Streamlit, que corre en un computador | Requiere red compartida |
| **Bluetooth** | No necesita red | Alcance corto y más difícil de integrar con Streamlit |
| **ESP-NOW** | Muy baja latencia | Necesita un segundo ESP32 conectado por USB como puente |

**Conexión de sensores I2C** (pines por defecto de la ESP32 DevKit):

| Señal | Pin ESP32 |
| --- | --- |
| SDA | GPIO 21 |
| SCL | GPIO 22 |

| Dispositivo I2C | Dirección típica |
| --- | --- |
| MPU6050 | `0x68` |
| QMC5883L | `0x0D` |
| TCS34725 | `0x29` |
| VL53L0X (si se usa) | `0x29` por defecto, **conflicto** con el TCS34725 |

> ⚠️ El VL53L0X y el TCS34725 comparten la dirección `0x29`. Con varios VL53L0X se necesita el pin **XSHUT** para reasignar direcciones o un multiplexor **TCA9548A**.

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🧠 Conceptos clave

| Concepto | Qué significa en este proyecto |
| --- | --- |
| **Rumbo / dirección polar** | Ángulo del robot respecto al norte, en grados |
| **Filtro complementario** | Combina el giroscopio (rápido, pero con deriva) y el magnetómetro (lento, pero absoluto) |
| **Skid steer** | Giro por diferencia de velocidad entre lados; las ruedas patinan |
| **Máquina de estados (FSM)** | El robot siempre está en un estado (avanzar, evitar, girar...) y cambia según los eventos |
| **Control de rumbo** | Se calcula el error entre rumbo deseado y actual y se corrige con los motores |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 🛠️ Solución de problemas

| Problema | Posible causa | Solución |
| --- | --- | --- |
| El rumbo se desvía solo | Giroscopio sin calibrar | Mantener el robot quieto al encender para calibrar el sesgo |
| El rumbo salta cuando andan los motores | Interferencia magnética | Alejar el QMC5883L de motores y cables de potencia |
| El ESP32 se reinicia al arrancar los motores | Caída de tensión | Alimentar motores y lógica por separado y agregar condensadores de desacople |
| No aparece un sensor I2C | Dirección repetida o cableado | Revisar con un escáner I2C y resolver conflictos de dirección |
| Colores mal clasificados | Luz ambiente cambiante | Usar el LED propio, apantallar el sensor y recalibrar umbrales |
| El dashboard no abre | Se ejecutó con `python` | Usar `streamlit run app_streamlit.py` |

![](https://capsule-render.vercel.app/api?type=rect&color=0:0F2027,100:203A43&height=3&section=header)

## 👤 Autor

**Julian Camilo Perez Torres** · Estudiante de Ingeniería Mecatrónica · Universidad Militar Nueva Granada

[![GitHub](https://img.shields.io/badge/GitHub-JulianCPT-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/JulianCPT)

[![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:0F2027,50:203A43,100:2C5364&height=120&section=footer)](#)
