---
layout: page
title: Escornabot
permalink: /escornabot/
---

<figure style="text-align: center">
  <p style="text-align: center"> 
    <img width="50%" src="/assets/Escornabot/images/EscornabotPortada.jpeg"/>
  </p>
  <figcaption>
    Figura 1: Robot Escornabot Brivoi Compactus
  </figcaption>
</figure>

En esta página encontrarás toda la información para construir el robot educativo Escornabot en su versión Brivoi Compactus. Está basado en el proyecto: [Escornabot](https://escornabot.com/es/index) desarrollado en España. 

También puedes visitar la página de Pablo de Cierzo [pablorubma.cc](https://pablorubma.cc/) quien ha llevado a estos robots "chiquitines" a muchos niños también en España con su lema: "Pon un Escornabot en tu vida".

<h1>Introducción</h1>

Escornabot es un robot móvil de software y hardware abierto diseñado para la enseñanza de electrónica y programación en niños, adolescentes y no tan niños. Basado en la tarjeta [Arduino Nano](https://store.arduino.cc/usa/arduino-nano), destaca la sencillez de su diseño mecánico, electrónico y la estructura del código con el cual funciona. Todo esto ha sido posible por la comunidad que lo desarrolla y mantiene actualizado. Existen diversas versiones y la que corresponde a este manual es la <b>Brivoi Compactus</b> que se puede en la siguiente figura:

<figure style="text-align: center">
  <p style="text-align: center"> 
    <img src="/assets/Escornabot/images/3DParts/portadaOnshape_1.png"
    alt="Modelo 3D del Escornabot Brivoi Compactus" width="360"/>
  </p>
  <figcaption>
    Figura 2: Modelo 3D del Escornabot Brivoi Compactus
  </figcaption>
</figure>

Esta versión utiliza dos tarjetas de circuitos impresos (PCB):
<ul>
  <li><a href="https://github.com/xdesig/escornabot-electronics/tree/master/Electronics/E_KEYPAD/E_KEYPAD_2.20">E-KeyPad V2.2 diseñada por XDeSIG</a></li>
  <li><a href="https://github.com/xdesig/escornabot-electronics/tree/master/Electronics/EscornaCPU/1.x/1.20">EscornaCPU V1.2 diseñada por XDeSIG</a></li>
</ul>

<h2>Partes Impresas en 3D</h2>
El cuerpo del Escornabot está compuesto por 7 partes impresas en 3D que se muestran en la siguiente figura y que se explican enseguida.

<figure style="text-align: center">
  <p style="text-align: center"> 
    <img src="/assets/Escornabot/images/3DParts/Exploded_view_1.png"
    alt="Vista Explosionada de las partes impresas en 3D" width="360"/>
  </p>
  <figcaption>
    Figura 3: Vista Explosionada de las partes impresas en 3D
  </figcaption>
</figure>

<ol>
  <li><em>Motor-Bracket:</em> Parte central del robot a la que se fijan los motores y algunas de las otras piezas impresas en 3D.</li>
  <li><em>EscornaCPU_V1.2-Bracket:</em> Parte a la que se fija la tarjeta de control.</li>
  <li><em>E-KeypadV2.2-Bracket:</em> Parte a la que se fija la tarjeta de la botonera.</li>
  <li><em>Battery-Bracket:</em> Parte en donde van alojadas las 4 baterías AA que alimentan el Escornabot.</li>
  <li><em>Wheel-Right:</em> Rueda derecha del Escornabot.</li>
  <li><em>Wheel-Left:</em> Rueda izquierda del Escornabot.</li>
  <li><em>Ball-Caster:</em> Parte que aloja la esfera que sirve de pivote para que el Escornabot pueda desplazarse y girar.</li>
</ol>

<h1>Componentes Electrónicos</h1>

<h2>Lista de Componentes Electrónicos</h2>
La siguiente Tabla detalla la cantidad, descripción y la tarjeta (Botonera o la CPU) en la que irán colocados cada uno de los componentes del Escornabot. 

<table>
    <colgroup>
       <col span="1" style="width: 20%;">
       <col span="1" style="width: 40%;">
       <col span="1" style="width: 20%;">
       <col span="1" style="width: 20%;">
    </colgroup>
  <caption>Lista de Componentes para el Escornabot</caption>
  <tr>
    <th style="text-align:center">CANTIDAD TOTAL</th>
    <th style="text-align:center" >DESCRIPCIÓN</th>
    <th style="text-align:center">CANTIDAD BOTONERA</th>
    <th style="text-align:center">CANTIDAD CPU</th>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Tarjeta EscornaCPU versión 1.2</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Tarjeta E_KeyPad versión 2.2</td>
    <td style="text-align:center">1</td>
    <td style="text-align:center">-</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Arduino Nano</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>2 Motor paso a paso 28BYJ-48</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Driver ULN2803</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Zócalo de 18 pines para el driver ULN2803</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Portapilas para 4 pilas AA</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Terminal T-block de 5mm para alimentación</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">4</td>
    <td>Resistencia 1 KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">9</td>
    <td>Resistencia 10 KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 18 KΩ o de 20KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 22 KΩ o de 20KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Azul/td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Rojo</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Amarillo</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Led de 3mm Verde</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 18 KΩ o de 20KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 18 KΩ o de 20KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>
  <tr>
    <td style="text-align:center">1</td>
    <td>Resistencia 18 KΩ o de 20KΩ</td>
    <td style="text-align:center">-</td>
    <td style="text-align:center">1</td>
  </tr>

1 
1 
1 
1 
1 Led de 3mm Amarillo
1 Led de 3mm Verde
5 Pulsadores de 12mm
7 Pines/headers macho a 90◦
8 Pines/headers macho rectos
1 Puente o Jumper para pines rectos
- 2
- 1
- 1
- -
- 1
4 -
5 4
- 1
1 -
1 -
1 -
1 -
1 -
5 -
7 -
- 8
- 1
                 Tira de 15 pines hembra que serán la base so- 2-2
bre la cual se colocará el Arduino Nano
Interruptor de alimentación SK12F14 o 1-1
Conector macho para motor paso a paso JST- 2-1
1
Tira de 4 pines hembra si posteriormente se desea colocar un adaptador Bluetooth para co- nectar el Escornabot con una aplicación para teléfono móvil.
-
1
SK12D07
 XHP-5
1 Buzzer pasivo CFG12 para Arduino
1 Fusible rearmable XF050
1 Diodo Schottky 1N5817
2 Condensadores cerámicos 104 de 100nF
6 Cables Dupont hembra - hembra de 10cm
- 1
- 1
- 1
- 2
6 -
</table>