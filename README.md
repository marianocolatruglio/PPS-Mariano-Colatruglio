<img width="1920" height="850" alt="image" src="https://github.com/user-attachments/assets/0d32e7cb-cd37-48f3-a46f-b64e3e03b03f" />

Alumno: Mariano Colatruglio\
Carrera: Ingeniería Mecatrónica\
Empresa: Salvador Colatruglio S.R.L.\
Tutor Institucional: Christian Moscariello - Supervisor de Mantenimiento\
Tutor Académico: Cristian Lukaszewicz



# INFORME DE PRACTICA PROFESIONAL SUPERVISADA 
# 🛠️ Actualización tecnológica de sistema CNC para corte por plasma

Este repositorio contiene el informe final del proyecto desarrollado en el marco de las **Prácticas Profesionales Supervisadas (PPS)** de la carrera **Ingeniería en Mecatrónica** – Facultad de Ingeniería, Universidad Nacional de Lomas de Zamora.

## 📋 Descripción del proyecto

El proyecto consistió en la **modernización de un sistema de control CNC** para una máquina de **corte por plasma**, utilizado en una empresa del rubro metalúrgico. El sistema original (basado en arquitectura modular, comunicaciones RS-232/422 y plataforma DOS) fue reemplazado por un controlador **FangLing F2100**, incorporando tecnología ARM y FPGA con sistema operativo en tiempo real.
Se integró además un sistema de **control de altura (THC)** 

