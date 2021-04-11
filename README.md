# lab01- sumador 
laboratorio 01 introducción a HDL

### Presentado por: Manuel Alejandro Pérez Carvajal

Para dar inicio al laboratorio, se realizó la descarga de Quartus y el primer acercamiento a este software mediante la implementación de un sumador de 1 bit. Sin embargo, en este primer intento, se contó con un inconveniente que venía dado al abrir el archivo lab01-sumador (el cual tiene el primer código), donde Quartus arrojaba un error; la solución a este problema fue muy sencilla y con la eliminación de las carpetas db, incremental_db y simulation, fue suficiente para corregirlo ya que Quartus estaba tomando archivos que contenían información de una versión pasada.

Como se mencionó, el primer código para el sumador de un bit, estaba dado en la guía y en los archivos clonados. Sin embargo, se hizo un análisis de este para entender un poco el lenguaje de programación.

## Sumador de 1 bit
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
Siguiente a esto, se forzó las dos entradas para validar todas las posibles entradas y el funcionamiento del sumador. Sin embargo, al dar valores manualmente a las entradas, se observa que es un proceso algo extenso y lo mejor es implementar el TestBench de modo que tengamos todos los valores posibles en las entradas sin tener que modificar uno a uno y así poder representar las salidad de mejor manera. El TestBench utilizado para el sumador de 1 bit fue el siguiente:
```verilog
module sum1bcc_TB;

  reg x;
  reg y;
  reg c;
  wire out;
  wire z;


sum1bcc uut(x, y, c,out,z);


initial begin
forever begin
x=0; y=0; c=0; #3;
x=0; y=0; c=1; #3;
x=0; y=1; c=0; #3;
x=0; y=1; c=1; #3;
x=1; y=0; c=0; #3;
x=1; y=0; c=1; #3;
x=1; y=1; c=0; #3;
x=1; y=1; c=1; #3;
end


end

initial begin: TEST_CASE
     $dumpfile("sum1bcc_TB.vcd");
     $dumpvars(-1, uut);
     #(200) $finish;
   end

endmodule
```

![Sumador1bcc](/images/TBsum1bcc.png)

Allí notamos que funciona perfectamente como un sumador de 1 bit.

## Sumador de 4 Bits
Para realizar el sumador de 4 bits básicamente se hizo uso del sumador de 1bit instanciando este sumador 4 veces. El código a utilizar fue el siguiente:
```verilog
`timescale 1ns / 1ps
module sum4b(xi, yi,co,zi);


  input [3 :0] xi;
  input [3 :0] yi;
  output co;
  output [3 :0] zi;

  wire c1,c2,c3;
  sum1bcc s0 (.A(xi[0]), .B(yi[0]), .Ci(0),  .Cout(c1) ,.S(zi[0]));
  sum1bcc s1 (.A(xi[1]), .B(yi[1]), .Ci(c1), .Cout(c2) ,.S(zi[1]));
  sum1bcc s2 (.A(xi[2]), .B(yi[2]), .Ci(c2), .Cout(c3) ,.S(zi[2]));
  sum1bcc s3 (.A(xi[3]), .B(yi[3]), .Ci(c3), .Cout(co) ,.S(zi[3]));



endmodule
````
Seguido a esto y antes de observar el RTL simulation, generamos el TestBench con el ánimo de cumplir los mismos objetivos anteriormente dichos en el sumador de 1 bit. El TestBench utilizado es el siguiente:
```verilog
`timescale 1ns / 1ps
module sum4b_TB;

  reg [3:0] A;
  reg [3:0] B;

  wire co;
  wire [3:0] S;

  sum4b uut (
    .xi(A), 
    .yi(B), 
    .co(co), 
    .zi(S)
  );

  
  initial begin
  
    A=0;
	 for (B = 0; B < 16; B = B + 1) begin
      if (B==0)
        A=A+1;
      #5 ;
		$display("El valor de %d + %d = %d",A, B,S) ;
    end
	
  end      

  initial begin: TEST_CASE
     $dumpfile("sum4b_TB.vcd");
     $dumpvars(-1, uut);
     #(1200) $finish;
   end


endmodule
````
A partir de esto, se utiliza el RTL simulation para poder observar las salidas resultantes debido a todas las entradas posibles.

![Sumador1bcc](/images/TBsum4b.png)

## Implementación de la FPGA
Lo primero que se debe realizar para implementar la FPGA en Quartus, es realizar la asignación de la tarjeta que estamos usando, como se puede observar en la siguiente imagen:

![Sumador1bcc](/images/fpga-asignada.png)

Posterior a ello, debemos asignar los pines que usaremos para sintetizar el programa. En este caso sinterizaremos el sumador de 4 bits y la asignación de pines queda de la siguiente manera:
![Sumador1bcc](/images/pines.png)
### Netlist Viewers
Dentro de la implementación, tendremos la oportunidad de visualizar las conexiones entre los pines de entrada y salida junto a las cajas lógicas utilizadas en el programa. Un ejemplo es el RTL Viewer el cual se observa a continuación:

![Sumador1bcc](/images/rtlviewer.png)

Otra visualización importante está dada por el technology map viewer (post-mapping), donde se evidencias las conexiones realizadas entre el sumador y los pines utilizados de la fpga. Esta visualización se puede observar en la siguiente imagen:
![Sumador1bcc](/images/postmapping.png)
Continuando con la implementación de la FPGA, no queda más que conectarla mediante el usb blaster y alimentar la tarjeta (instalando los certificados y drivers previamente). Sintetizando el programa, obtenemos los siguientes resultados en la tarjeta:
![Sumador1bcc](/images/resultado1.jpg)
![Sumador1bcc](/images/resultado2.jpg)
![Sumador1bcc](/images/resultado3.jpg)
![Sumador1bcc](/images/resultado4.jpg)
![Sumador1bcc](/images/resultado5.jpg)
![Sumador1bcc](/images/resultado6.jpg)
![Sumador1bcc](/images/resultado7.jpg)
![Sumador1bcc](/images/resultado8.jpg)