# Arquitectura de procesadores Grupo5  
# Integrantes:
-  Santiago Medrano Poveda.
- Julian Pulido Aguirre.
- Sofia Arias Novoa
## Documentacion
# Funcionamiento Sumador de 1 Bit
# Sumador de 1 Bit 

Este proyecto contiene un módulo Verilog que implementa un **sumador completo de 1 bit** con acarreo . Este sumador realiza la suma de dos bits (`A` y `B`) junto con un bit de acarreo de entrada (`Ci`), generando como salida el resultado de la suma (`So`) y el acarreo de salida (`Co`).

## Descripción del Módulo `sum1b`

El módulo tiene tres entradas y dos salidas:

### Entradas
- `A`: Primer bit de entrada.
- `B`: Segundo bit de entrada.
- `Ci`: Acarreo de entrada.

### Salidas
- `So`: Suma de los bits de entrada.
- `Co`: Acarreo de salida.

### Funcionamiento

Este módulo se comporta como un **sumador completo**, lo que significa que toma dos bits (`A` y `B`) y un acarreo de entrada (`Ci`), y produce una suma (`So`) y un acarreo de salida (`Co`).

Internamente, se utiliza un registro de 2 bits para almacenar el resultado de la suma:

- `So = result[0]`: Contiene el bit de la suma.
- `Co = result[1]`: Contiene el acarreo de salida.

### Código Verilog

```verilog
module sum1b(
    input A, 
    input B, 
    input Ci,
    output wire Co,
    output wire So
);

    reg [1:0] result;

    assign So = result[0];
    assign Co = result[1];

    always @(*) begin
        result = A + B + Ci;
    end

endmodule