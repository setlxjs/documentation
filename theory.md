# 2. Theorie

In diesem Kapitel werden die Rahmenbedingungen rund um SetlX und die relevanten Grundlagen der Compilerentwicklung erläutert.

## 2.1 Transpiler

Ein Transpiler (auch Transcompiler oder Source-to-Source Compiler) ist ein Computerprogramm, das Programmcode von einer Programmiersprache in Programmcode einer anderen Programmiersprache übersetzt [[1]](http://www.injoit.org/index.php/j1/article/view/295/242). Diese Arbeit beschäftigt sich mit der Implementierung eines solchen Transpilers, um SetlX Sourcecode in JavaScript Sourcecode umzuwandeln. Transpiler sind von Compilern abzugrenzen, die in der Regel Programmcode in ausführbaren Maschienen- bzw. Bytecode umwandeln.

## 2.2 Parser

Ein Parser ist ein Programm, das Programmcode in semantische Bestandteile aufteilt. In der Regel wird das Ergebnis eines Parsers. Je nach Art des Parsers ist dem Parser noch ein sogenannter Lexer vorangestellt, der das Programm zuerst in Tokens aufteilt. Dann verarbeitet der Parser nicht direkt den Programmcode sondern den erzeugten Tokenstream.

## 2.3 Parser Generator

Ein Parser Generator ist ein Programm, das aus Programmdefinitionen, in der Regel in Form von sogenannten Grammatiken, einen Parser erzeugt. Häufig erzeugt ein Parsergenerator auch nur Programmcode, der dann zu einem Parser kompiliert werden kann. SetlX.js nutzt einen Parsergenerator um einen Parser in JavaScript zu erzeugen.

## 2.4 SetlX

### 2.4.1 Type System

Das SetlX Type System kennt verschiedene Primitive Typen: Booleans sind klassische Boolsche Wahrheitswerte ausgedrückt durch die Literale `true` und `false`. Strings sind Zeichenketten. Neben Integers und Fließkommazahlen stehen in SetlX auch rationale Zahlen zur Verfügung. Weiterhin kennt SetlX Listen, Mengen, Funktionen, reguläre Ausdrücke und Objekte. SetlX.js implementiert allerdings nur Booleans, Integers, Doubles, Strings, Funktionen, Listen und Mengen.
