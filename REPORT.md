# Lab 5 Integration and SOA - Project Report

## 1. EIP Diagram (Before)

![Before Diagram](diagrams/before.png)

Describe what the starter code does and what problems you noticed.
En el código inicial, se implementa un sistema de integración utilizando patrones de Enterprise Integration Patterns (EIP) con Spring Integration. El sistema recibe una secuencia de números enteros y 
los procesa a través de varios canales y filtros para clasificarlos en números pares y impares.
Sin embargo en dicho código existen múltiples problemas que afecta a su correcto funcionamiento:
- El filtro en oddFlow para identificar números impares no está implementado correctamente, lo que provoca que no se filtren adecuadamente, dejando pasar números pares y rechazando los impares.
- El @Gateway estaba apuntando a evenChannel en lugar de oddChannel, lo que causaba que los números negativos se trataran como pares.
- El canal oddChanel no estaba definido de manera explícita, por lo que Spring creaba un DirectChannel por defecto, lo que podía causar problemas de concurrencia y pérdida de mensajes si había múltiples suscriptores.
---

## 2. What Was Wrong

Explain the bugs you found in the starter code:

- **Bug 1**: What was the problem? Why did it happen? How did   you fix it?
-En primer lugar, el código inicial tenía un problema en la lógica del filtro a aplicar en el canal de números primos
- **Bug 2**: What was the second problem? Why did it happen? How did you fix it?
- En segundo lugar, el oddChanel estaba configurado como un DirectChannel, lo que provocaba inconsistencias en el procesamiento de mensajes. 
- Por ello se ha modificado a un PublishSubscribeChannel para asegurar que todos los suscriptores recibieran los mensajes correctamente.
- **Bug 3**: What was the third problem? Why did it happen? How did you fix it? 
- Cuando se envía numero negativo, el @Gateway estaba apuntando al canal incorrecto (evenChannel en lugar de oddChannel).
- **(More bugs if you found them)**

---

## 3. What You Learned

Write a few sentences about:

- What you learned about Enterprise Integration Patterns
- How Spring Integration works
- What was challenging and how you solved it

---

## 4. AI Disclosure

**Did you use AI tools?** (ChatGPT, Copilot, Claude, etc.)

- If YES: Which tools? What did they help with? What did you do yourself?
- If NO: Write "No AI tools were used."

**Important**: Explain your own understanding of the code and patterns, even if AI helped you write it.

---

## Additional Notes

Any other comments or observations about the assignment.



Componentes del esquema
Item arriba izq. Es un puller que está ejecutando algo de manera repetida.
El de abajo es justo su contrapar, un gateway, por el cual cualquier aplicacion pueded meter cualquier valor al sistema, lo que le pase por parámetros lo
mete al canal.
Si es par se va al canal par, el ultimo handler leerá el valor y hará algo con él.
En el canal odd meteremos los impares, el canal de abajo es un canal pub/sub 
UN canal por defecto es un canal directo, donde cuando se public el mensaje el se lleva la copia.
En el canal pub/sub cada uno se lleva una copia del mensaje.