![PANTOGRAFO-NEW](https://github.com/user-attachments/assets/38fd0095-8a30-49a6-a8e4-12ce7c9c6ac3)

## 🎯 Objetivos

- Sustituir el sistema CNC obsoleto por uno moderno y confiable.
- Implementar un controlador con interpolación mixta (software + hardware).
- Incorporar control de altura automático (THC) para mejorar la calidad de corte.
- Asegurar la compatibilidad con herramientas CAD/CAM y generar G-code optimizado.
- Documentar la implementación técnica como parte del proceso de formación profesional.

## 🧩 Tecnologías y herramientas utilizadas

- 🔌 Controlador **FangLing F2100B**
- ⚙️ Fuente de corte **Hypertherm HSD130**
- 📐 Software de diseño y CAM: AutoCAD + Pronest 2019
- 💾 Postprocesador personalizado para código G
- 🔧 Sensores mecánicos e inductivos, electrónica de potencia
- 📑 Documentación en formato **pdf**


## 1 – PRESENTACIÓN
### 1.1 Lugar donde he realizado la PPS.
Salvador Colatruglio S.R.L. es una PYME familiar del rubro metalúrgico, se encuentra  ubicada en la ciudad de Luis Guillón, partido de Esteban Echeverria, Buenos Aires. Iniciando sus actividades como una pequeña tornería fundada por Salvador Colatruglio, fue creciendo y transformándose en una empresa que brinda múltiples servicios, como ser corte, plegado, cilindrado, soldadura y fabricación de estructuras metálicas entre otras actividades. Actualmente cuenta con 9 empleados, entre personal administrativo, oficina técnica y producción. Posee una planta dividida en tres naves en las que se desarrollan las distintas actividades, sector de oficinas y almacenamiento, sector de soldadura y montaje , y sector de corte y plegado. Las naves cuentan con puentes grúa para el movimiento de materiales, carga y descarga de camiones.
Dentro de la gran variedad de maquinaria utilizada se destacan las maquinas de corte de laminas de acero, en todas sus variantes: Oxicorte, Laser y Plasma. Esta ultima es el objeto del desarrollo de este informe.
•	pantógrafo CNC “Master” Fuente plasma, 130A. “Hypertherm HySpeed 130” 
Con el paso del tiempo, la electrónica de control original quedó obsoleta, dificultando la obtención de repuestos y limitando las prestaciones de calidad de corte, precisión y velocidad.
### 1.2 Objeto del proyecto
La PPS tuvo como objetivo la actualización integral del sistema CNC de corte por plasma, abarcando: Sustitución del controlador original por un sistema CNC FangLing F2100, incluyendo la incorporación de un controlador de altura (THC) y configuración  de un postprocesador CAD/CAM para tal fin.
### 1.3 Áreas disciplinares involucradas
Automatización y Control · Electrónica Industrial · Programación de Sistemas Embebidos · CAD/CAM · Procesos de Manufactura · Tecnología de Materiales. 
## 2 – DETALLE DEL TRABAJO REALIZADO
### 2.1	Modernización de Hardware de Control CNC
•	Diagnóstico inicial: 
Se diagnosticó el estado general de la maquina y se relevaron los componentes principales de la máquina. 

•	Controlador CNC Gundel  - LPT
•	STEPPING Drivers Leadshine MD2278
•	Motores Paso a Paso NEMA 34 – 1.8° - 4.2A/PH FL86STH118-4208A
•	Controlador de altura PHC Hypertherm.

El resultado del diagnóstico arrojó que el control CNC era obsoleto y presentaba fallas recurrentes, a su vez el control de altura también presentaba un desgaste sustancial en cuanto a lo mecánico y también presentaba fallas en la electrónica de control. Ambos no reunían condiciones ni económicas (+$10000 USD) ni tecnológicas que posibilitaran su reparación por lo que se optó por el reemplazo completo del sistema de control, manteniendo los motores y drivers. 
Para ello se analizaron varias opciones que justificaran la actualización del equipo, se debían cumplir ciertos requisitos como ser: costos, tamaño, peso, configuración, y disponibilidad.

•	Selección de hardware: 
•	CNC FangLing F2100. 2 ejes.
•	Controlador de altura de antorcha por voltaje de arco HYD XPTHC-4H para máquina de corte por Plasma CNC.
•	Elevador CNC de 100mm, 2400 mm/min, JYKB-100-DC24V, T3, THC, para antorcha de corte por Plasma
•	

CNC	THC	Torch Lifter
 	 	 

<img width="1214" height="299" alt="image" src="https://github.com/user-attachments/assets/8217d2ff-d23e-4de5-af97-619208c1deb9" />



##  2.2 PRINCIPIO DE FUNCIONAMIENTO

En este resumen se pretende describir como es el funcionamiento del sistema de corte y como se relaciona el CNC con la fuente de plasma y el control de altura de la torcha de corte plasma.
El proceso de corte por chorro de plasma es una tecnología de corte térmico altamente eficiente que emplea un gas ionizado (plasma) para fundir y expulsar el material del corte en metales conductores.
El sistema CNC (control numérico computarizado) controla el movimiento de la antorcha sobre el plano XY, siguiendo un recorrido programado (G-code) generado desde un software CAD/CAM.
Para ello existen interacciones entre los distintos componentes del sistema.
a) Interacción entre CNC y fuente de plasma
•	El CNC envía señales digitales (por relé o salida digital) para:
o	Encender/apagar el plasma (torch on)
o	Iniciar la secuencia de corte
•	La fuente Hypertherm devuelve señales de estado:
o	“Arc OK” (arco establecido y estable)
o	Fallas (sin arco, presión baja, consumible gastado, etc.)

b) Control de altura (THC – Torch Height Control)
Hypertherm recomienda utilizar un THC para garantizar cortes precisos y consistentes, ya que la distancia entre la boquilla y la chapa afecta directamente la calidad del corte y la vida útil de los consumibles.
El THC se vincula al sistema así:
1.	Búsqueda inicial: El THC detecta la superficie tocando o midiendo voltaje sin cortar (Touch Off)
2.	Perforado (Piercing): La antorcha se eleva a una distancia segura para iniciar el corte
3.	Corte en movimiento: Se ajusta dinámicamente la altura midiendo el voltaje del arco (arc voltage): si aumenta, sube la antorcha; si disminuye, baja
4.	Final de corte: El sistema detiene el movimiento y desactiva el plasma


c) Comunicación entre CNC, THC y plasma
En sistemas como FangLing, , la vinculación se realiza mediante:
•	Entradas/salidas digitales
•	Señales análogas (para leer tensión de arco)

