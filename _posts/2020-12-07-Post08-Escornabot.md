---
layout: post
title: "Robot Educativo Escornabot"
author: wilmer
categories: [Robots]
tags: [StepperMotors]
image: assets/images/Post08/EscornabotPortadaRoja.jpeg
description: "How to make from 0 to 100% finished an educational  mobile robot of open source hardware"
featured: false
hidden: false
rating: 5
beforetoc: "En esta página encontrarás toda la información para construir el robot educativo Escornabot en su versión Brivoi Compactus"
toc: true
date:   2020-12-07 08:00:00 -0600
---

Este robot en su versión *Brivoi Compactus* está basado en el proyecto: 
<a href="https://escornabot.com/es/index">Escornabot</a> desarrollado en España por una amplia comunidad de ingenieros, makers y profesores para enseñar robótica y programación a niños de primaria y secundaria. 

<style>
  /* Three image containers (use 25% for four, and 50% for two, etc) */
  .column {
    float: left;
    width: 46%;
    padding: 2%;
  }

  /* Clear floats after image containers */
  .row::after {
    content: "";
    clear: both;
    display: table;
  }

  img {
    border-radius: 5%;
  }

  video {
    border-radius: 5%;
  }

  p {
    text-align: justify;
  }

  figure {
    text-align: justify;
  }

  table, th, td {
    border: 1px solid black;
    border-collapse: collapse;
    padding-left: 15px;
    padding-right: 15px;
  }
</style>


<p>
  También puedes visitar la página de Pablo de Cierzo: 
  <a href="https://pablorubma.cc/">pablorubma.cc</a>
  quien ha llevado a estos robots "chiquitines" a muchos niños también en España con su lema: 
  "Pon un Escornabot en tu vida".
</p>

## Introducción
<p>
  Escornabot es un robot móvil de software y hardware abierto diseñado para la enseñanza de 
  electrónica y programación en niños, adolescentes y no tan niños. Basado en la 
  tarjeta <a href="https://store.arduino.cc/usa/arduino-nano">Arduino Nano</a>, destaca la 
  sencillez de su diseño mecánico, electrónico y la estructura del código con el 
  cual funciona. Todo esto ha sido posible por la comunidad que lo desarrolla y 
  mantiene actualizado. 
</p>
<p>
  Existen diversas versiones y la que corresponde a este manual 
  es la <b>Brivoi Compactus</b> que se puede observar en la Figura 2.
  El <a href="https://cad.onshape.com/documents/840de37dec1e20b94d9912ff/w/254b42c01cad12e8c66ac520/e/163fa93be055b155f8c058d3">
    modelo 3D </a> es público y se encuentra disponible para hacer una copia, y compartirlo. 
</p>

<figure style="text-align: center; width:70%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/3DParts/portadaOnshape_1.png"
    alt="Modelo 3D del Escornabot Brivoi Compactus" width="100%"/>
  <figcaption>
    Figura 2: Modelo 3D del Escornabot Brivoi Compactus
  </figcaption>
</figure>
<p>
  Esta versión utiliza las dos siguientes tarjetas de circuitos impresos (PCB) 
  desarrolladas por <a href="https://github.com/xdesig">XDeSIG</a> y cuya documentación
  se encuentra aquí:
</p>

<ul>
  <li><a href="https://github.com/xdesig/escornabot-electronics/tree/master/Electronics/E_KEY/E_KEYPAD/E_KEYPAD_2.20">E-KeyPad V2.2</a></li>
  <li><a href="https://github.com/xdesig/escornabot-electronics/tree/master/Electronics/EscornaCPU/1.x/1.20">EscornaCPU V1.2</a></li>
</ul>

### Partes Impresas en 3D
<p>
  El cuerpo del Escornabot está compuesto por 7 partes impresas en 3D que se muestran en la 
  Figura 3 y que se pueden descargar en formato STL a partir de los vínculos de la siguiente lista
  donde se describen.
</p>

<figure style="text-align: center; width:90%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/3DParts/Exploded_view_1.png"
    alt="Vista Explosionada de las partes impresas en 3D" width="100%"/>
  <figcaption>
    Figura 3: Vista Explosionada de las partes impresas en 3D
  </figcaption>
</figure>

<ol>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/Motor-Bracket.stl">
  Motor-Bracket: </a> Parte central del robot a la que se fijan los motores y algunas de las 
      otras piezas impresas en 3D.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/EscornaCPU_1.2-Bracket.stl">
    EscornaCPU_V1.2-Bracket:</a> Parte a la que se fija la tarjeta de control.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/E-Keypad_2.2-Bracket.stl">E-KeypadV2.2-Bracket:</a> Parte a la que se fija la tarjeta de la botonera.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/Battery-Bracket.stl">
    Battery-Bracket:</a> 
    Parte en donde van alojadas las 4 baterías AA que alimentan el Escornabot.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/Wheel-Right.stl">
    Wheel-Right:</a> Rueda derecha del Escornabot.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/Wheel-Left.stl">
    Wheel-Left:</a> Rueda izquierda del Escornabot.</li>
  <li><a href="https://github.com/wgaonar/Escornabot-Brivoi_Compactus/blob/801b0207904528a2303c471870367c1062eae421/STL/Ball-Caster.stl">
    Ball-Caster:</a> Parte que aloja la esfera que sirve de pivote para que el Escornabot 
      pueda desplazarse y girar.</li>
</ol>

### Lista de componentes
<p>
  La Tabla 1 detalla la cantidad y descripción de todos lo componentes electrónicos y mecánicos
  utilizados para el Escornabot y también la botonera y la tarjeta de control, en las que irán 
  colocados. 
</p>

<table>
    <colgroup>
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 80%;">
    </colgroup>
  <caption>Tabla 1: Lista de Componentes para el Escornabot</caption>
  <tr>
    <th style="text-align:center">CANT</th>
    <th style="text-align:center" >DESCRIPCIÓN</th>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Arduino Nano</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Driver ULN2803</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Zócalo de 18 pines para el driver ULN2803</td>
  </tr>
  <tr>
    <td style="text-align:center">4</td>
    <td>Resistencia 1 KΩ</td>
  </tr>
  <tr>
    <td style="text-align:center">9</td>
    <td>Resistencia 10 KΩ</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 18 KΩ o de 20KΩ</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 22 KΩ o de 20KΩ</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Azul</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Rojo</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Amarillo</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Verde</td>
  </tr>
  <tr>
    <td style="text-align:center">5</td>
    <td>Pulsadores de 12mm</td>
  </tr>
  <tr>
    <td style="text-align:center">15</td>
    <td>Pines/headers macho rectos</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Puente o Jumper para pines rectos</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Tira de 15 pines hembra para colorar el Arduino Nano</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Tira de 4 pines hembra para colocar un adaptador Bluetooth</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Interruptor ON/OFF SK12F14 o SK12D07</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>2 Motor paso a paso 28BYJ-48</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Buzzer activo de 5V para Arduino</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Condensadores cerámicos 104 de 100nF</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Diodo Schottky 1N5817</td>
  </tr>
  <tr>
    <td style="text-align:center">6</td>
    <td>Cables Dupont hembra - hembra de 10cm</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Fusible rearmable XF050</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Conector macho JST-XHP-5 para motor paso a paso 28BYJ-48</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Conector macho JST-XHP-2 para alimentación de la tarjeta de control</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Portapilas cuadrado para 4 pilas AA</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Canica de 17 mm</td>
  </tr>
  <tr>
    <td style="text-align:center">3</td>
    <td>Tornillo Allen con cabeza de botón o cilíndrica M3 x 12mm</td>
  </tr>
  <tr>
    <td style="text-align:center">12</td>
    <td>Tornillo Allen con cabeza de botón o cilíndrica M3 x 6mm</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td><a href="https://www.pcbway.com/project/shareproject/E_KEYPAD_2_2.html">Tarjeta E-KeyPad V2.2</a></td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td><a href="https://www.pcbway.com/project/shareproject/W50475ASN23_Escorna_CPU_1_20.html">Tarjeta de control EscornaCPU V1.2</a></td>
  </tr> 
