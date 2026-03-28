[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/xB5owuT7)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23014482&assignment_repo_type=AssignmentRepo)
# Lab03: Visualización interactiva de datos en Raspberry Pi usando Python y Matplotlib

## Integrantes

Grupo 9
Juan Sebastian Organista
Yesid Fabian Alfonso
Edwar Raul Carrero

Yesid Fabián Alfonso Pérez - 987081,
Edwar Raúl Carrero Ayala - 64325202,
Sebastián organista -10953333




## Documentación

<img width="269" height="521" alt="image" src="https://github.com/user-attachments/assets/66b00ba7-f1fb-47c4-90d2-7069654ecfca" />



## Preguntas
1. ¿Qué función cumple ```plt.fignum_exists(self.fig.number)``` en el ciclo principal?

Esta función verifica si la ventana de la gráfica aún existe y está abierta. Si el usuario cierra manualmente la ventana de matplotlib, plt.fignum_exists() retorna False, lo que hace que el ciclo while termine y el programa finalice correctamente, evitando errores al intentar graficar en una ventana que ya no existe.


2. ¿Por qué se usa ```time.sleep(self.intervalo)``` y qué pasa si se quita? 

time.sleep(self.intervalo) controla la frecuencia de muestreo, estableciendo un intervalo fijo (0.5 segundos) entre mediciones. Si se quita:
El programa tomaría mediciones lo más rápido posible, saturando el sensor y el procesador
La gráfica se actualizaría constantemente, consumiendo muchos recursos
Se perdería la relación temporal deseada entre mediciones
Podría causar interferencias en las lecturas del sensor ultrasónico

3. ¿Qué ventaja tiene usar ```__init__``` para inicializar listas y variables?

Encapsulación: Todas las variables de estado están claramente definidas desde el inicio
Consistencia: El objeto siempre se crea en un estado válido y predecible
Reusabilidad: Permite crear múltiples instancias independientes del monitor
Mantenibilidad: Centraliza la configuración inicial, facilitando modificaciones
Legibilidad: Define claramente qué variables pertenecen al estado del objeto

4. ¿Qué se está midiendo con ```self.inicio = time.time()```?

self.inicio captura el timestamp absoluto del momento en que comienza la ejecución del monitor. Luego se usa para calcular el tiempo relativo transcurrido (ahora = time.time() - self.inicio), permitiendo que el eje X de la gráfica muestre cuántos segundos han pasado desde el inicio, independientemente de la hora real del sistema.

5. ¿Qué hace exactamente ```subprocess.check_output(...)```?

Ejecutar comandos del sistema operativo y capturar su salida
En un contexto de sensores, podría usarse para leer vcgencmd para medir temperatura de la CPU
Captura la salida estándar del comando y la retorna como bytes

6. ¿Por qué se almacena ```ahora = time.time() - self.inicio``` en lugar del tiempo absoluto?

El eje X comienza en 0, mostrando claramente la duración del monitoreo
No importa a qué hora se inició, la gráfica muestra el tiempo transcurrido
Los valores son más pequeños y manejables para la escala de la gráfica
Facilita comparar diferentes sesiones de monitoreo

7. ¿Por qué se usa ```self.ax.clear()``` antes de graficar?

Evitar superposición: Sin clear(), los nuevos gráficos se dibujarían sobre los anteriores
Tambien elimina los elementos gráficos antiguos
Asegura que todos los elementos (títulos, etiquetas, leyendas) se refresquen correctamente
Es más eficiente limpiar y redibujar que modificar elementos existentes

8. ¿Qué captura el bloque ```try...except``` dentro de ```leer_temperatura()```?

Errores de timeout: Cuando el sensor no responde dentro del tiempo esperado
Excepciones de GPIO: Problemas con los pines, interferencias eléctricas
Errores de cálculo: Posibles divisiones por cero o valores inválidos
Cualquier excepción general: Previene que el programa completo se detenga por un error de medición aislado

9. ¿Cómo podría modificar el script para guardar las temperaturas en un archivo .```csv```?

python
import csv
import os

class MonitorUltrasonicoRPI:
    def __init__(self, duracion_max=60, intervalo=0.5, archivo_csv="mediciones.csv"):
        # ... código existente ...
        self.archivo_csv = archivo_csv
        self._inicializar_archivo_csv()
    
    def _inicializar_archivo_csv(self):
        """Inicializa el archivo CSV con encabezados si no existe"""
        archivo_nuevo = not os.path.exists(self.archivo_csv)
        with open(self.archivo_csv, mode='a', newline='') as file:
            writer = csv.writer(file)
            if archivo_nuevo:
                writer.writerow(['Tiempo (s)', 'Distancia (cm)', 'Timestamp'])
    
    def guardar_medicion_csv(self, tiempo, distancia):
        """Guarda una medición en el archivo CSV"""
        try:
            with open(self.archivo_csv, mode='a', newline='') as file:
                writer = csv.writer(file)
                timestamp = time.strftime('%Y-%m-%d %H:%M:%S')
                writer.writerow([tiempo, distancia, timestamp])
        except Exception as e:
            print(f"Error al guardar en CSV: {e}")
    
    def actualizar_datos(self):
        # ... código existente ...
        ahora = time.time() - self.inicio
        distancia = self.medir_distancia()
        
        if distancia is not None:
            self.tiempos.append(ahora)
            self.distancias.append(distancia)
            self.guardar_medicion_csv(ahora, distancia)  # Nueva línea
            print(f" Tiempo: {ahora:5.1f}s | Distancia: {distancia:6.2f} cm")
        else:
            print(f"  Tiempo: {ahora:5.1f}s | Fuera de rango/Error")
    
    def _limpiar_recursos(self):
        # ... código existente ...
        print(f"\n Mediciones guardadas en: {self.archivo_csv}")