## 2.3 CICLO DE FUNCIONAMIENTO.

Paso 1:  El CNC, para dar inicio al movimiento consulta el estado de posición, si ningún fin de carrera esta activo, se desplaza hasta la posición de inicio de corte. 
Paso 2: Para iniciar el corte, el elevador de torcha desciende hasta hacer contacto con la chapa, y luego asciende una distancia predeterminada para que el escudo no esté en contacto con la chapa al momento del pinchazo, lo que provocaría el desgaste prematuro de consumibles. 
Paso 3: La fuente recibe la señal de corte y activa el proceso de arco piloto, comprueba la señal de arco transferido durante un tiempo determinado y de sostenerse activa continua e inicia el sensado de altura. El sensado de altura se obtiene mediante la medición del voltaje de arco establecido entre el electrodo de corte y el material que se esta cortando. Este voltaje varía, dependiendo de muchos factores, principalmente la velocidad de desplazamiento y el espesor de corte. Para obtener la medición se utiliza un divisor de voltaje colocado en la fuente de plasma para adaptar la señal a valores que pueda medir el THC. La fuente de plasma entrega voltajes que van desde los 100v a los 200v , se utiliza un divisor de voltaje de relación 1:50 para acondicionar esos niveles. Si la tensión de arco excede los limites se emite una señal de alarma y se detiene el movimiento y el proceso de corte. 
Paso 4: se desplaza hasta el siguiente pinchazo y se repite desde el paso 1.
Esta es una simplificación de todo el funcionamiento ya que existen muchos mas pasos dependiendo de la configuración y programación del CNC, como ser bloqueo del THC, disminucion de velocidad en curvas, detección de colisión, detección de sangría, etc.
 
## 2.4 DESARROLLO DE LA INSTALACION
El desafío principal de este desarrollo fue poder identificar como funcionaban los distintos dispositivos y como se vinculaban entre ellos, ya que no tenía disponible esa información de parte del fabricante de la maquina por razones de confidencialidad, lo que llevo a hacer una ingeniería inversa de la maquina
El proceso de actualización se dividió en 3 etapas, 
1-	Documentación de salidas y entradas del cnc, cableado, relés etc.
2-	Desmontaje del tablero y adaptación al nuevo sistema CNC.
3-	Montaje y conexión del nuevo sistema CNC
2.4.1	Relevamiento y documentación del sistema original

Se comenzó con el desmontaje parcial del gabinete eléctrico, relevando y documentando todas las conexiones existentes. Esto incluyó:
- Identificación de señales de entrada y salida del controlador CNC original.
- Mapeo de señales de los drivers Leadshine y la lógica de control del THC Hypertherm.
- Trazado de los circuitos de relés, fuentes de alimentación y protecciones existentes.
- Registro fotográfico y esquemático para asegurar la correcta reconexión en la nueva etapa.
Esta etapa fue crucial para asegurar la compatibilidad del nuevo sistema CNC con los componentes a conservar (drivers y motores), así como para establecer una base segura para el cableado del nuevo hardware.
2.4.2	Desmontaje del sistema obsoleto y preparación del tablero

Una vez identificadas las conexiones críticas, se procedió al desmontaje completo del controlador CNC Gundel y del sistema de control de altura original. Esto implicó:
- Eliminación de cables, relés y tarjetas innecesarias.
- Limpieza y reacondicionamiento del gabinete de control.
- Incorporación de nuevas canalizaciones, borneras y etiquetas para un cableado ordenado y accesible.
- Adecuación de la fuente de 24 VDC existente, en función de los requerimientos del CNC FangLing y el nuevo módulo THC.

### 2.4.3 Instalación y puesta en funcionamiento del nuevo sistema

