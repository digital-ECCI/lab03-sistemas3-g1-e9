[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/xB5owuT7)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23014482&assignment_repo_type=AssignmentRepo)
# Lab03: Visualización interactiva de datos en Raspberry Pi usando Python y Matplotlib

## Integrantes

Yesid Fabián Alfonso Pérez - 987081,
Edwar Raúl Carrero Ayala - 64325202,
Sebastián organista -10953333



## Documentación

Realizar la medición de temperatura del sistema.

Inicio
  │
  ▼
Guardar tiempo inicial
  │
  ▼
Leer temperatura del sistema
  │
  ▼
Calcular tiempo transcurrido
  │
  ▼
Guardar datos
  │
  ▼
Actualizar gráfica
  │
  ▼
¿Ventana abierta?
  │
  ├─ Sí → repetir proceso
  │
  └─ No → finalizar programa

## Preguntas

1. ¿Qué función cumple ```plt.fignum_exists(self.fig.number)``` en el ciclo principal?

R/ Permite que el programa siga ejecutándose solo mientras la ventana de la gráfica esté abierta.

2. ¿Por qué se usa ```time.sleep(self.intervalo)``` y qué pasa si se quita

R/ detiene el programa durante un tiempo determinado esto nos ayuda a tener un mayo control del sispositivo como por ejemplo la medicon de la temperatura, tambiaen Evitar que el ciclo se ejecute demasiado rápido es decir reducir el uso del CPU.


3. ¿Qué ventaja tiene usar ```__init__``` para inicializar listas y variables?

R/ es el constructor de la clase permitiendo inicializar todas las variables cuando se crea el objeto, tambien permite organizar mejor el código.

Evita errores por variables no definida permitiendo reutilizar la clase fácilmente por ejemplo de lo que se inicializa normalmente:

listas de tiempo

listas de temperatura

configuración de la gráfica

intervalo de medición

en resumen sirve para prepara el objeto antes de que empiece a funcionar el programa.

4. ¿Qué se está midiendo con ```self.inicio = time.time()```?

R/ devuelve el tiempo actual del sistema en segundos, por ejemplo se está guardando el momento exacto en que empieza el programa.Luego se usa para calcular cuánto tiempo ha pasado desde el inicio.

5. ¿Qué hace exactamente ```subprocess.check_output(...)```?

R/ ejecuta un comando del sistema operativo desde Python. Ejecuta el comando del sistema. Captura lo que el comando devuelve y lo entrega a Python como texto. En este caso se usa para leer la temperatura del CPU de la Raspberry Pi.

6. ¿Por qué se almacena ```ahora = time.time() - self.inicio``` en lugar del tiempo absoluto?

R/  Porque se quiere medir el tiempo transcurrido desde que empezó el programa.

7. ¿Por qué se usa ```self.ax.clear()``` antes de graficar?

R/ borra la gráfica anterior ya que esto es necesario porque el programa está actualizando la gráfica continuamente, si no se usara las líneas se dibujarían encima unas de otras y la gráfica se volvería confusa y saturada.

Con clear():

Se limpia el gráfico.

Se dibuja la nueva actualización.

[Inicio]
     │
     ▼
[Guardar tiempo inicial
 inicio = time.time()]
     │
     ▼
[Ejecutar ciclo del programa]
     │
     ▼
[Leer tiempo actual
 time.time()]
     │
     ▼
[Calcular tiempo transcurrido
 ahora = tiempo_actual - inicio]
     │
     ▼
[Usar tiempo en gráfica o datos]

8. ¿Qué captura el bloque ```try...except``` dentro de ```leer_temperatura()```?

R/ El bloque try...except captura errores al leer la temperatura puesto a que el comando del sistema falla. No se encuentra el sensor lo que ocasiona problemas de permisos, error al convertir el dato.

9. ¿Cómo podría modificar el script para guardar las temperaturas en un archivo .```csv```?

R/ Se puede usar el módulo csv se ejecutaría cada vez que se mida la temperatura dentro del ciclo. Si se quisiera obtener una grafica que guarde los datos en CSV automáticamente esto se podria lograr para Raspberry Pi o Linux.