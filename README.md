# Wiki-Workshop3

## Descripción General
Este proyecto es una simulación de un sistema de monitoreo de niveles de líquido utilizando un PLC programado en **OPENPLC** y simulado en una interfaz HMI en **CODESYS**. Se implementó lógica en **Ladder Logic (LD)** para controlar el estado de un tanque representado mediante **LEDs**.

## Objetivos
- Simular el monitoreo de niveles de un tanque mediante LEDs.
- Implementar y probar lógica de control en **OPENPLC**.
- Desarrollar una interfaz HMI en **CODESYS** para la visualización de los estados del sistema.
- Probar la integración con **OpenPLC** con un circuito fisico.

## Diseño del Circuito y Lógica de Control
### Tablas de Comportamiento y Mapas de Karnaugh
Para realizar el diseño lógico del sistema, inicialmente se crearon tablas de comportamiento que identifican claramente los estados esperados en cada salida (LED) según todas las combinaciones posibles de entradas (sensores). Posteriormente, para simplificar y optimizar las funciones booleanas asociadas a cada salida, se utilizaron mapas de Karnaugh. Esta técnica permitió minimizar el número de elementos lógicos necesarios, facilitando una implementación eficiente tanto en la simulación en CODESYS como en la implementación práctica en OpenPLC.

### Diagramas Ladder (LD)
El programa de control sigue esta lógica:
1. **LED de nivel bajo encendido**: Indica que el tanque está vacío o por debajo del umbral mínimo.
2. **LED de nivel correcto encendido**: Indica que el nivel está dentro del rango adecuado.
3. **LED de nivel alto encendido**: Indica un posible desbordamiento.
4. **LED de vacio alto encendido**: Indica que no hay nada en el tanque.
5. **LED de error**: Se activa si los sensores detectan una condición inválida.

El diagrama Ladder fue diseñado en **CODESYS** y probado con una simulación en HMI.
![image](https://github.com/user-attachments/assets/c405d96d-30e6-4d2b-a786-454811628a39)

## Implementación en CODESYS
El programa fue implementado en **CODESYS**, con las siguientes consideraciones:
- Uso de **contactos y bobinas** para representar sensores y LEDs.
- Condiciones lógicas para activar los LEDs según el estado del sistema.
- Interfaz HMI para monitoreo visual.
El funcionamiento es el siguiente: 
1. **Lectura de Sensores:** Se evalúan los estados de los sensores B3 (sobrellenado), B2 (nivel mínimo) y B1 (tanque vacío).
2. **Activación de LEDs:**
   - Si B1, B2 y B3 están activos, se enciende H3 (amarillo).
   - Si B1 está activo, se enciende H2 (azul).
   - Si B1 y B2 están activo, se enciende H1 (verde).
   - Si ninguno esta activo, se enciende H4 (rojo)
   - Si hay una condición invalida, se activa H5 (error).
3. **Botones de comiezo y parada:** Se utilizan para comenzar el funcionamiento del proceso, sin el boton de **START** no se puede comenzar la imulación y si pasa algo durante el proceso el boton **STOP** para el proceso y reinicia las variables. 

![image](https://github.com/user-attachments/assets/d63da023-5f69-4ee4-8f70-2d2ad5faebbe)


## Implementación en OpenPLC *(Por completar)*
La lógica diseñada originalmente en CODESYS fue replicada en OpenPLC, manteniendo la esencia del control Ladder. Debido a limitaciones físicas, se omitieron los botones START y STOP. Las entradas (B1, B2, B3) se asignaron respectivamente a %IX0.0, %IX0.1 y %IX0.2, mientras que las salidas (LEDs H1 a H5) se asignaron de %QX0.0 a %QX0.4.
Se generaron tablas de comportamiento y mapas de Karnaugh para optimizar las funciones booleanas correspondientes a cada salida, facilitando una implementación eficiente y precisa.

![image](https://github.com/user-attachments/assets/1bab8ddc-8028-4718-95fb-caf66454bba9)


## Implementación Fisica 
Se utilizó un microcontrolador ESP32 conectado a un protoboard, empleando LEDs como indicadores visuales y botones táctiles (NC) para simular sensores con lógica negativa. Las entradas fueron conectadas a los puertos GPIO 36, 39 y 34; y las salidas digitales (LEDs indicadores) a los puertos GPIO 32, 33, 25, 26 y 27. La configuración del circuito fue realizada cuidadosamente para facilitar el montaje y asegurar un funcionamiento claro y eficiente.

![WhatsApp Image 2025-04-05 at 20 36 04](https://github.com/user-attachments/assets/f6ff82a7-67b4-44d4-b349-226eada398fe)


## Pruebas y Resultados
Se realizaron pruebas en el entorno de simulación, confirmando el correcto funcionamiento del sistema. Los LEDs respondieron de manera adecuada según la lógica implementada.
Ademas, se realizó nuevamente la programación en **OpenPLC** para implementar y controlar con la ayuda de un SP32 el circuito fisico.

## Conclusión
El código Ladder se implementó con éxito en la simulación y en hardware real usando OpenPLC, validando la lógica de control de los sensores y LEDs en cada fase del proceso. Esto permitió aprender sobre estructuración de secuencias en Ladder, validación de estados, integración con OpenPLC y depuración de errores, fortaleciendo conocimientos en automatización y control de procesos.

## Referencias 
[1] OpenAI, "ChatGPT," [Modelo de lenguaje de gran tamaño]. [En línea]. Disponible: https://chat.openai.com/chat. [Accedido: 1-abr-2025].  

[2] Scantime Automation & Training, “An Introduction to HMI/Visualization Programming with CODESYS!,” YouTube, 7 de octubre de 2021, [Video en línea]. Disponible en: https://www.youtube.com/watch?v=eXs28TuCrBg. [Accedido: 28-mar-2025].