</table>

## Ensamble de la botonera
<p>  
  La botonera utilizada es la
  <a href="https://www.pcbway.com/project/shareproject/E_KEYPAD_2_2.html">Tarjeta E-KeyPad V2.2</a>
  que se puede ver en la Figura 4:
</p>


<figure style="text-align: center; width:80%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/Botonera/botonera_inicial.jpg"
    alt="botonera_inicial.jpg" width="100%"/>
  <figcaption>
    Figura 4: Botonera E-KeyPad V2.2
  </figcaption>
</figure>

### Componentes
Los componentes requeridos y su correspondiente función se enlistan en la Tabla 2:

<table>
    <colgroup>
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 70%;">
    </colgroup>
  <caption>Tabla 2: Descripción y funcionamiento de los componentes requeridos para la botonera</caption>
  <tr>
    <th style="text-align:center">CANT</th>
    <th style="text-align:center">DESCRIPCIÓN</th>
    <th style="text-align:center">ETIQUETA</th>
    <th style="text-align:center">FUNCIÓN</th>
  </tr>
  <tr>
    <td style="text-align:center">4</td>
    <td>Resistencia 1 KΩ</td>
    <td style="text-align:center">R6, R8, R9, R10</td>
    <td>Resistencias para la activación de los Leds de la botonera</td>
  </tr>
  <tr>
    <td style="text-align:center;" rowspan="2">5</td>
    <td rowspan="2">Resistencia 10 KΩ</td>
    <td style="text-align:center">R1, R2, R3, R4</td>
    <td>En conjunto con los botones, conforman el divisor de voltaje que permiten elegir el movimiento que el esconarbot realizará:
      <ul>
        <li>R1 conectada con el botón S1 selecciona un movimiento hacia 
        <span style="color:white; background-color: blue;"> ADELANTE </span> </li>
        <li>R2 conectada con el botón S2 selecciona un movimiento hacia la
        <span style="color:white; background-color: red;"> IZQUIERDA </span> </li>
        <li>R3 conectada con el botón S3 selecciona un movimiento hacia
        <span style="color:black; background-color: yellow;"> ATRÁS </span> </li>
        <li>R4 conectada con el botón S5 realiza la secuencia de movimentos 
        <span style="color:white; background-color: black;"> GO </span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">R7</td>
    <td><span style="color:red;">Opcional: </span> Está presente en la botonera en caso de que se use una tarjeta de control diferente a la EscornaCPU V1.2 y en la que no se utilice la resistencia interna de PULL-UP del Arduino Nano. En el firmware del robot se tendría que definir la palabra clave: KEYBOARD_WIRES con el valor de 3. 
    Por precaución, esta resistencia se puede soldar</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 22 KΩ o de 20KΩ</td>
    <td style="text-align:center">R5</td>
    <td>La última resistencia del divisor de voltaje que conectada con el botón S4 selecciona un movimiento hacia la
    <span style="color:white; background-color: green;"> DERECHA </span> </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm azul</td>
    <td style="text-align:center">Led1</td>
    <td>Led indicador de un movimiento hacia
    <span style="color:white; background-color: blue;"> ADELANTE </span></td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm rojo</td>
    <td style="text-align:center">Led2</td>
    <td>Led indicador de un movimiento hacia la
    <span style="color:white; background-color: red;"> IZQUIERDA </span></td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm amarillo</td>
    <td style="text-align:center">Led3</td>
    <td>Led indicador de un movimiento hacia
    <span style="color:black; background-color: yellow;"> ATRÁS </span> </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm verde</td>
    <td style="text-align:center">Led4</td>
    <td>Led indicador de un movimiento hacia la
    <span style="color:white; background-color: green;"> DERECHA </span> </td>
  </tr>
  <tr>
    <td style="text-align:center;" rowspan="2">5</td>
    <td rowspan="2">Pulsadores de 12mm</td>
    <td style="text-align:center;">S1, S2, S3, S4</td>
    <td>Botones para elegir los movimientos:
      <ul>
        <li>S1 selecciona un movimiento hacia 
        <span style="color:white; background-color: blue;"> ADELANTE </span> </li>
        <li>S2 selecciona un movimiento hacia la
        <span style="color:white; background-color: red;"> IZQUIERDA </span> </li>
        <li>S3 selecciona un movimiento hacia
        <span style="color:black; background-color: yellow;"> ATRÁS </span> </li>
        <li>S4 selecciona un movimiento hacia 
        <span style="color:white; background-color: green;"> DERECHA </span></li>
      </ul>
    </td>
  </tr>
  <tr>
    <td style="text-align:center;">S5</td>
    <td>Botón <span style="color:white; background-color: black;"> GO </span> para realizar la 
      secuencia de movimientos 
    </td>
  </tr>
</table>

### Soldadura de las resistencias
Las resistencias no tienen polaridad, así que no importa la orientación en que sean colocadas. Para proceder a soldarlas, colocar las resistencias teniendo presente que los valores correspondan con las etiquetas en la tarjeta. 

En la Figuras 4a hasta 4f se muestran los pasos para soldar las resistencias. Se tiene que prestar atención a la ubicación de R5 para evitar confundirla con las demás, ya que solo se utiliza una.

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera1.jpg"
        alt="botonera1" style="width:100%">
      <figcaption>Figura 4a: PCB de la botonera vista por debajo con resistencias colocadas para soldar</figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera2.jpg"
        alt="botonera2" style="width:100%">
      <figcaption>Figura 4b: Soldadura R1 y R7 de 10 KΩ</figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera5.jpg"
        alt="botonera5" style="width:100%">
      <figcaption>Figura 4c: Soldadura R5 de 22KΩ</figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: center;">
      <img src="../assets/images/Post08/Botonera/botonera6.jpg"
        alt="botonera6" style="width:100%">
      <figcaption>Figura 4d: Soldadura de R2 de 10KΩ</figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera3.jpg"
        alt="botonera3" style="width:100%">
      <figcaption>Figura 4e: Soldadura de R6, R8, R9, R10 de 1KΩ</figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera4.jpg"
        alt="botonera4" style="width:100%">
      <figcaption> Figura 4f: Soldadura de R3 y R4 de 10KΩ</figcaption>
    </figure>
  </div>
</div>

### Soldadura de los Leds
Los Leds recomendados son de 3mm ya que el espacio donde irán colocados es muy pequeño. 
Antes de soldarlos en la tarjeta se tiene que poner especial antención en la polaridad, 
ya que a diferencia de las resistencias, <span style="color:red;"> los Leds si tienes 
poladirad </span>. Para ello, se ubica la patita o terminal más corta del Led que 
corresponde al cátodo o la terminal negativa. Esta misma terminal está ubicada donde 
se puede observar corte plano en el cuerpo de cristal tal como se aprecia en la Figura 5:

<figure style="text-align: center">
  <p style="text-align: center">
    <img src="../assets/images/Post08/Botonera/led0.png"
    alt="Led0.png" width="60%">
  </p>
<figcaption>Figura 5: Esquema para indicar la polaridad de un Led</figcaption>
</figure>

