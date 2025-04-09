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

La **Figura 7** muestra la confirguración para la creación de un cubo.

Se ha especificado un vector de dimensiones de la forma `[1 1 1]`, lo que significa que el cuerpo tiene:

  - **Ancho (X):** 1 metro  
  - **Alto (Y):** 1 metro  
  - **Profundidad (Z):** 1 metro

  Esto configura un cubo perfecto de **1 m³** en el entorno de simulación.

- **Unidad:**  
  Las unidades están definidas en metros (`m`), lo cual es coherente con la mayoría de los entornos físicos simulados y facilita el análisis dimensional y de escalado.

- **Compile-time:**  
  Esta opción indica que las dimensiones están definidas en tiempo de compilación, es decir, no cambian durante la simulación.

![image](https://github.com/user-attachments/assets/15d44cf6-9d75-4eac-bdeb-5ad4b7a29ddd)

***Fig 8. Brick Solid***

### SIMULACION

Durante la ejecución de la simulación, se puede observar el comportamiento dinámico del cubo creado mediante el bloque **Solid**. Este cubo está acoplado a una **junta prismática (Prismatic Joint)**, la cual permite desplazamientos exclusivamente a lo largo de un eje lineal, en este caso, el eje vertical (eje Y).

Gracias a la señal senoidal aplicada como entrada de movimiento al **Prismatic Joint**, el cubo ejecuta un **movimiento oscilatorio de traslación vertical**, subiendo y bajando de forma periódica a lo largo del tiempo.

En la **Figura 9** se puede ver el cubo durante uno de los ciclos de su movimiento oscilante. El cubo aparece desplazado hacia una posición inferior, lo cual confirma la acción de la onda senoidal que modula su posición al igual que en la **Figura 10** ya que el cubo aparece desplazado hacia una posición inferior.

En la animación de la simulación se aprecia claramente cómo el cubo:

- Asciende y desciende suavemente en función de la amplitud y frecuencia de la onda senoidal configurada.
- Mantiene una trayectoria rectilínea sobre el eje **Y**, gracias a las restricciones impuestas por el **Prismatic Joint**.
- Responde con fluidez al perfil de entrada definido, evidenciando un correcto acoplamiento entre el mundo físico (Simscape) y el entorno de señales matemáticas (Simulink).

Este resultado valida el modelo construido, confirmando que la interacción entre los bloques ha sido correctamente configurada para simular un comportamiento físico realista.

![image](https://github.com/user-attachments/assets/ca005403-439f-4eb2-86da-5df6b42d9c85)

***Fig 9. Movimiento del cubo inferior***

![image](https://github.com/user-attachments/assets/60dd17f3-9c65-4ed6-9a59-77b6c9839e58)

***Fig 10. Movimiento del cubo superior***

## 3) Ejercicios

### ✅ Caso 1: Sistema biela-manivela-corredera

Este mecanismo clásico puede modelarse con tres cuerpos rígidos: manivela, biela y pistón. 

![image](https://github.com/user-attachments/assets/570e2076-b81d-430f-9586-223987135b0b)

***Fig 11. Esquema del Sistema Biela-Manivela-Corredera***

La **Figura 11** representa la implementación en Simulink/Simscape Multibody de un mecanismo clásico compuesto por **biela**, **manivela** y **corredera**. Este tipo de sistema es ampliamente utilizado en aplicaciones como motores de combustión interna y compresores, debido a su capacidad de convertir un **movimiento rotacional** en **movimiento lineal**.

#### Descripción del Modelo

El modelo se estructura a partir de varios bloques fundamentales de Simscape Multibody:

- **Solver Configuration**: Configura el solucionador para realizar la simulación física.
- **World Frame**: Define el sistema de referencia inercial global.
- **Mechanism Configuration**: Permite ajustar condiciones globales del sistema, como la gravedad.
- **Simulink-PS Converter**: Convierte una señal senoidal de entrada de Simulink en una señal física (PS) utilizable por los bloques de Simscape.
- **PS-Simulink Converter**: Convierte señales físicas de salida nuevamente al dominio de Simulink para su visualización (por ejemplo, en el Scope).

#### Componentes del Mecanismo

1. **Manivela**: 
   - Bloque sólido que gira continuamente alrededor de un eje fijo mediante una **Revolute Joint** (articulación rotacional).
   - El movimiento rotacional se genera mediante la señal senoidal conectada a la junta rotacional, simulando un eje impulsor.

2. **Biela**: 
   - Conectada entre la manivela y la corredera mediante dos juntas **Revolute Joint**.
   - Transmite el movimiento rotacional de la manivela a la corredera en forma de desplazamiento lineal.

3. **Corredera**: 
   - Representada como un sólido que se mueve linealmente gracias a una **Prismatic Joint**.
   - Esta junta limita el movimiento a lo largo de un solo eje (en este caso, lineal horizontal o vertical, según la configuración del modelo).
   - El desplazamiento de la corredera se visualiza mediante un bloque **Scope**.

4. **Rigid Transform**:
   - Se utiliza para establecer posiciones relativas entre los distintos componentes sólidos y ajustar alineaciones físicas.

#### Funcionalidad

El sistema convierte el **movimiento oscilatorio rotacional de la manivela**, provocado por la onda senoidal, en un **movimiento alternativo rectilíneo de la corredera**. La biela actúa como el eslabón intermedio que transmite y adapta el movimiento entre las otras dos partes.

Este modelo permite observar en tiempo real cómo se comporta un sistema mecánico real bajo condiciones de entrada dinámicas y es útil tanto en simulaciones de diseño como en estudios de control y dinámica de mecanismos.

#### Simulación

En Las **Figura 12** y **Figura 13**  se muestran capturas de la simulación en Simscape Multibody del sistema biela-manivela-corredera. En esta vista, se pueden observar claramente los tres elementos principales del mecanismo:

- **Manivela** (bloque azul a la izquierda),
- **Biela** (elemento en color magenta),
- **Corredera** (bloque marrón a la derecha).

![image](https://github.com/user-attachments/assets/3d3dbf0d-e8b4-4666-889d-926d9f08844f)
 
***Fig 12. simulacion1 del Sistema Biela-Manivela-Corredera***

![image](https://github.com/user-attachments/assets/a766ed80-13c7-47f0-99fa-4f7484e5b91f)

***Fig 13. simulacion2 del Sistema Biela-Manivela-Corredera***

En la animación se puede notar que:

- El conjunto de cuerpos rígidos reacciona de manera continua a la entrada de onda senoidal.
- La **biela** actúa como eslabón intermediario dinámico, cambiando de orientación para acompañar tanto el giro de la manivela como el desplazamiento de la corredera.
- El sistema completo funciona de manera armónica, simulando fielmente un **ciclo mecánico completo**, como el que ocurre en el pistón de un motor de combustión interna.

### ✅ Caso 2: tres eslabones

El modelo de un mecanismo compuesto por **tres eslabones** conectados mediante **juntas rotacionales (Revolute Joints)** permite representar con fidelidad el comportamiento de sistemas articulados como brazos robóticos, extremidades mecánicas o manipuladores articulados.

La **Figura 14** representa un sistema mecánico compuesto por **tres eslabones** conectados secuencialmente mediante **juntas rotacionales (Revolute Joints)**. Este tipo de configuración es comúnmente utilizada en mecanismos articulados como brazos robóticos y manipuladores.

![image](https://github.com/user-attachments/assets/0a2a3750-5ff9-4512-9b81-6f2b4f8d5496)

***Fig 14. Esquema de diseño de 3 eslabones***

### Componentes del Modelo

- **Solver Configuration**: Define los parámetros de resolución del modelo físico.
- **World Frame**: Establece el marco de referencia global para la simulación.
- **Mechanism Configuration**: Permite especificar condiciones físicas del entorno como la gravedad.
- **Rigid Transform**: Posiciona el primer eslabón respecto al marco global.


### Estructura de los Eslabones

1. **Eslabón 1**
   - Conectado al **World Frame** mediante una **Revolute Joint**.
   - Permite la rotación inicial del sistema.
   - Simula una base giratoria.

2. **Eslabón 2**
   - Unido al extremo del eslabón 1 mediante una segunda **Revolute Joint**.
   - Esta articulación permite el movimiento relativo entre el primer y segundo segmento.
   - El bloque de sólido representa su geometría y masa.

3. **Eslabón 3**
   - Conectado al final del segundo eslabón a través de una tercera **Revolute Joint**.
   - Funciona como el extremo libre o efector del sistema.

### Funcionalidad del Sistema

- **Actuación rotacional**: A través de las juntas rotacionales, se puede aplicar movimiento controlado a cada eslabón.
- **Medición angular**: Mediante sensores en las juntas se puede obtener la posición angular de cada articulación.
- **Salida a Scope**: El ángulo de alguna de las juntas es enviado a un bloque `Scope` mediante un `PS-Simulink Converter`, permitiendo visualizar la respuesta dinámica del sistema.

### Simulación.

Las **Figuras 15, 16 y 17** corresponden a diferentes momentos capturados durante la simulación del mecanismo de **tres eslabones** articulados mediante juntas rotacionales (`Revolute Joint`). Este sistema puede asociarse conceptualmente con un brazo robótico en movimiento, en el cual cada eslabón se mueve como resultado de las señales de entrada que provocan rotaciones en las articulaciones.

![image](https://github.com/user-attachments/assets/a352502e-9950-41d8-99b8-658e26a209a6)

***Fig 15. Simulacion 1 de diseño de 3 eslabones***

![image](https://github.com/user-attachments/assets/f6ed20c8-2f52-4ae3-ae7d-150c6ca087b9)

***Fig 16. Simulacion 2 de diseño de 3 eslabones***

![image](https://github.com/user-attachments/assets/66426e1e-cfa2-4df8-86fb-1f6f42c00ca6)

***Fig 17. Simulacion 3 de diseño de 3 eslabones***


### ✅ Caso 3: Suspensión automotriz

En un sistema de suspensión:

- Las ruedas están conectadas al chasis por eslabones.
- Se usan juntas rotacionales y traslacionales para simular los grados de libertad del sistema.
- Se pueden incluir resortes y amortiguadores para representar las fuerzas de suspensión.

En el ejemplo presentado a continuación se podra obser el funcionamiento

---

## 3) Conclusiones

El modelado de sistemas mecánicos mediante Simscape Multibody en Simulink permite:

- Simular con precisión mecanismos con múltiples grados de libertad.
- Visualizar el comportamiento dinámico de los eslabones.
- Medir y analizar variables como torques, fuerzas, desplazamientos y velocidades angulares.

La combinación de bloques como `Solid`, `Revolute Joint`, `Prismatic Joint` y `PS Converter` ofrece una plataforma potente y versátil para diseñar, analizar y validar prototipos virtuales antes de su implementación física.

Además, la integración de señales externas posibilita la aplicación de estrategias de control avanzado, análisis de comportamiento ante perturbaciones y validación de algoritmos de optimización e inteligencia artificial.

En resumen, el conocimiento profundo sobre el modelado de eslabones y su implementación en Simulink es una base sólida para enfrentar retos en automatización, robótica, diseño mecánico y control de sistemas complejos.
