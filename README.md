# 4-bit adder with BCD converter and 7-segment display 

Real world adder, BCD converter and display made in Minecraft using Minecraft redstone and redstone-based logic gates. It can add two 4-bit numbers and display numbers from 0 to 30. 

<div align="center">
   <img height="400" src="https://github.com/user-attachments/assets/3e4de494-f249-4ee4-a19d-036813df6b13" alt="entire minecraft circuit" />
</div>

## Table of contents

- [Compatibility](#compatibility)
- [Installation](#installation)
- [Demo](#demo)
- [Made with](#made-with)
- [How it works](#how-it-works)
   - [Truth table](#truth-table)
   - [Karnaugh map](#karnaugh-map)
   - [Corresponding circuit](#corresponding-circuit)
- [Logic gates](#logic-gates)
- [Minecraft logic gates](#minecraft-logic-gates)
- [Half adder and full adder](#half-adder-and-full-adder)
- [BCD converter](#bcd-converter)

## Compatibility

Minecraft version &ge; 1.18

I guess that's mainly because of the new world generation because older versions won't even load the world and instead they generate a new one. However you can easily recreate the same redstone mechanisms in older versions of Minecraft.

## Installation

1. Download ZIP and extract.

   ![image](https://github.com/user-attachments/assets/6176cd00-b270-402f-ad60-0e287acdf4c3)

   or alternatively

   Clone the repository into any folder using git:
   ```
   git clone https://github.com/Me-Wosh/Minecraft4BitAdderWithDisplay.git
   ```

2. Copy Minecraft world into your saves folder:
   
   | System  | Location                                                   |
   | ------- | ---------------------------------------------------------- |
   | Windows | `%APPDATA%\.minecraft\saves`                               |
   | Linux   | `~/Library/Application Support/minecraft/saves`            |
   | MacOS   | `~/.minecraft/saves`                                       |

## Demo

[minecraft circuit demo](https://github.com/user-attachments/assets/efc949db-956c-4608-8483-e4b7622ee5ee)

## Made with

- Minecraft redstone
- [NeoForge](https://neoforged.net/) and [WorldEdit](https://www.curseforge.com/minecraft/mc-mods/worldedit) mod
- [Karnaugh Map Solver](https://www.charlie-coleman.com/experiments/kmap/)

## How it works

You can build any electronic circuit using logic gates, a truth table and a Karnaugh map.

Let's consider the following example (7-segment display), where each segment has a corresponding letter A-G:

![7-segment display](https://github.com/user-attachments/assets/c12c285d-6860-4bc7-85ee-c4e06627a632)

Image author: Uln2003

### Truth table

We construct a truth table with all the possible 2-bit inputs and outputs. Since a single display can only display numbers from 0 to 9 our inputs need to contain every 2-bit number from 0 to 9 and since we have 7 segments we will have 7 outputs.

| x<sub>0</sub> | x<sub>1</sub> | x<sub>2</sub> | x<sub>3</sub> | A | B | C | D | E | F | G |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  0  |  0  |  0  |  0  |  1  |  1  |  1  |  1  |  1  |  1  |  0  |
|  0  |  0  |  0  |  1  |  0  |  1  |  1  |  0  |  0  |  0  |  0  |
|  0  |  0  |  1  |  0  |  1  |  1  |  0  |  1  |  1  |  0  |  1  |
|  0  |  0  |  1  |  1  |  1  |  1  |  1  |  1  |  0  |  0  |  1  |
|  0  |  1  |  0  |  0  |  0  |  1  |  1  |  0  |  0  |  1  |  1  |
|  0  |  1  |  0  |  1  |  1  |  0  |  1  |  1  |  0  |  1  |  1  |
|  0  |  1  |  1  |  0  |  1  |  0  |  1  |  1  |  1  |  1  |  1  |
|  0  |  1  |  1  |  1  |  1  |  1  |  1  |  0  |  0  |  0  |  0  |
|  1  |  0  |  0  |  0  |  1  |  1  |  1  |  1  |  1  |  1  |  1  |
|  1  |  0  |  0  |  1  |  1  |  1  |  1  |  1  |  0  |  1  |  1  |

The ones under A, B, C, D, E, F and G segments indicate whether the segment is lit. For example when we want to display number 4 (0100 in a binary system) we need to light up B, C, F and G segments.

![number 4 on 7-segment display](https://github.com/user-attachments/assets/1861c8cd-2bf0-4d2b-8513-11bc7f6ae8ef)

### Karnaugh map

After we fill up our table it's time to construct a Karnaugh map so that we can build a circuit for each segment. I will use the segment E as an example.

| x<sub>0</sub>x<sub>1</sub> \ x<sub>2</sub>x<sub>3</sub> | 00 | 01 | 11 | 10 |
| :--: | :-: | :-: | :-: | :-: |
|  00  |  1  |  0  |  0  |  1  |
|  01  |  0  |  0  |  0  |  1  |
|  11  |  0  |  0  |  0  |  0  |
|  10  |  1  |  0  |  0  |  0  |

The next step is to group each number one into the least amount of the largest possible groups of adjacent ones. You can "fold" a Karnaugh map in half horizontally and/or vertically to achieve this. A group needs to be of 2<sup>n</sup> size, where n &ge; 0. More details: [grouping number ones in a Karnaugh map](https://en.wikipedia.org/wiki/Karnaugh_map#Grouping). Also, you can see that the inputs in a Karnaugh map aren't in the "correct" binary order, instead they are ordered in [Gray code](https://en.wikipedia.org/wiki/Gray_code). So instead of 00, 01, 10, 11 you have, in this case: 00, 01, 11, 10.

### Corresponding circuit

After that we can read the contents of the Karnaugh map to get the minimized Boolean function. You can get the function yourself, which is explained here: [solving Karnaugh map](https://en.wikipedia.org/wiki/Karnaugh_map#Solution) or use a program to achieve this, for example: [Karnaugh map solver](https://www.charlie-coleman.com/experiments/kmap/) (especially helpful when there are more than 4 inputs). 

In this case f(x<sub>0</sub>, x<sub>1</sub>, x<sub>2</sub>, x<sub>3</sub>) = !x<sub>0</sub>x<sub>2</sub>!x<sub>3</sub> + !x<sub>1</sub>!x<sub>2</sub>!x<sub>3</sub> ,

where multiplication represents AND logic gate, addition OR gate and ! represents NOT gate.

## Logic gates

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="2" align="center">NOT</td>
      <td rowspan="2"><img src="https://github.com/user-attachments/assets/174e2048-3ab2-4414-af15-09923d399fb5" alt="not logic gate"/></td>
      <td>0</td>
      <td>1</td>
   </tr>
   
   <tr>
      <td>1</td>
      <td>0</td>
   </tr>
</table>
   
<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">AND</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/2e1d3382-4879-4d4a-9eb1-4c9a8b9f13d9" alt="and logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>1</td>
   </tr>
</table>

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">NAND</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/e08e3b17-1c61-47f7-a679-cecdf8fe4f1c" alt="nand logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>0</td>
   </tr>
</table>

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">OR</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/6cbbab98-ebae-465f-8b72-831cf3b82a91" alt="or logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>1</td>
   </tr>
</table>

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">NOR</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/5a16c7d5-4b91-45bb-a4a7-0aabfa75efa8" alt="nor logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>0</td>
   </tr>
</table>

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">XOR</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/8a9ebf5c-c299-40c7-9d11-b249c10ddcf2" alt="xor logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>1</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>0</td>
   </tr>
</table>

<table>
   <tr>
      <th>Logic gate</th>
      <th>Symbol</th>
      <th>A</th>
      <th>B</th>
      <th>Y</th>
   </tr>
   
   <tr>
      <td rowspan="4" align="center">XNOR</td>
      <td rowspan="4"><img src="https://github.com/user-attachments/assets/56689824-7adb-4f2b-8aa1-cfd1d824a483" alt="xnor logic gate"/></td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
   </tr>
   
   <tr>
      <td>0</td>
      <td>1</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>0</td>
      <td>0</td>
   </tr>

   <tr>
      <td>1</td>
      <td>1</td>
      <td>1</td>
   </tr>
</table>

Images author: [Inductiveload](https://commons.wikimedia.org/wiki/User:Inductiveload)

## Minecraft logic gates

Here are logic gates I used:

<img width="200" src="https://github.com/user-attachments/assets/bc67ac6a-7360-4f2a-99a6-35bbe6fcf5db" alt="minecraft not logic gate" /> NOT

<img width="200" src="https://github.com/user-attachments/assets/1ecf279b-32a5-4e7a-8b2a-18ab129814ef" alt="minecraft and logic gate" /> AND

<img width="200" src="https://github.com/user-attachments/assets/4a8fb014-aed0-4622-9e23-ccd3c90efbdf" alt="minecraft or logic gate" /> OR (yes, in the real world you would end up in prison for combining 2 cables into 1).

<img width="200" src="https://github.com/user-attachments/assets/10abfb7d-b364-4d12-816f-ac0ba117a9e5" alt="minecraft xor logic gate" /> XOR

## Half adder and full adder

Adder is a digital circuit that does exactly what the name suggests - adds 2 numbers (A and B), produces a result (S) of this operation and an overflow (C or C<sub>out</sub>).

![half adder](https://github.com/user-attachments/assets/10d0c440-ddd2-4be9-aef6-f9712ae1c5ea) Half adder
![fulladder2](https://github.com/user-attachments/assets/4adecf03-7d2c-499e-85bd-64bd8b8f808b) Full adder

Images authors: [Inductiveload](https://commons.wikimedia.org/wiki/User:Inductiveload), [Cburnett](https://en.wikipedia.org/wiki/User:Cburnett)

The difference between a full adder and a half adder is that the full adder not only produces an overflow as an output but also takes an overflow as an input. This way you can combine multiple adders together to handle larger inputs. In my case I combined 4 adders to achieve addition of two 4-bit numbers.

## BCD converter

Since any binary number is just a chain of ones and zeroes it doesn't give us enough information about the number we want to display. For example number 12 in a binary system is just 1100 - not helpful at all. That's why we can use one of many BCDs - [Binary Coded Decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal) to transform a binary number into a more readable input. Since 12 acts as two numbers: 1 and 2 I also converted the binary number into 2 numbers: 0001 (1) and 0010 (2). Now I can tell my first display to show the number 1 and my second display to show the number 2.

You can see that I used a 5-bit BCD converter even though my adder is only 4-bit. That's because addition of two 4-bit numbers can result into any number from 0 to 30 however numbers larger than 15 are not 4-bit anymore and instead they are 5-bit. Since the output overflow from the last adder already acts as the 5-th bit I just passed it into my BCD converter.