Si se observa la tarjeta de frente, el cátado o terminal negativa de cada uno de los leds 
se orienta a la izquierda indicado por el dibujo de esta parte plana. Este dibujo guía 
permite que la soldadura de los Leds sea más fácil fijándose que la polaridad y el color 
coincida con la etiqueta en la tarjeta. Al finalizar de soldar todos los Leds, 
recortar los sobrantes. Los pasos en la soldadura de los Leds se puede observar en las 
Figuras 6a hasta 6d.

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/led1.jpg"
        alt="led azul" style="width:100%">
      <figcaption>
        Figura 6a: Led 1 de color azul soldado (Puede que el cuerpo de cristal sea transparente como en este caso)
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/led2.jpg"
        alt="leds rojo y verde" style="width:100%">
      <figcaption>
        Figura 6b: Led 2 de color rojo y Led 4: verde soldados
      </figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/led3.jpg"
        alt="led amarillo" style="width:100%">
      <figcaption>
        Figura 6c: Led 3 de color amarillo soldado
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/led4.jpg"
        alt="Pines de los leds recortados" style="width:100%">
      <figcaption>
        Figura 6d: Vista posterior de la tarjeta con las terminales de los leds recortadas.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura de los botones
La soldadura de los botones es simple de realizar ya que tienen las terminales dobladas 
formando un resorte para que se queden fijas en la tarjeta, es suficiente insertarlos con 
cierta fuerza hasta que se escuche el sonido de un “click” el cual indica que están 
fijos en su posición. Las Figuras 7a y 7b: 

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botones1.jpg"
        alt="botones1.jpg" style="width:100%">
      <figcaption>
        Figura 7a: Botones soldados.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botones2.jpg"
        alt="Pines de los leds recortados" style="width:100%">
      <figcaption>
        Figura 7b: A este modelo de botón se le permite poner una tapa de color que coincide 
        con el color de los Leds.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura de los pines de conexión
La soldadura de los pines tipo macho se pueden ver en las Figuras 8a y 8b cuidando 
que el lado más largo debe quedar en el reverso de la tarjeta. 

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/pinesdeConexión1.jpg"
        alt="pinesdeConexión1.jpg" style="width:100%">
      <figcaption>
        Figura 8a: Posición correcta para soldar los pines de conexión.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/pinesdeConexión2.jpg"
        alt="Pines de los leds recortados" style="width:100%">
      <figcaption>
        Figura 8b: Vista superior de los pines de conexión soldados.
      </figcaption>
    </figure>
  </div>
</div>

La función de cada uno de estos pines de conexión se puede ver en la Tabla 3: 
<table>
  <colgroup>
     <col span="1" style="width: 10%;">
     <col span="1" style="width: 10%;">
     <col span="1" style="width: 80%;">
  </colgroup>
  <caption>
    Tabla 3: Función de los pines de conexión
    entre la botonera y la tarjeta EscornaCPU que contiene el Arduino Nano.
  </caption>
  <tr>
    <th style="text-align:center">PIN</th>
    <th style="text-align:center" >ETIQUETA</th>
    <th style="text-align:center">FUNCIÓN</th>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td style="text-align:center">GND</td>
    <td>Trae la señal de tierra (0V) desde la tarjeta EscornaCPU que contiene al Arduino Nano.</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td style="text-align:center">Signal</td>
    <td>
      Lleva la señal analógica del voltaje resultante al presionar uno de los botones, es decir,
      el botón: <span style="color: white; background-color: black;">GO</span>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">3</td>
    <td style="text-align:center">5V</td>
    <td> <span style="color: red;">Opcional:</span> Está presente en la botonera en caso de que se utilice una tarjeta de
      control diferente a la EscornaCPU versión 1.2 y en la que no se utilice la 
      resistencia interna de PULL-UP del Arduino Nano. En el firmware del robot se 
      tendría que definir la palabra clave: KEY- BOARD_WIRES con valor de 3.</td>
  </tr>
  <tr>
    <td style="text-align:center">4</td>
    <td style="text-align:center">L1</td>
    <td>
        Señal de control para el Led 1 de color azul que indica un movimiento hacia 
        <span style="color:white; background-color: blue;">ADELANTE</span>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">5</td>
    <td style="text-align:center">L1</td>
    <td>
        Señal de control para el Led 2 de color rojo que indica un movimiento hacia 
        <span style="color:white; background-color:red;">IZQUIERDA</span>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">6</td>
    <td style="text-align:center">L3</td>
    <td>
        Señal de control para el Led 3 de color amarillo que indica un movimiento hacia 
        <span style="color:black; background-color: yellow;">ATRÁS</span>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">7</td>
    <td style="text-align:center">L4</td>
    <td>
        Señal de control para el Led 4 de color verde que indica un movimiento hacia 
        <span style="color:white; background-color:green;">DERECHA</span>
    </td>
  </tr>
</table>

### Prueba de los Leds
<p>
  El objetivo es verificar que todos los Leds se encienden al energizarlos. Para ello se puede
  utilizar el Arduino Nano alimentado con las baterías dentro del portapilas y el cable de tierra
  o de color negro conectado al pin GND del Arduino Nano y el positivo o cable rojo al 
  pin Vin también del Arduino Nano.
  Posteriormente, mediante un cable tipo "jumper" conectado al otro pin GND del Arduino Nano 
  y al pin GND de la botonera y otro cable al pin +5V del Arduino Nano y a cualquier pin 
  de la botonera con la etiqueta: L1, L2, L3 o L4, dependiendo de que Led se 
  quiera probar.
</p>

<p>
  En caso de que no encendieran se debe verificar la orientación (polaridad) 
  de los Leds en la botonera y el valor de las resistencias R6, R8, R9 y R10 que 
  debe ser de 1 KΩ.
</p>

<p>
  Como ejemplo, se puede observar en la Figura 9a que al conectar un cable al pin GND y el otro 
  cable a al pin L1 se enciende el Led de color azul. De igual forma, en la Figura 9b 
  se puede ver que al conectar un cable al pin GND y el otro cable al pin L3 se enciende 
  el Led de color amarillo.
</p> 

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera_ensambleLed_1.png"
        alt="botonera_ensambleLed_1.jpg" style="width:100%">
      <figcaption>
        Figura 9a: Prueba del Led 1 de color azul conectado al pin L1.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Botonera/botonera_ensambleLed_3.png"
        alt="botonera_ensambleLed_3.jpg" style="width:100%">
      <figcaption>
        Figura 9b: Prueba del Led 3 de color amarillo conectado al pin L3.
      </figcaption>
    </figure>
  </div>
</div>


Por otro lado, para probar el funcionamiento de los botones se necesita de un 
multímetro con el objetivo de medir el valor de la resistencia al accionar cualquiera 
de los botones. El siguiente procedimiento es una guía para realizar la prueba:
<p>
  <ol>
    <li>Colocar el multímetro en la escala de 200 KΩ</li>
    <li>Conectar la punta de prueba negra en el pin de la botonera con la etiqueta: <b>GND</b></li>
    <li>Conectar la punta de prueba roja en el pin de la botonera con la etiqueta: <b>Sig</b></li>
    <li>Pulsar cada uno de los botones y comparar el valor de la resistencia indicada por el 
      multímetro con los siguientes valores:</li>
      <ul>
        <li>
            Al pulsar el botón del movimiento hacia 
            <span style="background-color: blue; color: white;">ADELANTE</span> 
            ubicado encima del Led 1 de color azul, el multímetro indicará 
            <span style="background-color: blue; color: white;">10KΩ</span> aproximadamente.
        </li>
        <li>
          Al pulsar el botón del giro hacia la 
          <span style="background-color: red; color: white;">IZQUIERDA</span> 
          ubicado a la izquierda del Led 2 de color rojo, el multímetro indicará 
          <span style="background-color: red; color: white;">20KΩ </span> aproximadamente.
        </li>
        <li>
          Al pulsar el botón del movimiento hacia 
          <span style="background-color:yellow; color: black;">ATRÁS</span> 
          ubicado encima del Led 3 de color amarillo, el multímetro indicará 
          <span style="background-color:yellow; color: black;">30KΩ</span> aproximadamente.
        </li>
        <li>
          Al pulsar el botón del giro hacia la 
          <span style="background-color: green; color: white;">DERECHA</span> 
          ubicado a la derehca del Led 4 de color verde, el multímetro indicará 
          <span style="background-color: green; color: white;">62KΩ </span> aproximadamente.
        </li>
        <li>
          Al pulsar el botón del centro
          <span style="background-color:black; color: white;">GO</span> 
          el multímetro indicará 
          <span style="background-color:black; color: white;">40KΩ </span> aproximadamente.
        </li>
      </ul>
  </ol>
