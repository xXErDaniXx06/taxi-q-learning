# 🚕 Agente Autónomo con Q-Learning (Taxi-v3)

![Python](https://img.shields.io/badge/Python-3.13-blue?style=for-the-badge&logo=python)
![Gymnasium](https://img.shields.io/badge/Gymnasium-RL_Environment-orange?style=for-the-badge)
![NumPy](https://img.shields.io/badge/NumPy-Math-013243?style=for-the-badge&logo=numpy)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter)

Este repositorio contiene la implementación y documentación analítica de un agente de Inteligencia Artificial basado en **Aprendizaje por Refuerzo (Reinforcement Learning)**. El proyecto resuelve el entorno estocástico `Taxi-v3` empleando una aproximación tabular mediante el algoritmo **Q-Learning**, partiendo desde una inicialización de conocimiento cero (tabula rasa).

> Proyecto desarrollado como demostración técnica de Procesos de Decisión de Markov (MDP) y optimización de políticas mediante la Ecuación de Bellman.

---

## 📌 1. Descripción del Entorno

El agente interactúa con un entorno logístico simulado en una cuadrícula de 5x5. Su objetivo es aprender a recoger a un pasajero en una de las cuatro ubicaciones de partida ($R, G, Y, B$) y transportarlo a una ubicación de destino diferente en el menor número de pasos posible.

> [!NOTE]
> **Espacio de Estados ($S$):** El entorno define un espacio de estados finito de cardinalidad **500**. Esto resulta del producto cartesiano de:
> * 25 coordenadas espaciales del taxi $(x, y)$.
> * 5 estados del pasajero (en una de las 4 paradas, o a bordo del vehículo).
> * 4 ubicaciones de destino finales.

> [!NOTE]
> **Espacio de Acciones ($A$):** Espacio discreto con **6 acciones**:
> * `0`: Moverse al Sur
> * `1`: Moverse al Norte
> * `2`: Moverse al Este
> * `3`: Moverse al Oeste
> * `4`: Recoger pasajero (*Pickup*)
> * `5`: Dejar pasajero (*Dropoff*)

---

## 🧠 2. Fundamentación Matemática

El "cerebro" del agente es una Q-Table (matriz de $500 \times 6$) que mapea el valor esperado de tomar una acción $a$ estando en un estado $s$. 

El algoritmo utiliza la **Actualización por Diferencia Temporal (TD)**, implementando iterativamente la **Ecuación de Bellman** tras cada paso del agente:

$$Q(s, a) \leftarrow Q(s, a) + \alpha \left[ R + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]$$

### Hiperparámetros Utilizados:
* **Tasa de Aprendizaje ($\alpha = 0.1$):** Determina en qué medida la nueva información sobrescribe la anterior. Se ha optado por un valor bajo para asegurar una convergencia estable frente a la estocasticidad del entorno.
* **Factor de Descuento ($\gamma = 0.6$):** Pondera la importancia de las recompensas futuras frente a las inmediatas. Un valor de 0.6 incentiva al agente a buscar el objetivo final (+20 puntos) sin generar miopía a corto plazo.
* **Tasa de Exploración ($\epsilon = 0.1$):** Se emplea una política **Epsilon-Greedy** durante el entrenamiento. El agente confía en su matriz el 90% de las veces, pero toma una decisión totalmente estocástica el 10% de las iteraciones para garantizar la exploración topológica completa del entorno.

> [!TIP]
> **Ajuste Fino:** Si el agente se estanca en mínimos locales durante tus pruebas, prueba a implementar un $\epsilon$ dinámico (Decaying Epsilon) que comience en 1.0 y disminuya gradualmente a 0.01 a lo largo de los episodios.

---

## ⚙️ 3. Estructura del Proyecto

```text
taxi-q-learning/
│
├── notebooks/
│   └── agente_taxi.ipynb    # Código principal: Entrenamiento, matemáticas y métricas
│
├── .gitignore               # Exclusión de entornos virtuales y cachés
├── requirements.txt         # Manifiesto de dependencias exactas
└── README.md                # Documentación del proyecto (Estás aquí)

## 🚀 4. Instalación y Ejecución Local

> [!IMPORTANT]
> Se recomienda encarecidamente utilizar un **entorno virtual** (`venv`, `conda` o `pipenv`) para instalar las dependencias de este proyecto y evitar conflictos con la instalación global de Python de tu sistema, especialmente con paquetes científicos como NumPy.

### Pasos:

1. **Clona este repositorio:**
   ```bash
   git clone https://github.com/TU_USUARIO/taxi-q-learning.git
   cd taxi-q-learning
   ```

2. **Crea y activa un entorno virtual (Ejemplo para Windows/PowerShell):**
   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. **Instala las librerías requeridas:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Inicia el servidor de Jupyter:**
   ```bash
   jupyter notebook notebooks/agente_taxi.ipynb
   ```

---

## 📊 5. Evaluación de Resultados

Tras someter al agente a un entrenamiento iterativo de **10.000 episodios**, los resultados demuestran una convergencia total hacia una política óptima.

> [!WARNING]
> La función de recompensa del entorno emite señales altamente ruidosas debido a las penalizaciones de -10 por acciones logísticas inválidas y -1 por desplazamiento. 
> 
> En el Notebook adjunto, se ha implementado un filtro de **Media Móvil (Moving Average)** mediante `np.convolve` para el procesamiento analítico de las curvas.

**Hitos de Entrenamiento:**
* **Fase Temprana (0 - 2.000 episodios):** Comportamiento errático, las recompensas acumuladas son fuertemente negativas (-200 a -500). Alta penalización por acciones nulas.
* **Fase de Inflexión (2.000 - 4.000 episodios):** El valor de la Q-Table retropropaga con éxito la señal de recompensa de éxito final (+20). Se observa una pendiente logarítmica creciente.
* **Asintotización (4.000+ episodios):** Las penalizaciones se reducen a cero estadístico. El agente traza rutas deterministas (mínimo número de pasos posibles dictado por la topología de la matriz).

---
*Documentación estructurada y desarrollada siguiendo los principios de control de versiones y convenciones de GitHub.*