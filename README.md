# Bie-mani-corre

# Diseño de Eslabones con Simulink y Simscape Multibody

## 1) Diseño de eslabones

El diseño de eslabones en entornos de simulación como **Simulink**, utilizando el complemento **Simscape Multibody**, constituye una herramienta clave para representar y analizar el comportamiento de sistemas mecánicos complejos. En este contexto, los *eslabones* son elementos rígidos que transmiten fuerzas o movimientos dentro de un mecanismo. Se utilizan en una amplia variedad de aplicaciones, desde brazos robóticos hasta sistemas de suspensión automotriz o mecanismos de transmisión como la biela-manivela.

Dentro del entorno de Simulink, los eslabones se modelan mediante el bloque `Solid`, el cual permite especificar parámetros físicos fundamentales como:

- Masa
- Volumen
- Centro de gravedad
- Geometría

La conectividad entre los cuerpos rígidos se logra mediante **juntas mecánicas** (*joints*), que definen la relación espacial entre ellos y los grados de libertad del sistema. Algunas de las juntas más comunes incluyen:

- 🔧 **Revolute Joint**: permite la rotación relativa entre dos sólidos alrededor de un eje fijo. Es comparable a una bisagra o a una articulación como el codo en un brazo robótico.


![image](https://github.com/user-attachments/assets/e701a845-b586-4226-b350-0fbb9ef036fe)

***Fig 1. Revolute Joing***


- 🔧 **Prismatic Joint**: restringe el movimiento a una sola dimensión lineal. Es similar al comportamiento de un pistón o actuador lineal.

![image](https://github.com/user-attachments/assets/271a360b-e82a-4d06-ae96-bbde94dd4e32)

***Fig 2. Prismatic Joing***


Además, se pueden integrar **actuadores** y **sensores** en el modelo para aplicar fuerzas, torques, o movimientos predefinidos, y para medir variables físicas como:

- Ángulos de rotación
- Velocidades angulares
- Posiciones lineales

Una herramienta fundamental en la interacción entre el entorno físico y el matemático de Simulink es el bloque `PS Converter`, que permite transformar señales convencionales (como senoidales o impulsos) en señales físicas aceptadas por Simscape. Esto facilita, por ejemplo:

- Prescribir entradas dinámicas a las juntas
- Aplicar torques variables
- Controlar movimientos con señales generadas por algoritmos o controladores

![image](https://github.com/user-attachments/assets/80382fda-dda3-4192-943d-244b9882a5ac)

***Fig 3. PS Converter***

---

## 2) Ejemplo
A continuación, se presentará un ejemplo de simulación mecánica que ilustra el comportamiento dinámico de un cubo en movimiento. Este modelo tiene como objetivo aplicar los conceptos y componentes previamente descritos, tales como eslabones rígidos, juntas y señales físicas dentro del entorno de Simulink con Simscape Multibody.

En particular, el sistema simulado representa un movimiento oscilatorio vertical del cubo, caracterizado por un desplazamiento alternante de subida y bajada a lo largo de un único eje (movimiento traslacional unidimensional). Este comportamiento se implementa mediante una Prismatic Joint, la cual restringe el movimiento del cuerpo rígido a un solo grado de libertad lineal.

La trayectoria oscilatoria del cubo se prescribe a través de una señal física senoidal, convertida desde el entorno de Simulink mediante un bloque PS Converter, lo que permite generar un movimiento de ida y vuelta continuo con frecuencia y amplitud definidas.

A partir del esquema presentado a continuación, se procede a realizar una descripción concisa del sistema, destacando sus componentes principales y su funcionamiento dentro del entorno de simulación.

![image](https://github.com/user-attachments/assets/0c2f4eae-404a-4449-b724-8683042abba2)

***Fig 4. Ejemplo***

 El esquema mostrado a continuación representa una configuración básica en **Simscape Multibody** para la simulación de un sistema mecánico simple. En él se emplean diversos bloques fundamentales que permiten ilustrar el movimiento oscilatorio vertical de un cuerpo rígido. A continuación, se describen los componentes principales utilizados:

- **Solver Configuration**, **World Frame** y **Mechanism Configuration**:  
  Estos tres bloques constituyen la base estructural mínima necesaria para inicializar y ejecutar correctamente una simulación física en Simscape Multibody. Permiten definir el entorno de referencia, los parámetros del solucionador y la configuración general del mecanismo.

- **Sine Wave**:  
  Genera una señal senoidal que define la trayectoria oscilatoria deseada, representando el desplazamiento periódico que será aplicado al sistema.

- **Simulink-PS Converter**:  
  Transforma la señal senoidal generada en Simulink en una señal física compatible con Simscape, permitiendo su aplicación directa sobre componentes del modelo físico.

- **Prismatic Joint**:  
  Define un grado de libertad lineal entre dos cuerpos, restringiendo el movimiento del cubo a una traslación uniaxial (de arriba hacia abajo) a lo largo de un eje definido.

- **Brick Solid**:  
  Representa el cuerpo rígido (el cubo) en la simulación. Se definen sus propiedades físicas como forma, dimensiones y masa.


### Configuracion principal mechanism Configuration

El bloque **Mechanism Configuration** tiene como propósito establecer los parámetros globales del entorno de simulación mecánica. En este caso específico, se utiliza para habilitar la acción de la **gravedad**, la cual se ha definido actuando a lo largo del eje **Y**. Esta configuración asegura que las fuerzas gravitacionales influyan correctamente sobre el movimiento del cuerpo rígido (el cubo) durante la simulación.

Como se puede observar en la **Figura 5**, la dirección de la gravedad ha sido orientada negativamente sobre el eje Y, lo que simula un entorno físico realista en el que el cubo tiende a desplazarse hacia abajo cuando no se aplican fuerzas externas compensatorias.


![image](https://github.com/user-attachments/assets/14da1f3e-f8b1-4237-8312-4ab7c63b3b5f)

***Fig 5. mechanism Configuration***

### Configuracion Sine Wane

La **Figura 6** muestra los parámetros establecidos para el bloque **Sine Wave**, el cual se encarga de generar una señal senoidal que actúa como entrada dinámica para el sistema.

Los valores configurados son los siguientes:

- **Amplitude:** `0.5`  
  Define la amplitud máxima de la señal, es decir, el desplazamiento máximo que el cubo alcanzará en su movimiento oscilatorio vertical.

- **Bias:** `0`  
  No se aplica ningún desplazamiento constante a la señal, lo que indica que oscila simétricamente respecto al cero.

- **Samples per period:** `10`  
  La señal se discretiza en 10 muestras por ciclo completo, lo cual influye directamente en la resolución temporal de la onda.

- **Number of offset samples:** `0`  
  No se añade ningún desfase temporal a la señal.

- **Sample time:** `0`  
  Este valor indica que se utilizará el tiempo de simulación por defecto, es decir, el bloque generará una señal continua basada en el tiempo del solver.

- **Sine type:** `Sample based`  
  Se emplea una onda senoidal basada en muestreo, ideal para simulaciones que requieren un control preciso sobre el número de muestras por período.

- **Interpret vector parameters as 1-D:** Activado  
  Garantiza que los parámetros definidos como vectores se interpreten como vectores unidimensionales (1-D), lo cual es adecuado para señales escalares.

Esta configuración permite generar una señal senoidal suave y periódica, adecuada para simular un movimiento oscilatorio controlado del cubo a lo largo del eje Y mediante una **Prismatic Joint**.

![image](https://github.com/user-attachments/assets/1f4c2434-c4f9-4774-b41b-acf0590ff6ce)

***Fig 6. Sine Wane***

### Configuracion Prismatic Joing

La **Figura 7** muestra la configuración del bloque **Prismatic Joint**, empleado para restringir el movimiento relativo entre dos cuerpos rígidos a lo largo de un solo eje lineal. En este caso, se ha configurado la **primitiva prismatic Z (Pz)**, lo que indica que el movimiento permitido se realiza únicamente sobre el eje **Z** del sistema de coordenadas.

**Parámetros configurados:***

- **Actuation**:
  - **Force:** `Automatically Computed`  
    El bloque calcula automáticamente la fuerza requerida para que el cuerpo se mueva según la señal de entrada prescrita, considerando las fuerzas externas, como la gravedad.
  
  - **Motion:** `Provided by Input`  
    El desplazamiento del cuerpo móvil no se determina por las leyes físicas del sistema, sino que es impuesto directamente desde una entrada externa. Esta entrada proviene de una señal física (por ejemplo, una onda senoidal convertida desde Simulink mediante un **Simulink-PS Converter**).

- **Sensing**:
  - ✅ **Position:** Activada  
    Esta opción permite medir en tiempo real la posición del cuerpo a lo largo del eje Z. La activación de esta opción es útil para monitoreo, análisis posterior o para la implementación de lazos de control en tiempo real.
  
  - Las demás opciones (Velocity, Acceleration, Actuator Force, etc.) no están activadas en esta configuración, pero podrían habilitarse si se requiere una observación más detallada del comportamiento dinámico del sistema.

Esta configuración, junto con una entrada senoidal, genera un **movimiento oscilatorio vertical prescrito** del cuerpo (cubo) en el eje Z. Gracias al sensor de posición activado, es posible visualizar y registrar la trayectoria del cubo a lo largo del tiempo, lo que resulta útil para validar el comportamiento deseado o comparar contra otras estrategias de control.

![image](https://github.com/user-attachments/assets/530f6d3e-e831-4a27-9c1a-34991b193078)

***Fig 7. Prismatic Joing***

### Configuracion Brick Solid



## 3) Ejercicios

### ✅ Caso 1: Sistema biela-manivela-corredera

Este mecanismo clásico puede modelarse con tres cuerpos rígidos: manivela, biela y pistón. 

- La manivela gira con una `Revolute Joint`.
- El pistón se traslada con una `Prismatic Joint`.
- La biela interconecta ambos y transmite el movimiento.

Este modelo es útil para estudiar cómo un movimiento rotacional se convierte en traslacional, tal como ocurre en un motor de combustión interna.

### ✅ Caso 2: Manipulador robótico

Un brazo robótico puede representarse con múltiples eslabones conectados con `Revolute Joints`. Cada articulación permite la rotación relativa entre los segmentos. Se pueden aplicar torques y medir ángulos con sensores para:

- Controlar trayectorias
- Analizar cinemática y dinámica
- Validar estrategias de control

### ✅ Caso 3: Suspensión automotriz

En un sistema de suspensión:

- Las ruedas están conectadas al chasis por eslabones.
- Se usan juntas rotacionales y traslacionales para simular los grados de libertad del sistema.
- Se pueden incluir resortes y amortiguadores para representar las fuerzas de suspensión.

En el ejemplo presentado a continuación se podra obser el funcionamiento

### ✅ Caso 4: Señales dinámicas con PS Converter

Gracias al bloque `PS Converter`, es posible tomar señales generadas desde Simulink, como:

- Ondas senoidales
- Señales cuadradas
- Resultados de un controlador PID

Estas señales pueden aplicarse a una junta o actuador, permitiendo simular entradas reales al sistema físico y observar su respuesta.

---

## 3) Conclusiones

El modelado de sistemas mecánicos mediante Simscape Multibody en Simulink permite:

- Simular con precisión mecanismos con múltiples grados de libertad.
- Visualizar el comportamiento dinámico de los eslabones.
- Medir y analizar variables como torques, fuerzas, desplazamientos y velocidades angulares.

La combinación de bloques como `Solid`, `Revolute Joint`, `Prismatic Joint` y `PS Converter` ofrece una plataforma potente y versátil para diseñar, analizar y validar prototipos virtuales antes de su implementación física.

Además, la integración de señales externas posibilita la aplicación de estrategias de control avanzado, análisis de comportamiento ante perturbaciones y validación de algoritmos de optimización e inteligencia artificial.

En resumen, el conocimiento profundo sobre el modelado de eslabones y su implementación en Simulink es una base sólida para enfrentar retos en automatización, robótica, diseño mecánico y control de sistemas complejos.
