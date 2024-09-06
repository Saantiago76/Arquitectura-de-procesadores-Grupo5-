# Arquitectura de procesadores Grupo5  
# Integrantes:
-  Santiago Medrano Poveda.
- Julian Pulido Aguirre.
- Sofia Arias Novoa
## Documentacion
# Funcionamiento Sumador de 4 Bits
# Resumen
Los sumadores de 4 bits son componentes esenciales en el diseño de circuitos aritméticos digitales y mas en nuestro uso cotidiano como ingenieros que nos permiten realizar la suma de dos números binarios de 4 bits y manejar el acarreo resultante cual explicado en la clase. Entonces en este laboratorio detallamos los codigos proporcionados por la docente su tabla de verdad y simulacion del sumador de 4 bits y su testbench:
# Sumador de 4 Bits usando Sumadores de 1 Bit

Este laboratorio contiene un módulo Verilog llamado `Sum4b` que implementa un **sumador de 4 bits** utilizando instancias de sumadores de 1 bit (`sum1b`) **vistos anteriormente en los sumadores de 1 bit**. El diseño es jerárquico, reutilizando el módulo `sum1b` para realizar la suma bit a bit, y propagando los acarreo intermedios entre cada etapa del sumador.

## Descripción del Módulo `Sum4b`

El módulo tiene las siguientes entradas y salidas:

### Entradas
- `A[3:0]`: Un número de 4 bits que representa el primer operando.
- `B[3:0]`: Un número de 4 bits que representa el segundo operando.

### Salidas
- `Sum[3:0]`: La suma de los dos números de 4 bits (`A` y `B`).
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
//`include "sum1b.v"

