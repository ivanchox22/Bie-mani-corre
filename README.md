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

---

## 2) Ejemplos

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