</p>

El funcionamiento de la botonera se basa en un divisor de voltaje, el cual, al pulsar cada uno de 
los botones varía el voltaje en un rango de 0-5V. Esta señal de voltaje es la que va conectada 
al pin con la etiqueta <b>Sig</b> de la botonera. El esquema eléctrico de la botonera, así como un esquema 
simplificado y las ecuaciones del funcionamiento del circuito divisor de voltaje se muestran en la Figura 10:

<figure style="text-align: center">
  <p style="text-align: center"> 
    <img src="../assets/images/Post08/Botonera/divisorVoltaje.png"
    alt="divisorVoltaje.png" width="90%"/>
  </p>
  <figcaption>
    Figura 10: Esquemas y ecuaciones del circuito divisor de voltaje que conforma la botonera
  </figcaption>
</figure>

## Ensamble de la tarjeta de control
La tarjeta de control utilizada es la
<a href="https://github.com/escornabot/electronics/tree/master/EscornaCPU/1.x/1.20">EscornaCPU V1.2</a>
que se puede ver en la Figura 11:

<figure style="text-align: center; width:80%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/CPU/cpu_inicial.jpg"
    alt="cpu_inicial.jpg" width="100%"/>
  <figcaption>
    Figura 11: Tarjeta de control EscornaCPU_V1.2
  </figcaption>
</figure>

### Componentes
Los componentes requeridos y su correspondiente función se enlistan en la Tabla 4:
<table>
    <colgroup>
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 30%;">
       <col span="1" style="width: 20%;">
       <col span="1" style="width: 40%;">
    </colgroup>
  <caption>Tabla 4: Descripción y funcionamiento de los componentes requeridos para la botonera</caption>
  <tr>
    <th style="text-align:center">CANT</th>
    <th style="text-align:center">DESCRIPCIÓN</th>
    <th style="text-align:center">ETIQUETA</th>
    <th style="text-align:center">FUNCIÓN</th>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Arduino Nano</td>
    <td style="text-align:center">Arduino Nano</td>
    <td>Tarjeta de desarrollo y programación basada en el micro- controlador ATmega328</td>
  </tr>
  <tr>
    <td style="text-align:center;">1</td>
    <td>Driver ULN2803</td>
    <td style="text-align:center">IC1 ULN2803</td>
    <td>Circuito integrado que es el enlace o interfaz entre las señales de control provenientes 
      del Arduino Nano y los motores paso a paso 28BYJ-48.
      <p>
        Nota: <span style="color: red;">este circuito no se solda,</span>
        se coloca sobre un zócalo o base que sií irá soldada a la tarjeta.
      </p>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Zócalo de 18 pines</td>
    <td style="text-align:center">IC1 ULN2803</td>
    <td>Base que soldada a la tarjeta sobre la que se coloca el driver ULN2803.</td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Conector macho JST-XHP-5 para motor paso a paso 28BYJ-48</td>
    <td style="text-align:center">Motor Left / Motor Right</td>
    <td>Conectar los motores a la tarjeta.</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Conector macho JST-XHP-2 para alimentación de la tarjeta de control</td>
    <td style="text-align:center">GND +</td>
    <td>Recibir la alimentación del portapilas con las 4 baterías AA</td>
  </tr>
  <tr>
    <td style="text-align:center;" rowspan="3">4</td>
    <td rowspan="3">Resistencia 10 KΩ</td>
    <td style="text-align:center">R1</td>
    <td> 
      Está presente en la tarjeta de control en caso de que se utilice una botonera 
      diferente a la E_KeyPad V2.2 que no cuente con esta resistencia y que no se 
      utilice la resistencia interna de PULL_UP del Arduino Nano. 
      <p>
        <span style="color:red;"> Por precaución </span>
        esta resistencia se puede soldar y en el firmware del robot se tendría 
        que definir la palabra clave: KEYBOARD_WIRES con el valor de 3.
      </p>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">R2</td>
    <td>
      <span style="color:red;">Opcional: </span> 
      Es la primera resistancia del divisor de voltaje que reduce el voltaje de 5.0V 
      a 3.3V que va desde el pin 1 del Arduino hacia el pin de transmisión 
      del adaptador Bluetooth, en caso de que se desea conectar y utilizar.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">R4,R5</td>
    <td><span style="color:red;">Son Opcionales: </span> 
      Están presentes en la tarjeta de control en caso de que se desea 
      conectar el Escornabot a una red WiFi a través de un módulo un ESP-01.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia de 18KΩ o de 20KΩ</td>
    <td style="text-align:center">R3</td>
    <td><span style="color:red;">Opcional: </span> 
      La segunda resistencia del divisor de voltaje que reduce el voltaje de 
      5.0V a 3.3V que va desde el pin 1 del Arduino hacia el pin de transmisión 
      del adaptador Bluetooth, en caso de que se desee conectar y utilizar.
    </td>
  </tr>
  <tr>
    <td style="text-align:center;" rowspan="2">8</td>
    <td rowspan="2">Pines o Headers macho</td>
    <td style="text-align:center">A0, A1, A2, A3, A7, GND</td>
    <td> 6 Pines de conexión con la tarjeta de la botonera. </td>
  </tr>
  <tr>
    <td style="text-align:center">BUZZ ON</td>
    <td>
      2 Pines que permitirán activar / desactivar de forma manual el 
      buzzer del Escornabot a través de la colocación de un puente o 
      Jumper para pines rectos.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Tira de 15 pines hembra</td>
    <td style="text-align:center">D12 / D13</td>
    <td>Base soldada sobre la cual se colocará el Arduino Nano</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Tira de 4 pines hembra</td>
    <td style="text-align:center">BlueT</td>
    <td><span style="color:red;">Opcional: </span>
      En caso de que se desee colocar un adaptador Bluetooth para 
      conectar el Escornabot con una aplicación para teléfono móvil.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Interruptor de alimentación SK12F14 o SK12D07</td>
    <td style="text-align:center">S7</td>
    <td>Interruptor general que enciende o apaga el Escornabot.</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Buzzer activo de 5V para Arduino</td>
    <td style="text-align:center">Z1</td>
    <td>Emitir un sonido frente a cada movimiento o instrucción 
      ejecutada por el Escornabot.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Fusible Rearmable XF050</td>
    <td style="text-align:center">F1</td>
    <td>Interruptor de seguridad que se abre frente al paso de una 
      cantidad de corriente que podría quemar el Arduino Nano u 
      otro componente de la tarjeta de control.
      <p></p><b>Nota:</b>En caso de no contar con este componente, se puede soldar un trozo 
        de alambre de cobre</p>
    </td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Diodo Schottky 1N5817</td>
    <td style="text-align:center">D1</td>
    <td>
      Diodo utilizado para la protección del circuito frente a 
      polaridad invertida.
    </td>
  </tr>
  <tr>
    <td style="text-align:center">2</td>
    <td>Condensador cerámico 104 de 100nF</td>
    <td style="text-align:center">C1,C3</td>
    <td>
      Eliminación de ruido eléctrico en la señal de alimentación de la 
      tarjeta y en el driver ULN2803.
    </td>
  </tr>
</table>

### Soldadura de las resistencias
<p>
  Soldar las resistencias de forma similar a la Botonera. Teniendo cuidado de no 
  confundir los valores. Las Figuras 12a y 12b muestran el soldado de cada una de 
  las resistencias:
</p>

