<p align="center">
  <img src="" style="width: 250px;">
  
</p>
<h1 align="center">FinanceGymRL</h1>


FinanceGymRL, un entorno personalizado y sencillo para entrenar modelos de aprendizaje por refuerzo (RL) diseñados para estrategias de trading e inversión. Basado en OpenAI Gym, este repositorio ofrece un marco personalizable y robusto para cualquiera que desee adentrarse en el mundo del RL financiero.

## ¿Por qué FinanceGymRL?

En un intento fallido de utilizar GitHub - ClementPerroud/Gym-Trading-Env por problemas de memoria en mi ordenador, decidí crear un environment personalizado en Gym desde el cual poder ejecutar y entrenar agentes especializados en movimientos de trading. Los mercados financieros son un dominio complejo y dinámico donde las estrategias tradicionales a menudo se quedan cortas. El aprendizaje por refuerzo, con su capacidad para aprender y adaptarse a entornos cambiantes, presenta una alternativa poderosa. FinanceGymRL cierra la brecha entre la investigación avanzada en RL y las aplicaciones prácticas de trading, proporcionando una plataforma intuitiva tanto para principiantes como para expertos.

## Características del entorno

**StockTradingEnv** es un entorno de trading de acciones para OpenAI Gym, donde un agente toma decisiones de compra y venta de acciones. Las características del entorno son las siguientes:

### Tipos de Estados

**Espacio de estados:**
El espacio de estados es continuo y multidimensional, compuesto por:

* Posición de apertura de las acciones de los últimos cinco días, escalada entre 0 y 1.
* Máximo precio de las acciones de los últimos cinco días, escalada entre 0 y 1.
* Mínimo precio de las acciones de los últimos cinco días, escalada entre 0 y 1.
* Precio de cierre de las acciones de los últimos cinco días, escalada entre 0 y 1.
* Volumen de las acciones de los últimos cinco días, escalado entre 0 y 1.
* Datos adicionales escalados entre 0 y 1:
  - Saldo actual (balance).
  - Valor neto máximo alcanzado (max_net_worth).
  - Acciones en posesión (shares_held).
  - Base de coste media de las acciones en posesión (cost_basis).
  - Total de acciones vendidas (total_shares_sold).
  - Valor total de las ventas (total_sales_value).

### Tipos de acciones

**Espacio de acciones:**
El espacio de acciones es continuo, con dos dimensiones:

* Tipo de acción (0: Comprar, 1: Vender, 2: Mantener).
* Cantidad de la acción (proporción del saldo o de las acciones en posesión).

### Recompensas

La recompensa se calcula en función del saldo actual multiplicado por un modificador de retraso, que es proporcional al progreso dentro del episodio. No hay recompensas positivas o adicionales explícitas por alcanzar ciertos objetivos, pero la recompensa implícita es maximizar el valor neto de la cartera a lo largo del tiempo.


## Desglose y explicación de CustomTradingEnv.ipynb

### Definición del entorno

Comenzamos definiendo una serie de constantes que representan los límites máximos de varias variables, tales como el balance de la cuenta, el número de acciones, los precios de las acciones y los pasos máximos de la simulación. Estas constantes ayudan a estructurar el entorno y establecer las condiciones bajo las cuales el agente tomará decisiones.

### Clase StockTradingEnv

Creamos una clase `StockTradingEnv` que hereda de `gym.Env`. Esta clase define el entorno de trading de acciones, especificando cómo se inicializa, cómo se toman las acciones, cómo se observan los estados y cómo se calcula la recompensa.

#### Inicialización

En el método `__init__`, inicializamos las variables necesarias y definimos los espacios de acciones y observaciones. El espacio de acciones es continuo (`Box`), y el espacio de observaciones incluye valores OHLC (Open, High, Low, Close) de los últimos cinco días, junto con otras características del estado de la cuenta. Los espacios se definen con límites y tipos de datos adecuados.

#### Observaciones

El método `_next_observation` obtiene los puntos de datos de acciones de los últimos cinco días y los escala entre 0 y 1. También incluye datos adicionales como el balance de la cuenta, el valor neto, las acciones en posesión, el costo base y las ventas totales. Este método asegura que el agente tenga una visión completa y normalizada del estado actual del entorno.

#### Acciones

El método `_take_action` ejecuta la acción seleccionada (mantener, comprar, vender) y actualiza el estado del entorno en consecuencia. Dependiendo de la acción, ajusta el balance de la cuenta, el número de acciones en posesión y otros factores financieros relevantes. Este método simula el impacto de las decisiones del agente en el entorno de trading.

#### Paso y Reinicio

El método `step` ejecuta un paso en el entorno, actualiza el estado y calcula la recompensa. Este método también determina si el episodio ha terminado. El método `reset` reinicia el entorno a su estado inicial, preparándolo para un nuevo episodio de simulación. Estos métodos son esenciales para el ciclo de aprendizaje del agente, proporcionando un marco para la interacción y evaluación continua.

#### Renderizado

El método `render` visualiza el estado del entorno, mostrando gráficos del precio de las acciones y del valor neto a lo largo del tiempo. Esto permite una comprensión visual del rendimiento del agente y la evolución del entorno durante la simulación.

## Tutorial

Este script te permite crear tu propio entorno de trading sin necesidad de picar código:

* Define tus datos del mercado: Carga datos históricos del mercado ya sea precios de acciones, valores de criptomonedas o lo que necesites.
* Personaliza recompensas y acciones: Adapta la estructura de recompensas y las acciones disponibles para reflejar tu estrategia de trading.
* Entrena tu modelo: Utiliza los bucles de entrenamiento integrados o intégralos con tu biblioteca de RL preferida.


## Resumen del repositorio

## Estructura del repositorio
 <pre>
├── TEST_Gym_Trading_Env_OoM.ipynb # Gym-Trading-Env de ClementPerroud
├── CustomTradingEnv.ipynb # Creación del entorno personalizado
├── Data # Carpeta con los Datos utilizados en el entrenamiento
│ ├── AAPL.csv
│ └── bitfinex2-BTCUSDT-1h.pkl
└── README.md # Este archivo README
  </pre>

Este repositorio contiene dos scripts principales que demuestran las capacidades y desafíos de desarrollar entornos de trading utilizando OpenAI Gym:

### TEST_Gym_Trading_Env_OoM.ipynb

En este cuaderno, intentamos ejecutar un entorno de trading del repositorio Gym-Trading-Env de ClementPerroud. Aunque este script busca aprovechar un entorno de trading existente, encuentra problemas de memoria (Out of Memory, OoM), destacando los desafíos asociados con las simulaciones financieras a gran escala.

### CustomTradingEnv.ipynb

Este script está diseñado para crear y ejecutar un entorno de trading personalizado. El objetivo es simular el proceso de compra y venta de acciones en el mercado utilizando OpenAI Gym e integrarlo con algoritmos de aprendizaje por refuerzo, específicamente Proximal Policy Optimization (PPO), para desarrollar y evaluar estrategias de trading automatizadas.

<h2>Contribución</h2>

  <p>Si deseas contribuir o preguntar cualquier duda sobre este proyecto, ¡Contacta por LinkedIn! Y siéntete libre de crear pull requests con tus contribuciones.</p>

  <h2>Créditos</h2>

  <p>Este proyecto ha sido desarrollado por <a href="#"> Vicent Muñoz Correcher </a>.</p>

<h2> Contacto </h2>
Para cualquier consulta, puedes contactarme a través de mi perfil de LinkedIn o GitHub.


