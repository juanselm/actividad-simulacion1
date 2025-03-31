# Actividad de seguimiento - Simulación 1

|Integrante|correo|usuario github|
|---|---|---|
|Juan Sebastian Loaiza Mazo|juans.loaiza@udea.edu.co|juanselm|
|Nombre completo integrante 2|correo integrante 2|gihub user integrante 2|
|Nombre completo integrante 3|correo integrante 3|gihub user integrante 3|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).

**Importante**:
* Como la actividad es en las parejas del laboratorio, solo uno de los integrantes tiene que hacer el fork; y sobre repositorio bifurcado que se genera, la modificación se realiza en equipo.
* Como la entrega se debe hacer modificando el archivo READNE, se recomienda que consulte mas sobre el lenguaje **Markdown**. En el repo adjuntan dos cheatsheet ([cheat sheet 1](Markdown_Cheat_Sheet.pdf), [cheatsheet 2](markdown-cheatsheet.pdf)) para consulta rapida.
* Entre mas creativo mejor.

## Homework (Simulation)

This program, [`process-run.py`](process-run.py), allows you to see how process states change as programs run and either use the CPU (e.g., perform an add instruction) or do I/O (e.g., send a request to a disk and wait for it to complete). See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-intro/README.md) for details.

### Questions

1. Run `process-run.py` with the following flags: `-l 5:100,5:100`. What should the CPU utilization be (e.g., the percent of time the CPU is in use?) Why do you know this? Use the `-c` and `-p` flags to see if you were right.
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

2. Now run with these flags: `./process-run.py -l 4:100,1:0`. These flags specify one process with 4 instructions (all to use the CPU), and one that simply issues an I/O and waits for it to be done. How long does it take to complete both processes? Use `-c` and `-p` to find out if you were right. 
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

3. Switch the order of the processes: `-l 1:0,4:100`. What happens now? Does switching the order matter? Why? (As always, use `-c` and `-p` to see if you were right)
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

4. We'll now explore some of the other flags. One important flag is `-S`, which determines how the system reacts when a process issues an I/O. With the flag set to SWITCH ON END, the system will NOT switch to another process while one is doing I/O, instead waiting until the process is completely finished. What happens when you run the following two processes (`-l 1:0,4:100 -c -S SWITCH ON END`), one doing I/O and the other doing CPU work?
   
   <details>
   <summary>Answer</summary>
   <p><b>Comando</b></p>

   ```python
   python3 process-run.py -l 1:0,4:100 -c -S SWITCH_ON_END
   ```

   <img src="images/process4.png" alt="Process 5" style="display: block; margin: 0 auto; width: 80%; height: auto;">

   <br>

   ##### Explicación.

   Al ejecutat el comando anterior se simulan dos procesos:
   - **PID 0:** Este proceso ejecuta una operación de I/O.
   - **PID 1:** Este proceso ejecuta instrucciones sobre la CPU.
   - **SWITCH_ON_END:** Esta opción usada en el comando indica que solo puede ser intercambiado los procesos cuan se termine la ejecución del primero.

   <br>

   |PID|Tiempo 1|Tiempo 2-6|Tiempo 7|Tiempo 8-11|
   |---|---|---|---|---|
   |**0**|Está en ejecución con una operación de I/O.|Permanece en estado BLOCKED|Finaliza su proceso I/O|Ha finalizado (DONE).|
   |**1**|Está READY esperando su turno|Permanece READY sin ejecución.|Continua en estado Ready|Inicia ejecución en CPU hasta terminar|

   ##### Conclusión:
   El cambio de proceso solo ocurre cuando uno termina, en este caso hasta que termine el PID 0. Esto causa ineficiencia, ya que la CPU está inactiva durante el tiempo en que PID 0 está bloqueado y luego de eso el PID 1 puede iniciar su ejecución.

   </details>
   <br>

5. Now, run the same processes, but with the switching behavior set to switch to another process whenever one is WAITING for I/O (`-l 1:0,4:100 -c -S SWITCH ON IO`). What happens now? Use `-c` and `-p` to confirm that you are right.
   
   <details>
   <summary>Answer</summary>
   <p><b>Comando</b></p>

   ```python
   python3 process-run.py -l 1:0,4:100 -c -S SWITCH_ON_IO
   ```

   <img src="images/process5.png" alt="Process 5" style="display: block; margin: 0 auto; width: 80%; height: auto;">
   <br>

   ##### Explicación.

   Al ejecutat el comando anterior se simulan dos procesos:
   - **PID 0:** Este proceso ejecuta una operación de I/O.
   - **PID 1:** Este proceso ejecuta instrucciones sobre la CPU.
   - **SWITCH_ON_IO:** Cuando uno de los procesos entra en estado I/O se puede cambiar de proceso.

   <br>

   |PID|Tiempo 1|Tiempo 2-6|Tiempo 7|Tiempo 8-11|
   |---|---|---|---|---|
   |**0**|Empieza la ejecución con una operación de I/O|Se queda en estado BLOCKED esperando su I/O.|Permanece bloqueado esperando el fin de su I/O. |finaliza su proceso I/O|
   |**1**|Inicia su estado en READY|Usa la CPU (RUN: cpu) sin necesidad de que PID 0 halla terminado.|Finaliza su proceso|Ya ha finalizado su proceso|

   ##### Conclusión:
   En este caso se optimiza el uso de la CPU, ya que mientras PID 0 está bloqueado esperando la I/O, PID 1 usa la CPU, evitando tiempos muertos.

   </details>
   <br>

6. One other important behavior is what to do when an I/O completes. With `-I IO RUN LATER`, when an I/O completes, the process that issued it is not necessarily run right away; rather, whatever was running at the time keeps running. What happens when you run this combination of processes? (`./process-run.py -l 3:0,5:100,5:100,5:100 -S SWITCH ON IO -c -p -I IO RUN LATER`) Are system resources being effectively utilized?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

7. Now run the same processes, but with `-I IO RUN IMMEDIATE` set, which immediately runs the process that issued the I/O. How does this behavior differ? Why might running a process that just completed an I/O again be a good idea?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.