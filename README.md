# Arquitectura TOY-8

Nombre y apellido: Vanina Spinazzola

## Instrucciones

- Forkear este repo
- Completar nombre y apellido
- Responder editando este mismo archivo
- Pushear a GitHub y pasarme el link de su fork (el repo tiene que ser público)


Tienen un emulador de la computadora y el circuito para el Logisim en el [blog](https://la35.net/orga/emulador.html). Usenlos sabiamente para responder.

**Tip:** si editan en el Atom tienen un _preview_ de Markdown tocando `Ctrl + Shift + M` mientras editan este archivo.
## Ejercicios

1. Considerar el siguiente programa de TOY-8 en lenguaje máquina. Traducir a hexadecimal en la tercer columna y a ensamblador en la cuarta como se muestra en la primera línea. ¿Cuál es el valor de 0xE cuando el programa se detiene? ¿Qué es lo que hace este programa si 0xE es el resultado del mismo?

```
0x1:  10101011    #  ab  #  lw B
0x2:  11101010    #  ea  #  bze A
0x3:  00101101    #  2d  #  add D
0x4:  11001011    #  cb  #  sw B
0x5:  10101110    #  ae  #  lw E
0x6:  00101100    #  2c  #  add C
0x7:  11001110    #  ce  #  sw E
0x8:  10100000    #  a0  #  lw 0
0x9:  11100001    #  e1  #  bze 1
0xA:  00000000    #  00  #  halt 0
0xB:  00000003    #  03  #  halt 3
0xC:  00000006    #  06  #  halt 6
0xD:  11111111    #  ff  #  255
0xE:  00000000    #  00  #  halt 0
```

**RTA:** En 0xE se guarda un 18 (12 en hexadecimal), y esto es porque el programa se encarga de multiplicar los numeros en 0xB y 0xC.

2. Consideren el siguiente _hexdump_ de la memoria de TOY-8. O sea un volcado de la memoria en hexadecimal. ¿Cuántos programas distintos pueden encontrar? Indicar cuáles bytes interpretan como instrucciones y cuáles como datos.

```
0x0   00 A5 26 C7
0x4   00 08 05 00
0x8   A7 6D 2E C7
0xc   00 FF 01 00
```

**RTA:** Hay 2 programas, el primero va desde 0x1 hasta 0x4. Las direcciones 0x1, 0x2, 0x3 y 0x4 almacenan instrucciones. Las direcciones 0x5, 0x6 y 0x7 almacenan datos. 
```
0x1: A5 #lw 5 
0x2: 26 #add 6
0x3: C7 #sw 7
0x4: 00 #halt
```
El segundo va desde 0x8 hasta 0xc. Las direcciones 0x8, 0x9, 0xA, 0xB y 0xC son instrucciones. Las direcciones 0xD y 0xE son datos.
```
0x8: A7 #lw 7 
0x9: 6D #xor D
0xA: 2E #add E
0xB: C7 #sw 7
0xC: 00 #halt
```

3. Para el primer programa del ejercicio anterior. ¿Qué líneas de control se activan para cada instrucción? ¿Cuál es el valor del bus de datos y de instrucciones en cada instrucción? Completen la siguiente tabla, agreguen las filas que sean necesarias.

|Instrucción|Reloj|Control|Data bus|Address Bus|
|---|---|-------------------------|---|---|
|A5 |0  |IR en                    |A5 |1  |
|A5 |1  |R en, addr mux           |08 |5  |
|26 |0  |IR en                    |26 |2  |
|26 |1  |R en, addr mux, ALUOp(00)|05 |6  |
|C7 |0  |IR en                    |C7 |3  |
|C7 |1  |RAM str, addr mux        |0D |7  |
|00 |0  |IR en                    |00 |4  |
|00 |1  |halt, addr mux           |00 |0  |

4. El siguiente programa suma los números que encuentra en la entrada hasta que aparece un cero, y luego envía el resultado a la salida. Traducirlo a ensamblador y a C siguiendo el ejemplo de las primeras dos líneas.

```
0x1:  A0   #  lw   0  #
0x2:  CE   #  sw   E  #  int suma = 0;
0x3:  AF   #  lw   F  #  scanf("%d", &num);
0x4:  E9   #  bze  9  #  while (num != 0) {
0x5:  2E   #  add  E  #    suma += num;
0x6:  CE   #  sw   E  #    
0x7:  A0   #  lw   0  #    scanf("%d", &num);
0x8:  E3   #  bze  3  #  }
0x9:  AE   #  lw   E  #  
0xA:  CF   #  sw   F  #  printf("%d", sum);
0xB:  00   #  halt    #
```

5. Una mejora que le podríamos hacer a esta computadora es duplicar la cantidad de memoria, pasar de 16 bytes a 32 bytes. ¿Cómo lo harían manteniendo la longitud de las instrucciones en 8 bits? ¿Qué partes de la CPU habría que modificar y cómo?

**RTA:** Cambiando el registro PC, la entrada de direcciones del I/O y el tamaño de la memoria (junto con todos los multiplexores de por medio), ahora manipularian 5 bits en vez de 4. Además, el address tendría los 5 ultimos bits ( bits 0, 1, 2, 3, 4) para poder acceder a la memoria.