En esta etapa se llevó a cabo el montaje físico del sistema CNC FangLing F2100 y el módulo de control de altura HYD XPTHC-4H, así como el conexionado según los esquemas provistos en el manual técnico del fabricante. Las tareas incluyeron:
- Conexión de señales de movimiento (pulso/dirección) entre CNC y drivers Leadshine.
- Conexión de entradas/salidas digitales para los finales de carrera, torch on/off, señal de "arc OK" y control de altura.
- Ajuste de parámetros en el software de configuración del CNC (pasos por mm, aceleraciones, secuencia de inicio, alturas de perforado y corte, etc.).
- Pruebas de funcionalidad por etapas: movimientos manuales, test de señal de arco, control de altura dinámico, ejecución de archivos G-code de prueba.
- Verificación de protecciones eléctricas y comportamiento ante fallas (paro de emergencia, alarmas de tensión de arco, colisión de antorcha).

## 3. CONCLUSION

La actividad desarrollada durante esta Práctica Profesional Supervisada representó una excelente oportunidad para aplicar de manera concreta y contextualizada los conocimientos adquiridos a lo largo de la carrera de Ingeniería Mecatrónica. En particular, pude integrar conceptos de automatización industrial, sistemas embebidos, electrónica de potencia, programación y control de procesos, en un entorno real de trabajo con maquinaria de corte por plasma.
La actualización tecnológica de un sistema CNC implicó no solo una intervención técnica sobre hardware y software, sino también un análisis crítico del funcionamiento del sistema original, su diagnóstico, la toma de decisiones fundamentadas para su reemplazo, y la validación del nuevo sistema en condiciones de operación. Esta experiencia reforzó mi capacidad de análisis sistémico, interpretación de documentación técnica en inglés, y resolución de problemas técnicos concretos, muchas veces sin apoyo del fabricante, lo que exigió autonomía, criterio técnico y toma de decisiones responsables.
Asimismo, la interacción entre el nuevo CNC, el controlador de altura y la fuente de plasma me permitió profundizar en la lógica de funcionamiento de sistemas de control en tiempo real, afinar mis conocimientos sobre señales de entrada/salida, gestión de alarmas, tensiones industriales y protocolos de comunicación. Esto me ayudó a consolidar competencias en integración de tecnologías heterogéneas, una habilidad central para el perfil de un ingeniero mecatrónico.
Finalmente, destaco que esta práctica profesional fortaleció no solo mis conocimientos técnicos, sino también habilidades blandas como la planificación de tareas, trabajo colaborativo con operarios y técnicos, gestión del tiempo y comunicación clara de avances. Considero que fue una experiencia valiosa y altamente formativa, que me permitió vincular los contenidos académicos con una aplicación directa en el entorno productivo, reafirmando mi vocación y compromiso con la mejora continua en los procesos industriales.

 

ANEXO 1 - EXPLICACION DE PUERTOS

En la siguiente imagen podemos observar la interfaz del sistema CNC, consta de conectores standard DB25 y DB15, en nuestro caso solo utilizaremos los conectores CN1, CN2,CN3 y los terminales de alimentación 24v CC.
<img width="586" height="493" alt="image" src="https://github.com/user-attachments/assets/3ff72212-8c13-4dcc-b87e-c9b0608ade95" />
<img width="378" height="291" alt="image" src="https://github.com/user-attachments/assets/9deb6f61-68aa-4250-bdcb-0ab8ef76e38a" />


La señal de entrada proviene de un interruptor de contacto mecánico; se admiten tanto el tipo normalmente abierto como el tipo normalmente cerrado.
Es efectiva cuando está conectada a 24VG, y es inefectiva cuando está en estado flotante o conectada a 24V.
El terminal común (COM) del interruptor externo se conecta a 24VG. El otro terminal se conecta al puerto de E/S correspondiente.


 ## Puertos de entrada
<img width="439" height="620" alt="image" src="https://github.com/user-attachments/assets/7ba8966c-986f-416d-b82f-fabb8b25d6fc" />


## Puertos de salida

<img width="570" height="602" alt="image" src="https://github.com/user-attachments/assets/2bd2c8fa-fe0b-45df-a20a-b03d4b419f5c" />

## Diagrama de cableado CNC Fangling con modulo de control de altura automático

<img width="938" height="719" alt="image" src="https://github.com/user-attachments/assets/ab58a2a6-c80c-4527-815b-96d65fd95642" />


 




  



