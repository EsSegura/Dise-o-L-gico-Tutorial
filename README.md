# Desarrollador
## Esteban Segura Garcia 2022129260

# Nombre del proyecto
## Tutorial de diseño logico 

## 1. Abreviaturas y definiciones
- **FPGA**: Field Programmable Gate Arrays
- **clk**; Clock
- **rst**; Restart

## 2. Referencias
[0] David Harris y Sarah Harris. *Digital Design and Computer Architecture. RISC-V Edition.* Morgan Kaufmann, 2022. ISBN: 978-0-12-820064-3
[1] David Medina. (2024, August 1). Tutorial: herramientas os FPGA TangNano9k [Video]. YouTube. https://www.youtube.com/watch?v=AKO-SaOM7BA

## 3. Desarrollo

## El código consta de dos módulos en Verilog: uno para la implementación del contador que controla los LEDs (module_counter) y otro para la simulación de este módulo (module_counter_tb).

### 3.0 Descripción general del sistema

## El sistema diseñado es un contador de leds controlado por una FPGA, utilizando un módulo de contador parametrizable en Verilog. Este contador tiene la capacidad de contar hasta un valor específico "COUNT" determinado por el usuario y luego incrementar un contador de leds. El valor de este contador se invierte y se presenta en la salida count_o. El módulo de simulación (module_counter_tb) permite verificar el funcionamiento correcto del contador mediante una simulación en la que se observa el comportamiento de las señales de reloj "clk", reinicio "rst" y la salida del contador "count_o". Esta simulación asegura que el contador y el sistema de leds funcionen como se espera antes de la implementación en la fpga.

### 3.1 Módulo 1
#### 1. Encabezado del módulo "design"
module module_counter #
(
    parameter COUNT = 13500000
) 
(
    input logic clk,
    input logic rst,
    output logic [5 : 0] count_o
);

    localparam WIDTH_COUNT = $clog2(COUNT);

    logic [WIDTH_COUNT - 1 : 0] clk_counter = '0;
    logic [5 : 0] led_count_r;

    always_ff @ (posedge clk) begin
        if (!rst) begin
            led_count_r <= '0;
            clk_counter <= '0;
        end 
        else if (clk_counter == COUNT) begin
            clk_counter <= '0;
            led_count_r <= led_count_r + 1'b1;
        end 
        else begin
            clk_counter <= clk_counter + 1'b1;
            led_count_r <= led_count_r;
        end
end

assign count_o = ~led_count_r;

endmodule

#### 2. Encabezado del módulo "sim"
`timescale 1ns/1ps

module module_counter_tb;

    logic clk;
    logic rst;
    logic [5 : 0] count_o;

    module_counter # (10) COUNTER 
    (
        .clk (clk),
        .rst (rst),
        .count_o (count_o)
    );

    initial begin
    clk = 0;
    rst = 1;

    #30;
    
    rst = 0;

    #30;
    
    rst = 1;

    # 300000;
    $finish;
    end

    always begin
        clk = ~clk;
        #10;
    end

    initial begin
        $dumpfile("module_counter_tb.vcd");
        $dumpvars(0, module_counter_tb);
    end

endmodule

#### 4. Criterios de diseño
Diagramas, texto explicativo...

#### 5. Testbench
## En este apartado se probarán las pruebas hechas solo en el vc:
## La siguiente imagen corresponde al constr, donde se puede ver las variables con sus respectivos nombres:

![image](https://github.com/user-attachments/assets/474c34a7-09f1-4c38-9457-1c8c46d4867b)

## En la siguiente imagen, se puede observar los datos modificados según video:

![image](https://github.com/user-attachments/assets/617d6039-5636-4a67-8187-9017e4ad521c)

## En las siguientes imagenes se muestran la consola con los comandos funcionando:

![image](https://github.com/user-attachments/assets/3fe64847-e385-4c28-9299-90945d2c88f3)

## La prueba de que funcionó el WV:

![image](https://github.com/user-attachments/assets/9d782c89-54f6-4d87-b94c-a0f794fe1678)

## En la siguiente imagen se muestra el "make bitstream" funcionando, el "make load" tira error porque todavía no se prueba con una fpga:

![image](https://github.com/user-attachments/assets/d579fb14-1cd6-440d-bf88-a6dda2adcd71)

## 4. Consumo de recursos

## 5. Problemas encontrados durante el proyecto
## El primer problema encontrado fue que no instalé un programa en el cual, no me permitía ejecutar los comandos "make", luego de  instalarlos, los pude ejecutar de manera correcta.

## Apendice

![image](https://github.com/user-attachments/assets/7d9e1a71-4347-4f1c-9d29-dda03fb1046e)

## Estadisticas 

![image](https://github.com/user-attachments/assets/66028093-e5ad-4b8b-86f7-cbe63a9915d6)

