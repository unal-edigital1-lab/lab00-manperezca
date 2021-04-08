# lab01- sumador 
laboratorio 01 introducción a HDL

### Presentado por: Manuel Alejandro Pérez Carvajal

Para dar inicio al laboratorio, se realizó la descarga de Quartus y el primer acercamiento a este software mediante la implementación de un sumador de 1 bit. Sin embargo, en este primer intento, se contó con un inconveniente que venía dado al abrir el archivo lab01-sumador (el cual tiene el primer código), donde Quartus arrojaba un error; la solución a este problema fue muy sencilla y con la eliminación de las carpetas db, incremental_db y simulation, fue suficiente para corregirlo ya que Quartus estaba tomando archivos que contenían información de una versión pasada.

Como se mencionó, el primer código para el sumador de un bit, estaba dado en la guía y en los archivos clonados. Sin embargo, se hizo un análisis de este para entender un poco el lenguaje de programación.

# Sumador de 1 bit
Se hizo uso del siguiente código:
```verilog
module sum1bcc (A, B, Ci,Cout,S);

  input  A;
  input  B;
  input  Ci;
  output Cout;
  output S;

  reg [1:0] st;

  assign S = st[0];
  assign Cout = st[1];

  always @ ( * ) begin
  	st  = 	A+B+Ci;
  end
  
endmodule
```
Siguiente a esto, se forzó las dos entradas para validar todas las posibles entradas y el funcionamiento del sumador. Sin embargo, al dar valores manualmente a las entradas, se observa que es un proceso algo extenso y lo mejor es implementar el TestBench para que así le dé los valores posibles a las entradas y poder notar las salidas.

![Sumador1bcc](/images/TBsum1bcc.png)



 