<div class="row">
  <div class="column">
    <figure style="text-align: center;">
      <img src="../assets/images/Post08/CPU/cpu_resistencias_1.jpg"
        alt="cpu_resistencias_1.jpg" style="width:100%">
      <figcaption>
        Figura 12a: Soldadura de R1 y R2 de 10KΩ.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: center;">
      <img src="../assets/images/Post08/CPU/cpu_resistencias_2.jpg"
        alt="cpu_resistencias_2.jpg" style="width:100%">
      <figcaption>
        Figura 12b: Soldadura de R3 de 18KΩ.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura del diodo
<p>Antes de soldar el diodo se debe observar su polaridad, 
ya que este elemento permite el paso de la corriente en una 
única dirección, por lo que es importante que quede soldado de 
la manera correcta.</p> 

<p>El esquema con el que se representa a este componente consiste en 
un triángulo con una barra horizontal en uno de sus vértices. 
La dirección del triángulo indica la dirección de la corriente, 
mientras que la barra horizontal indica el no paso de la corriente.</p>

<p>Este esquema se encuentra dibujado en la tarjeta para orientar al 
diodo de forma correcta. Por otro lado, en el componente únicamente
se señala la barra horizontal de color gris, suficiente para orientarlo en la 
tarjeta tal como se muestra en la Figura 13.</p>

<figure style="text-align: center; width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/CPU/cpu_diodo_1.png"
    alt="cpu_diodo_1.png" width="100%"/>
  <figcaption>
    Figura 13: Soldadura del diodo Schotttky 1N5817
  </figcaption>
</figure>

### Soldadura de los condensadores
<p>
  Los condensadores cerámicos son elementos que <b>no</b> tienen polaridad, 
  a diferencia de los electrolíticos que sí la tienen.
  En este caso los condensadores 104 se pueden soldar sin importar la orientación 
  en que se coloquen. Las Figuras 14a y 14b muestran la soldadura de estos dos 
  condensadores y el aspecto de la tarjeta de control.
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_condensadores_1.png"
        alt="cpu_condensadores_1.png" style="width:100%">
      <figcaption>
        Figura 14a: Soldadura de los condensadores cerámicos 104
        de 100nF.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_condensadores_2.png"
        alt="cpu_condensadores_2.png" style="width:100%">
      <figcaption>
        Figura 14b: Vista inclinada de la tarjeta de control con los
        condensadores y el diodo soldados.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura del zócalo de 18 pines
<p>
  El zócalo de 18 pines se utiliza para colocar el driver ULN2803, 
  ya que no es recomendable soldar este último de forma directa a la placa debido 
  a que se puede quemar por el calor aplicado y además, en caso de que se necesite 
  reemplazar a futuro, pueda realizarse sin tener que desoldarlo de la tarjeta.
</p> 
<p>
  Antes de soldarlo hay que ubicarlo de forma correcta. Hay que tener presente 
  que tiene una <b>orientación específica</b> 
  con la muesca que tiene en uno de sus extremos orientada hacia la izquierda, 
  tal como se muestra en la Figura 15a teniendo cuidado de acomodarlo de forma 
  correcta para que asiente de forma plana sobre la tarjeta y la resistencia 
  R3 de 18 KΩ no le estorbe y quede debajo y oculta.
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_zocalo_1.png"
        alt="cpu_zocalo_1.png" style="width:100%">
      <figcaption>
        Figura 15a: Ubicación del zócalo de 18 pines con la muesca hacia 
        la izquierda.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_zocalo_3.png"
        alt="cpu_zocalo_3" style="width:100%">
      <figcaption>
        Figura 15b: Posición correcta del zócalo colocado de forma plana
        sobre la tarjeta.
      </figcaption>
    </figure>
  </div>
</div>

<p>
  Para soldarlo, se puede comenzar con las dos primeras terminales como
  se puede ver en la Figura 16 y antes de soldar las terminales restantes, 
  verificar que no quede levantado.
</p>

<figure style="width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/CPU/cpu_zocalo_2.png"
    alt="cpu_zocalo_2.png" width="100%"/>
  <figcaption>
    Figura 16: Soldadura de las dos primeras terminales del zócalo.
  </figcaption>
</figure>

### Soldadura del buzzer
<p>
  En primer lugar, proceder a soldar los 2 pines tal como se 
  muestra en la Figura 17a los cuales permitirán activar / desactivar el 
  buzzer de forma manual a través de la colocación de un puente o jumper.
</p>
<p>
  En segundo lugar, colocar el buzzer fijándose en la etiqueta: ⊕ que tiene 
  estampada en la parte superior tal como se muestra en la Figura 17b para 
  que coincida con la misma marca impresa en la tarjeta. Una vez realizado 
  esto soldar los dos pines del buzzer. 
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_postes_buzzer_1.png"
        alt="cpu_postes_buzzer_1.png" style="width:100%">
      <figcaption>
        Figura 17a: Soldadura de los pines de activación del buzzer.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_buzzer_1.png"
        alt="cpu_buzzer_1.png" style="width:100%">
      <figcaption>
        Figura 17b: Orientación correcta del buzzer para soldarlo.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura de los pines de conexión a la botonera
<p>
  Los 6 pines de conexión permiten conectar los cables que vienen de 
  la botonera. Antes de soldarlos hay que colocarlos como se muestra en la 
  Figura 18, fijándose que <span style="color: red;">queden hacia abajo</span>  
  y soldarlos por la parte de arriba de la tarjeta.
</p>

<figure style="width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/CPU/cpu_pines_botonera_1.png"
    alt="cpu_pines_botonera_1.png" width="100%"/>
  <figcaption>
    Figura 18: Soldadura de los pines de conexión a la botonera.
  </figcaption>
</figure>

### Soldadura de los pines para el Arduino Nano
<p>
  Al igual que el driver ULN2803, el Arduino Nano tiene que ir sobre las dos 
  tiras de 15 pines tipo hembra que le servirán de base, con el objetivo de que 
  sea fácil reemplazarlo en caso de cualquier avería. 
</p>

<p>
  Para asegurar que las tiras de pines queden alineadas de forma correcta, 
  <span style="color: red;">primero </span> se coloca el Arduino Nano sobre 
  ellas tal como se muestra en la Figura 19a.   
</p>

<p>
  En seguida, se procede a soldar uno o dos pines de cada lado para fijarlas 
  en posición, y posteriormente se puede retirar el Arduino Nano 
  para continuar con la soldadura de los pines restantes como se observa en la 
  Figura 19b.
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_arduino_1.png"
        alt="cpu_arduino_1.png" style="width:100%">
      <figcaption>
        Figura 19a: Arduino Nano colocado sobre los pines base y a su vez
        sobre la tarjeta de control.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_arduino_2.png"
        alt="cpu_arduino_2.png" style="width:100%">
      <figcaption>
        Figura 19b: Soldadura de las 2 tiras de los pines base del Arduino 
        Nano.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura de los conectores para alimentación y de los motores paso a paso

<p>
  Los conectores para alimentación y para la conexión de los motores
  paso a paso 28BYJ-48 son del tipo JST-XHP con 2 y 5 pines respectivamente.
  Posicionarlos como se muestra en la Figura 20a con las 
  <span style="color: red;"> ranuras hacia adentro </span> de la tarjeta.
</p>

<figure style="width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/CPU/JST-XHP_Posicion.jpg"
    alt="JST-XHP_Posicion.jpg" width="100%"/>
  <figcaption>
    Figura 20a: Posición de los conectores JST-XHP-2 y JST-XHP-5 para 
    alimentación y conexión de los motores paso a paso 28BYJ-48, 
    respectivamente. Las ranuras señaladas con los círculos deben 
    quedar hacia <span style="color: red;">adentro</span>.
  </figcaption>
</figure>

