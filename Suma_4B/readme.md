# Arquitectura de procesadores Grupo5  
# Integrantes:
-  Santiago Medrano Poveda.
- Julian Pulido Aguirre.
- Sofia Arias Novoa
## Documentacion
# Funcionamiento Sumador de 4 Bits
# Sumador de 4 Bits usando Sumadores de 1 Bit

Este proyecto contiene un módulo Verilog llamado `LABSUM1` que implementa un **sumador de 4 bits** utilizando instancias de sumadores de 1 bit (`sum1b`). El diseño es jerárquico, reutilizando el módulo `sum1b` para realizar la suma bit a bit, y propagando los acarreo intermedios entre cada etapa del sumador.

## Descripción del Módulo `LABSUM1`

El módulo tiene las siguientes entradas y salidas:

### Entradas
- `A[3:0]`: Un número de 4 bits que representa el primer operando.
- `B[3:0]`: Un número de 4 bits que representa el segundo operando.

### Salidas
- `So[3:0]`: La suma de los dos números de 4 bits (`A` y `B`).
- `Cout`: El acarreo de salida del bit más significativo.

### Funcionamiento

Este módulo es un **sumador de 4 bits** que se construye a partir de cuatro instancias del módulo `sum1b`, el cual es un sumador de 1 bit completo. A medida que se suman los bits correspondientes de `A` y `B`, el acarreo generado por cada sumador se propaga al siguiente bit más significativo. 

Los sumadores de 1 bit están conectados de la siguiente manera:

1. `sum1b s0`: Suma el bit menos significativo (`A[0]` y `B[0]`) y no recibe acarreo de entrada (se fija en 0). Genera un acarreo de salida (`c1`) que se pasa al siguiente sumador.
2. `sum1b s1`: Suma el siguiente bit de `A` y `B` (`A[1]` y `B[1]`), utilizando el acarreo de salida de la etapa anterior (`c1`) como acarreo de entrada. Genera un nuevo acarreo de salida (`c2`).
3. `sum1b s2`: Suma el tercer bit de `A` y `B` (`A[2]` y `B[2]`), con el acarreo de entrada proveniente de la etapa anterior (`c2`). Genera el acarreo de salida `c3`.
4. `sum1b s3`: Suma el bit más significativo (`A[3]` y `B[3]`), utilizando el acarreo de entrada de la etapa anterior (`c3`). El acarreo final (`Cout`) es la salida del sumador completo.

### Código Verilog

```verilog
// `include "sum1b.v"

module LABSUM1(
    input  [3:0] A,
    input  [3:0] B,
    output       Cout,
    output [3:0] So
);

    wire c1, c2, c3;

    sum1b s0 (.A(A[0]), .B(B[0]), .Ci(1'b0), .Co(c1), .So(So[0]));
    sum1b s1 (.A(A[1]), .B(B[1]), .Ci(c1), .Co(c2), .So(So[1]));
    sum1b s2 (.A(A[2]), .B(B[2]), .Ci(c2), .Co(c3), .So(So[2]));
    sum1b s3 (.A(A[3]), .B(B[3]), .Ci(c3), .Co(Cout), .So(So[3]));

endmodule