module sum4b (
        input  [3:0] A,
        input  [3:0] B,
        output       Cout,
        output [3:0] Sum
    );

  wire c1,c2,c3;
  wire c_out; 


  sum1b s0 (.A(A[0]), .B(B[0]), .Ci(1'b0),  .Cout(c1) ,.Sum(Sum[0]));
  sum1b s1 (.A(A[1]), .B(B[1]), .Ci(c1), .Cout(c2) ,.Sum(Sum[1]));
  sum1b s2 (.A(A[2]), .B(B[2]), .Ci(c2), .Cout(c3) ,.Sum(Sum[2]));
  sum1b s3 (.A(A[3]), .B(B[3]), .Ci(c3), .Cout(Cout) ,.Sum(Sum[3]));

endmodule
```
# Descripción del Sumador de 4 Bits (`sum4b_tb`)

Este archivo de testbench (`sum4b_tb.v`) está diseñado para verificar el correcto funcionamiento de un **sumador de 4 bits** (`sum4b`), simulando diferentes combinaciones de entradas y observando las salidas.

## Entradas y Salidas
- **Entradas**:
  - `A_tb`: Registro de 4 bits que representa el primer número a sumar.
  - `B_tb`: Registro de 4 bits que representa el segundo número a sumar.
  
- **Salidas**:
  - `So_tb`: Cable de 4 bits que contiene el resultado de la suma.
  - `Co_tb`: Cable que contiene el acarreo de salida (si la suma de los 4 bits desborda).

## Descripción del Proceso

1. **Inicialización de A y B**:
   - El registro `A_tb` se inicializa en **0**.
   - El ciclo `for` incrementa el valor de `B_tb` desde 0 hasta 14 para realizar pruebas con diferentes combinaciones de `A_tb` y `B_tb`.
   - Cada vez que `B_tb` vuelve a 0, el valor de `A_tb` se incrementa en 1, lo que permite probar diferentes combinaciones de entrada para ambos números.

2. **Simulación Temporal**:
   - Después de cada iteración, se espera un lapso de **5 unidades de tiempo** antes de mostrar el resultado en la consola.
   - La función `$display` imprime en la consola los valores actuales de `A_tb`, `B_tb` y la salida `So_tb`, lo que te permite verificar que el sumador de 4 bits está funcionando como se espera.

3. **Terminación de la Simulación**:
   - Cuando el ciclo ha probado todas las combinaciones, la simulación termina con `$finish`.

4. **Volcado de Resultados**:
   - Se utiliza `$dumpfile` y `$dumpvars` para generar un archivo `VCD` (cambio de valores de señales) que puedes abrir en un visor como **GTKWave**. Esto permite analizar gráficamente el comportamiento del sumador a lo largo del tiempo.
### Código Verilog
```
//include "sum4b.v"
//timescale 1ns/1ns

module sum4b_tb();

    reg [3:0] A_tb;
    reg [3:0] B_tb;
  
    wire Co_tb;
    wire [3:0] So_tb;
  
    // Instantiate the Unit Under Test (UUT)
    sum4b uut (
      .A(A_tb), 
      .B( B_tb), 
      .Co(Co_tb), 
      .So(So_tb)
    );
  
  initial begin
    A_tb=0;
    for ( B_tb = 0;  B_tb < 15;  B_tb =  B_tb + 1) begin
      if ( B_tb==0) begin
        A_tb=A_tb+1;
      end
      #5 $display("el valor de %d + %d = %d", A_tb, B_tb,So_tb);
    end
    $finish;
  end      

  initial begin: TEST_CASE
		$dumpfile("sum4b_tb.vcd");
		$dumpvars(-1, uut);
	end
  
  endmodule
  ```
# Tabla de Verdad para comprobar del Sumador de 4 Bit

Dado que cada entrada es un número de 4 bits (A y B), la tabla muestra los resultados para algunos casos representativos. El sumador también produce un acarreo de salida (Co).

Definiciones:
A[3:0]: Primer número de 4 bits.
B[3:0]: Segundo número de 4 bits.
So[3:0]: Resultado de la suma de A y B.
Co: Acarreo de salida.
| A[3:0] | B[3:0] | Co | So[3:0] |
|--------|--------|----|---------|
| 0000   | 0000   | 0  | 0000    |
| 0000   | 0001   | 0  | 0001    |
| 0001   | 0001   | 0  | 0010    |
| 0001   | 0010   | 0  | 0011    |
| 0001   | 0011   | 0  | 0100    |
| 0010   | 0010   | 0  | 0100    |
| 0010   | 0100   | 0  | 0110    |
| 0011   | 0011   | 0  | 0110    |
| 0111   | 0001   | 0  | 1000    |
| 0111   | 0010   | 0  | 1001    |
| 0111   | 0111   | 1  | 1110    |
| 1111   | 1111   | 1  | 1110    |
| 1111   | 0001   | 1  | 0000    |


#### - A[3:0] y B[3:0] son números binarios de 4 bits.
#### - So[3:0] es el resultado de sumar A y B en binario, con 4 bits de ancho.
#### - Co es el acarreo de salida, que se produce cuando la suma de los dos números de 4 bits y el acarreo de entrada excede el rango de 4 bits (es decir, cuando el resultado es mayor que 1111 en binario).

###  La tabla de verdad proporciona una representación de cómo debería comportarse el sumador de 4 bits para varias combinaciones de entrada. Nosotros hacemos este ejemplo ya que nos muestra y verifica el funcionamiento del sumador.

# Simulación
Esta simulación, que realizamos con el simulador que viene configurado por defecto en el instalador de **Quartus**, muestra el sumador de 4 bits, donde se están sumando dos números binarios de 4 bits, representados por las entradas A y B, junto con las salidas Sum (resultado de la suma) y Cout (carry-out).

![alt text](<Imagen de WhatsApp 2024-09-05 a las 17.30.34_e8c61f82.jpg>)

### Detalles de la simulación:
1. **Entradas**:
- *A:* La entrada de 4 bits, que cambia su valor en intervalos de tiempo. Por ejemplo, en los primeros 80 ns, el valor de A es `0111` (7 en decimal), luego cambia a 1110 (14 en decimal), y así sucesivamente.

- *B:* La segunda entrada de 4 bits, que también cambia a diferentes valores durante la simulación. Por ejemplo, comienza en `1110` (14 en decimal), luego pasa a 1011 (11 en decimal), etc.

2. **Salidas:**

- *Sum:* Es la salida que representa el resultado de la suma de las entradas A y B. Por ejemplo, cuando A es `0111` y B es `1110`, la salida Sum es `0101`, lo que corresponde a 21 en decimal (7 + 14 = 21). La salida aquí solo muestra los 4 bits menos significativos del resultado.

- *Cout:* Es el carry-out, que indica si hubo un acarreo más allá de los 4 bits. En esta simulación, se muestra que Cout es 1 cuando se genera un acarreo. Esto ocurre, por ejemplo, cuando la suma de A y B excede los 4 bits (por ejemplo, cuando se suma 7 y 14, cuyo resultado es 21, y Cout se activa para reflejar el acarreo).

### Análisis temporal:
En los primeros 80 ns, cuando `A = 0111 y B = 1110`, el resultado de la suma es `10101`. El bit menos significativo es almacenado en Sum (0101), y el bit más significativo de la suma (carry) se refleja en Cout, que es 1.
En los siguientes intervalos, la simulación sigue cambiando los valores de A y B, mostrando cómo varían Sum y Cout en función de las combinaciones.

# Conclusión 
Los sumadores de 4 bits nos permiten sumar números binarios de hasta 4 bits de manera eficiente, propagan los acarreos de bit a bit, y gestionan el acarreo final. El testbench correspondiente asegura que el sumador funciona correctamente mediante pruebas sistemáticas y proporciona una herramienta para la validación visual de la simulación.