<p>
  La soldadura de estos componentes se efectúa por la parte de encima
  de la tarjeta. La posición lista para soldar se puede observar en 
  las Figuras 20b y 20c:
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/JST-XHP_Bateria.jpg"
        alt="JST-XHP_Bateria.jpg" style="width:100%">
      <figcaption>
        Figura 20b: Posición del conector JST-XHP-2 para conectar
        la alimentación proveniente del portapilas.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/JST-XHP_Motores.jpg"
        alt="JST-XHP_Motores.jpg" style="width:100%">
      <figcaption>
        Figura 20c: Posición del conector JST-XHP-5 para conectar
        los motores paso paso 28BYJ-48.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura del interruptor de encendido

<p>
  La tarjeta de control tiene dos juegos de 5 agueros
  para el interruptor de alimentación tal como se puede ver en la Figura 21a. 
  La diferencia es el tamaño, siendo uno más grande que el otro. 
  Ambos están interconectados, así que no importa cual juego se elija, 
  esto depende del tamaño del interruptor disponible.
</p>

<p>
  Colocar el interruptor Sk12D07 o similar en la tarjeta de control 
  insertandolo en cualquiera de los dos juegos de agujeros, tal como
  se aprecia en la ubicación mostrada en la Figura 21b. Es importante
  mencionar que no importa la orientación, ya que conmuta entre 
  dos posiciones (ON/OFF).
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_inicial_interruptor.jpg"
        alt="cpu_inicial_interruptor.jpg" style="width:100%">
      <figcaption>
        Figura 21a: Se señalan los dos juegos de agujeros disponibles
        para soldar el interruptor de alimentación. Se puede utilizar
        cualquiera de los dos, el escoger entre uno u otro
        depende del tamaño del interrruptor disponible. 
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_SwitchSK12F14_b.jpg"
        alt="cpu_SwitchSK12F14_b.jpg" style="width:100%">
      <figcaption>
        Figura 21b: Interruptor de alimentación listo para soldar en la 
        tarjeta de control.
      </figcaption>
    </figure>
  </div>
</div>

### Soldadura del fusible rearmable

<p>
  El fusible rearmable XF050 protege la tarjeta de control de una
  sobrecarga de corriente. Para soldarlo simplemente hay que colocarlo
  en los agujeros identificados con la etiqueta F1 y soldarlo sin 
  importar la orientación. 
</p>

<p>
  Sin embargo, si no es posible conseguirlo, 
  como ha sido mi caso, se puede soldar un trozo de alambre en lo que
  está disponible. Para ello ubicar el trozo de alambre como se muestra
  en la Figura 22a y soldarlo con poca soldadura, para poder retirlo
  después, como se aprecia en la Figura 22b.
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_Fusible_a.jpg"
        alt="cpu_Fusible_a.jpg" style="width:100%">
      <figcaption>
        Figura 22a: Trozo de alambre en lugar del Fusible XF050, mientras
        es factible de conseguirse. 
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_Fusible_b.jpg"
        alt="cpu_Fusible_b.jpg" style="width:100%">
      <figcaption>
        Figura 22b: Trozo de alambre soldado ligeramente por la parte 
        superior de la tarjeta de control para posteriormente ser retirado
        y reemplazarlo por el fusible rearmable XF050.
      </figcaption>
    </figure>
  </div>
</div>

### Colocación del driver UNL2803 

<p>
  El driver UNL2803 junto a la tarjeta de control mostrados en la Figura 23a,
  se debe insertar en el zócalo teniendo cuidado de orientarlo de forma 
  correcta y de no doblar las patitas al hacerlo. 
</p>

<p>
  La orientación se puede 
  ver en la Figura 23b con la muesca apuntando hacia abajo, en la misma 
  dirección en la que está el Arduino Nano. 
</p>

<p>
  Al colocarlo acomodar las patitas
  para que encajen en el zócalo acomodándolas ligeramente sí es necesario, 
  teniendo cuidado de no doblarlas, tal como se aprecia en la Figura 23c. 
</p>

<p>  
  En seguida, empujarlo hasta que asiente en el fondo del zócalo y revisar que 
  ninguna patita se haya doblado, hasta que quede como se muestra en la Figura
  23d. 
</p>

<div class="row">
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/CPU/cpu_UNL2803_a.jpg"
        alt="cpu_UNL2803_a.jpg" style="width:100%">
      <figcaption>
        Figura 23a: Tarjeta de control y el driver UNL2803 de control 
        de los motores paso a paso 28BYJ-48. 
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/CPU/cpu_UNL2803_b.jpg"
        alt="cpu_UNL2803_b.jpg" style="width:100%">
      <figcaption>
        Figura 23b: Orientación del driver posicionado con 
        la muesca hacia abajo. 
      </figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/CPU/cpu_UNL2803_c.jpg"
        alt="cpu_UNL2803_c.jpg" style="width:100%">
      <figcaption>
        Figura 23c: Driver colocado en posición 
        <span style="color: red;">antes</span> de ser insertado
        en su zócalo, cuidando de que las patitas queden alineadas
        y sin doblarse. 
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_UNL2803_d.jpg"
        alt="cpu_UNL2803_d.jpg" style="width:100%">
      <figcaption>
        Figura 23d: Driver ya insertado cuidando de que no se doblen
        las patitas de conexión.
      </figcaption>
    </figure>
  </div>
</div>

### Tarjeta de control terminada

## Ensamble del Escornabot
Las partes impresas y los tornillos necesarios para ensamblar el Escornabot
se muestran en la Figura 21:

<figure style="text-align: center; width:90%; 
              margin-left: auto; 
              margin-right: auto;">
  <img src="../assets/images/Post08/Ensamble/lista_de_partes.jpg"
    alt="lista_de_partes" width="100%"/>
  <figcaption>
    Figura 21: Partes impresas y tornillos para ensamblar el robot.
  </figcaption>
</figure>

<p>
  La secuencia de ensamble es la siguiente:
</p>

### Paso 1: Inicio del ensamble
Partes requeridas:
<ul>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Motor-Bracket</li>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Battery-Bracket</li>
  <li>1 Tornillo M3 x 12mm</li>
</ul>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso1a.jpg"
        alt="paso1a" style="width:100%">
      <figcaption>
        Paso 1a: Colocar el Motor-Bracket y el Battery-Bracket 
        como se muestra en la imagen y sujertarlos con un tornillo M3 x 12mm.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso1b.jpg"
        alt="paso1b" style="width:100%">
      <figcaption>
        Paso 1b:  Terminar de atornillar y verificar la sujeción.
      </figcaption>
    </figure>
  </div>
</div>

### Paso 2: Ensamble del cuerpo
Partes requeridas:
<ul>
  <li>Ensamble del paso 1</li>
  <li><span style="color: green;">Pieza impresa en 3D:</span> E-KeypadV2.2-Bracket</li>
  <li><span style="color: green;">Pieza impresa en 3D:</span> EscornaCPU_V1.2-Bracket</li>
  <li>2 Tornillo M3 x 12mm</li>
</ul>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso2a.jpg"
        alt="paso2a" style="width:100%">
      <figcaption>
        Paso 2a: Colocar el E-KeypadV2.2-Bracket encima del ensamble anterior 
        sujertarlos con un tornillo M3 x 12mm en la parte de posterior.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso2b.jpg"
        alt="paso2b" style="width:100%">
      <figcaption>
        Paso 2b:  Colocar el EscornaCPU_V1.2-Bracket encima del ensamble anterior 
        sujertarlos con un tornillo M3 x 12mm en la parte frontal.
      </figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso2c.jpg"
        alt="paso2c" style="width:100%">
      <figcaption>
        Paso 2c: Terminar de atornillar el tornillo M3 x 12mm de la parte 
        frontal y verificar la sujeción.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso2d.jpg"
        alt="paso2d" style="width:100%">
      <figcaption>
        Paso 2d:  Terminar de atornillar el tornillo M3 x 12mm de la parte 
        posterior y verificar la sujeción.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="text-align: justify; width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/Ensamble/paso2e.jpg"
    alt="paso2e.jpg" width="100%"/>
  <figcaption>
    Paso 2e: <span style="color: red;">Girar</span> el ensamble para colocar y
    atornillar un tornillo M3 x 6mm en la parte inferior.
  </figcaption>
