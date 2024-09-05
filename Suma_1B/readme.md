# Arquitectura de procesadores Grupo5  
# Integrantes:
-  Santiago Medrano Poveda.
- Julian Pulido Aguirre.
- Sofia Arias Novoa
## Documentacion
# Funcionamiento Sumador de 1 Bit
# Resumen
En Arquitectura de procesadores, los sumadores de 1 bit son componentes fundamentales para operaciones aritméticas más complejas, como la adición de números de múltiples bits. Entonces en este laboratorio realizamos distintos tipos de sumadores de 1 bit implementados en este laboratorio del primer corte 
# Sumador sum1b.v

En este laboratorio realizamos el **sumador completo de 1 bit** con acarreo . Este sumador realiza la suma de dos bits (`A` y `B`) junto con un bit de acarreo de entrada (`Ci`), generando como salida el resultado de la suma (`So`) y el acarreo de salida (`Co`).

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

En este laboratorio realizamos un **sumador completo**, lo que significa que toma dos bits (`A` y `B`) y un acarreo de entrada (`Ci`), y produce una suma (`So`) y un acarreo de salida (`Co`) justo como lo explicado en clase.

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
```
# Sumador sum1b_primitive

En este segundo codigo proporcionado por la docente es otra forma de realizar un **sumador completo de 1 bit** usando primitivas lógicas básicas como AND, XOR y OR. El sumador toma dos bits de entrada (`A` y `B`) junto con un bit de acarreo de entrada (`Ci`) y produce una suma (`So`) y un acarreo de salida (`Co`).

## Descripción del Módulo `sum1b_primitive`

El módulo tiene tres entradas y dos salidas:

### Entradas
- `A`: Primer bit de entrada.
- `B`: Segundo bit de entrada.
- `Ci`: Acarreo de entrada.

### Salidas
- `So`: Suma de los bits de entrada.
- `Co`: Acarreo de salida.

### Funcionamiento

Este sumador implementa un **full adder** utilizando puertas lógicas básicas:

- Primero, calcula el **producto** de los bits `A` y `B` con una puerta `AND` (`a_ab`).
- Luego, calcula el **XOR** de `A` y `B` (`x_ab`), que es la suma parcial sin considerar el acarreo.
- Después, utiliza otra puerta `XOR` para sumar el acarreo de entrada `Ci` al resultado parcial `x_ab`, generando la suma final `So`.
- Para calcular el acarreo de salida (`Co`), se usan dos puertas `AND` y una puerta `OR`:
  - La puerta `AND` (`cout_t`) se usa para calcular el acarreo generado por `x_ab` y `Ci`.
  - Finalmente, el acarreo de salida `Co` es el resultado de la operación `OR` entre `cout_t` y `a_ab`.

### Código Verilog

```verilog
module sum1b_primitive (
    input A, 
    input B, 
    input Ci,
    output wire Co,
    output wire So);
  
    wire a_ab;
    wire x_ab;
    wire cout_t;
  
    and(a_ab, A, B);
    xor(x_ab, A, B);
  
    xor(So, x_ab, Ci);
    and(cout_t, x_ab, Ci);
  
    or (Co, cout_t, a_ab);
endmodule
```
# Tabla de Verdad para comprobar del Sumador de 1 Bit

Esta tabla muestra como es el funcionamiento de un **sumador de 1 bit** , que toma dos bits de entrada (`A` y `B`), junto con un acarreo de entrada (`Ci`), y genera una suma (`So`) y un acarreo de salida (`Co`) como fue explicado en el laboratorio en clase.

## Tabla de Verdad

La siguiente es la **tabla de verdad** para el sumador de 1 |bit:
```
|  A  |  B  | Ci  | Co  | So  |
| --- | --- | --- | --- | --- |
|  0  |  0  |  0  |  0  |  0  |
|  0  |  0  |  1  |  0  |  1  |
|  0  |  1  |  0  |  0  |  1  |
|  0  |  1  |  1  |  1  |  0  |
|  1  |  0  |  0  |  0  |  1  |
|  1  |  0  |  1  |  1  |  0  |
|  1  |  1  |  0  |  1  |  0  |
|  1  |  1  |  1  |  1  |  1  |
```
### Explicación:

- **Entradas**:
  - **A**: Primer bit de entrada.
  - **B**: Segundo bit de entrada.
  - **Ci**: Acarreo de entrada.

- **Salidas**:
  - **So**: Suma de los bits de entrada.
  - **Co**: Acarreo de salida.

#### Ejemplos:
1. **Cuando `A = 0`, `B = 1`, y `Ci = 0`**:
   - La salida es **`So = 1`** y **`Co = 0`**.
   
2. **Cuando `A = 1`, `B = 1`, y `Ci = 1`**:
   - La salida es **`So = 1`** y **`Co = 1`** (la suma total es 3, que en binario es `11`).

Con esta **tabla de verdad** podemos verificar su funcionamiento de el **sumador completo** de 1 bit funcione correctamente. mostrando las combinaciones posibles con ejemplos (`A`, `B`, `Ci`), y los resultados obtenidos (sumas y acarreo) deben coincidir con los valores de **So** y **Co** en la tabla.

# Simulacion