</figure>

### Paso 3: Ensamble de los motores
Partes requeridas:
<ul>
  <li>Ensamble del paso 2</li>
  <li>Motores paso a paso 28BYJ-48</li>
  <li>4 Tornillos M3 x 6mm</li>
</ul>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso3a.jpg"
        alt="paso3a" style="width:100%">
      <figcaption>
        Paso 3a: Colocar uno de los motores en el lado derecho del 
        robot con la orientación mostrada y con los cables apuntando 
        hacia adelante. Sujetárlo con 2 tornillos M3 x 6mm.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso3b.jpg"
        alt="paso3b" style="width:100%">
      <figcaption>
        Paso 3b:  Igual que el lado derecho, colocar y sujetar el motor
        del lado izquierdo con 2 tornillo M3 x 6mm.
      </figcaption>
    </figure>
  </div>
</div>

### Paso 4: Preparación de las ruedas
Componentes necesarios:
<ul>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Wheel-Right</li>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Wheel-Left</li>
  <li>2 Tornillos M3 x 6mm</li>
  <li>2 Tuercas M3</li>
</ul>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso4a.jpg"
        alt="paso4a" style="width:100%">
      <figcaption>
        Paso 4a: Colocar una de las tuercas M3 en la ranura.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso4b.jpg"
        alt="paso4b" style="width:100%">
      <figcaption>
        Paso 4b:  Con ayuda de unas pinzas insertar la tuerca hasta el fondo
        de la ranura.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="text-align: left; width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/Ensamble/paso4c.jpg"
    alt="paso4c.jpg" width="100%"/>
  <figcaption>
    Paso 4c: Colocar uno de los tornillos M3 x 6mm y verificar que pueda 
    atornillarse a la tuerca insertada en el paso anterior.
  </figcaption>
</figure>

### Paso 5: Ensamble de las ruedas al cuerpo
Componentes necesarios:
<ul>
  <li>Ensamble del paso 3</li>
  <li>Wheel-Right con la tuerca M3 y el tornillo M3 x 6mm colocados</li>
  <li>Wheel-Left con la tuerca M3 y el tornillo M3 x 6mm colocados</li>
</ul>

<div class="row">
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso5a.jpg"
        alt="paso5a" style="width:100%">
      <figcaption>
        Paso 5a: Tomar la rueda de la derecha (Wheel-Right) e insertarla
        en el eje del motor del lado derecho del cuerpo del robot. 
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso5b.jpg"
        alt="paso5b" style="width:100%">
      <figcaption>
        Paso 5b:  Verificar que esté insertada completamente hasta el 
        fondo y asegurar el tornillo para sujetar la rueda al eje del motor.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="text-align: justify; width:50%; 
              margin-left: auto; 
              margin-right: auto;">
  <img src="../assets/images/Post08/Ensamble/paso5c.jpg"
    alt="paso5e.jpg" width="100%"/>
  <figcaption>
    Paso 5c: Repetir el procedimiento para el lado izquierdo del robot.
  </figcaption>
</figure>

### Paso 6: Soporte de la rueda de pivote
Partes requeridas:
<ul>
  <li>Ensamble del paso 5</li>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Ball-Caster</li>
  <li>5 Tornillos M3 x 6mm </li>
</ul>

<div class="row">
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso6a.jpg"
        alt="paso6a" style="width:100%">
      <figcaption>
        Paso 6a: Sujetar el soporte de la rueda loca con los 2 tornillos 
        M3 x 6mm al Battery-Bracket
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso6b.jpg"
        alt="paso6b" style="width:100%">
      <figcaption>
        Paso 6b:  Colocar, sin atornillar completamente, 3 tornillos M3 x 6mm 
        sobre la pieza impresa: E-KeypadV2.2-Bracket.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="text-align: center; width:90%; 
              margin-left: auto; 
              margin-right: auto;">
    <video width="80%" controls poster="../assets/images/Post08/Ensamble/paso6c.png">
      <source src="../assets/images/Post08/Ensamble/paso6c.mp4" type="video/mp4">
    </video>
  <figcaption>
    Paso 6c: Vista en 360º del ensamble del cuerpo del robot.
  </figcaption>
</figure>

### Paso 7: Conexión de la botonera y la tarjeta de control

<div class="row">
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso7a.jpg"
        alt="paso7a" style="width:100%">
      <figcaption>
        Paso 7a: Ensamble realizado anteriormente, botonera y tarjeta 
        de control.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso7b.jpg"
        alt="paso7b" style="width:100%">
      <figcaption>
        Paso 7b: Conectar con 6 cables tipo Jumper los pines de 
        conexión de la botonera. <span style="color: red;">Atención: </span>
        Para este modelo <span style="color: red;"><b>no</b></span> conectar el pin 
        con la etiqueta 5V.
      </figcaption>
    </figure>
  </div>
</div>

<p>
  Antes de seguir, se debe tener presente la conexión de los 
  cables con la tarjeta de control, para ello, la Tabla 5: detalla esa 
  correspondencia:
</p>

<table style="width: 40%; margin-left: auto; margin-right: auto;">
  <colgroup>
     <col span="1" style="width: 10%;">
     <col span="1" style="width: 10%;">
  </colgroup>
  <caption>
    Tabla 5: Correspondencia de los cables entre la botonera y la 
    tarjeta de control
  </caption>
  <tr>
    <th style="text-align:center">PIN BOTONERA</th>
    <th style="text-align:center">PIN TARJETA DE CONTROL</th>
  </tr>
  <tr>
    <td style="text-align:center">L4</td>
    <td style="text-align:center">A0</td>
  </tr>
  <tr>
    <td style="text-align:center">L3</td>
    <td style="text-align:center">A1</td>
  </tr>
  <tr>
    <td style="text-align:center">L2</td>
    <td style="text-align:center">A2</td>
  </tr>
  <tr>
    <td style="text-align:center">L1</td>
    <td style="text-align:center">A3</td>
  </tr>
  <tr>
    <td style="text-align:center">5V</td>
    <td style="text-align:center">No conectado</td>
  </tr>
  <tr>
    <td style="text-align:center">Sig</td>
    <td style="text-align:center">A7</td>
  </tr>
  <tr>
    <td style="text-align:center">GND</td>
    <td style="text-align:center">GND</td>
  </tr>
</table>

<p>
  Proceder a conectar los cables en la tarjeta de control como se muestra en la
  imagen del Paso 7c fijándose muy bien en el orden de los cables, tal como se especificó 
  en la Tabla 5. El resultado debe ser como se muestra en la imagen del Paso 7d.
</p>

<div class="row">
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso7c.jpg"
        alt="paso7c" style="width:100%">
      <figcaption>
        Paso 7c: Conectar el otro extremo de los 6 cables tipo Jumper a la 
        tarjeta de control. Fijándose en el orden de las conexiones. Como guía,
        el cable con la etiqueta L4 procedente de la botonera debe
        corresponder con la etiqueta A0 en la tarjeta de control.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: left;">
      <img src="../assets/images/Post08/Ensamble/paso7d.jpg"
        alt="paso7d" style="width:100%">
      <figcaption>
        Paso 7d: Cables conectados desde la botonera hasta la tarjeta de control.
      </figcaption>
    </figure>
  </div>
</div>

### Paso 8: Conexión de los motores y colocación de la tarjeta de control en el cuerpo del robot


<div class="row">
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso8a.jpg"
        alt="paso8a" style="width:100%">
      <figcaption>
        Paso 8a: Organizar los cables provenientes de ambos motores en forma 
        de círculo, enrollando con mucho cuidado, los cables sobre sí mismos.
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso8b.jpg"
        alt="paso8b" style="width:100%">
      <figcaption>
        Paso 8b: Sujetar los cables de ambos motores con una cremallera plástica
        verificando que los conectores queden ubicados hacia la izquierda, tal 
        como se muestra en la imagen.
      </figcaption>
    </figure>
  </div>
</div>

<div class="row">
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso8c.jpg"
        alt="paso8c" style="width:100%">
      <figcaption>
        Paso 8c: Conectar los motores a la tarjeta de control fijándose muy bien
        que el motor del lado derecho coincida con la etiqueta: <i>Motor Right</i> 
        y el motor del lado izquierdo con la etiqueta: <i>Motor Left</i>. No hay 
        que preocuparse con la orientación, ya que este tipo de conectores 
        únicamente permiten ser insertados en una sola posición.        
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso8d.jpg"
        alt="paso8d" style="width:100%">
      <figcaption>
        Paso 8d: Insertar el extremo tarjeta de control donde se ubica el puerto mini USB 
        del Arduino Nano en la ranura frontral de la pieza:  
        <span style="color: green;">EscornaCPU_V1.2-Bracket</span>.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="text-align: justify; width:50%; 
              margin-left: auto; 
              margin-right: auto;"> 
    <img src="../assets/images/Post08/Ensamble/paso8e.jpg"
    alt="paso8e.jpg" width="100%"/>
  <figcaption>
    Paso 8e: Insertar el otro extremo de la tarjeta de control en la ranura 
    de la pieza: <span style="color: green;">EscornaCPU_V1.2-Bracket</span>
    haciendo un poco de presión para acomodarla hasta escuchar un sonido 
    parecido a un "click" o hasta que quede bien asentada.
  </figcaption>
</figure>

### Paso 9: Colocación de la canica pivote
Partes requeridas:
<ul>
  <li><span style="color: green;">Pieza impresa en 3D:</span> Ball-Caster</li>
  <li>1 Canica de 17mm </li>
</ul>

<p>
  Para colocar la canica de 17mm de diámetro en la pieza impresa en 3D: 
  <span style="color: green;font-style: italic;">Ball-Caster</span>, es necesario
  des-ensamblarlo del cuerpo y ponerlo sobre una superficie plana.
</p>

<div class="row">
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso9a.jpg"
        alt="paso9a" style="width:100%">
      <figcaption>
        Paso 9a: Colocar la canica encima de la pieza: 
        <span style="color: green;">Ball-Caster</span>, que a su vez
        deberá estar sobre una superficie plana para poder insertar la canica
        de forma adecuada.       
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure style="text-align: justify;">
      <img src="../assets/images/Post08/Ensamble/paso9b.jpg"
        alt="paso9b" style="width:100%">
      <figcaption>
        Paso 9b: Insertar la canica en la cavidad haciendo un poco de 
        presión para acomodarla adentro hasta escuchar un sonido parecido 
        a un "click" o hasta que quede bien asentada.
      </figcaption>
    </figure>
  </div>
</div>

### Paso 10: Conexión de las baterías y la botonera


Partes requeridas:
<ul>
  <li>Portapilas cuadrado para 4 pilas AA</li>
  <li>4 pilas o baterías AA</li>
</ul>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso10a.jpg"
        alt="paso10a" style="width:100%">
      <figcaption>
        Paso 10a: Colocar las baterías en el portapilas cuidando que la orientación
        sea la correcta.        
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/Ensamble/paso10b.jpg"
        alt="paso10b" style="width:100%">
      <figcaption>
        Paso 10b: Atornillar la botonera sobre la pieza:  
        <span style="color: green;">E-KeypadV2.2-Bracket</span>
        fijándose que el botón azul quede hacia el frente del robot y que los
        cables del portapilas queden debajo de la botonera entre los soportes
        donde va a atornillada la botonera.
      </figcaption>
    </figure>
  </div>
</div>

<figure style="width:50%; 
              margin-left: auto; 
              margin-right: auto;">
    <img src="../assets/images/Post08/Ensamble/paso10c.jpg"
    alt="paso10c" width="100%"/>
  <figcaption>
    Paso 10c: Conectar el conector JST-XHP-2 del portapilas a su receptáculo
    en la tarjeta de control haciéndolo con cuidado para no desoldarlo.
  </figcaption>
</figure>

### Paso 11: Vista en 360º del ensamble terminado

<figure style="text-align: center; width:90%; 
              margin-left: auto; 
              margin-right: auto;">
    <video width="80%" controls poster="../assets/images/Post08/Ensamble/paso11_portada.png">
      <source src="../assets/images/Post08/Ensamble/paso11_video.mp4" type="video/mp4">
    </video>
  <figcaption>
    Paso 11: Video de 360&deg; para mostrar el ensamble del cuerpo del robot.
  </figcaption>
</figure>

## Puesta en marcha
### Carga del firmware
<p>
  Para programar el Arduino Nano de la tarjeta de control se debe conectar
  a la computadora mediante el cable USB y utilizar el 
  <a href="https://www.arduino.cc/en/main/OldSoftwareReleases">IDE</a> de Arduino para
  programarlo con el firmware del Escornabot para esta versión del Escornabot. Se puede 
  descargar aquí: <a href="https://github.com/wgaonar/Escornabot-Arduino.git"> 
    firmware Escornabot Brivoi Compactus</a> el cual se encuentra configurado y 
    y calibrado para que funcione correctamente. Una imagen de la apaciencia del firmware abirto en la IDE de Arduino se muestra en la Figura 26:
</p>

<figure style="text-align: center; width:70%; 
              margin-left: auto; 
              margin-right: auto;">
    <img style="border-radius: 0px;" src="../assets/images/Post08/CPU/cpu_ArduinoIDE.png"
    alt="cpu_ArduinoIDE.png" width="100%"/>
  <figcaption>
    Figura 26: Robot Escornabot Brivoi Compactus
  </figcaption>
</figure>

<p>
  Ahora se necesita conectar el Arduino Nano de la tarjeta de control a la computadora
  a través del cable USB como se muestra en al Figura 27a y har click en el botón con el ícono de la flecha hacia
  la derecha que acciona el comando de  "subir" o "<i>upload</i>" el programa al Arduino 
  Nano. Al terminar de programarlo, se debe escuchar un sonido en el buzzer de la tarjeta
  de control, tal como se puede escuchar al <b>final</b> del Video 27b.
</p>

<div class="row">
  <div class="column">
    <figure>
      <img src="../assets/images/Post08/CPU/cpu_programada_a.jpg"
        alt="paso10a" style="width:100%">
      <figcaption>
        Figura 27a: Conexión de la tarjeta de programación.        
      </figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
      <video width="100%" controls poster="../assets/images/Post08/CPU/cpu_programada_a.jpg">
        <source src="../assets/images/Post08/CPU/cpu_programacion.mp4" type="video/mp4">
      </video>
      <figcaption>
         Video 27b: Programación del firmware del Escornabot.
      </figcaption>
    </figure>
  </div>
</div>

### Prueba de funcionamiento
<p>  
  La primera prueba después de cargar el firmware en el Escornabot, consiste en hacer que el robot ejecute una secuencia de movimientos. Para ello, se puede llevar a cabo el siguiente procedimiento:
  <ol>
    <li>Energizar el Escornabot con el interruptor de alimentación en la tarjeta de control</li>
    <li>Oprimir varias veces algunos de los botones de colores.</li>
    <li>Oprimir el botón de 
      <span style="background-color: black; color: white;">GO</span>.</li>
  </ol>
  Como resultado, se deberá empezar a moverse ejecutando en order la rutina de 
  movimientos programada.
</p>
