# 4.2 SetlX.js Library

Die SetlX.js Library ist in ES2015 geschrieben und wird, wie der Transpiler für die Verwendung im Browser und Node.js zuerst von Babel transpiliert. Von der Bibliothek werden Funktionen bereitgestellt, die ein in JavaScript übersetztes SetlX Programm brauchen könnte.

## 4.2.1 Struktur

Toplevel Ordnerstruktur:
```
dist/
src/
test/
```
`src` enthält alle Sourcedateien, also die eigentlichen Funktionen in ES2015 Syntax. `dist` enthält Dateien, die von ES2015 zu ES5 übersetzt wurden. `test` enthält Unittests für einige der Funktionen. Jeder dieser Ordner enthält die gleiche Ordnerstruktur auf der zweiten Ebene:

```
class/
hlp/
std/
util/
```
`class` enthält Klassendefinitionen. `hlp` enthält alle Helperfunktionen. `std` enthält Funktionen der SetlX Standard Library. `util` enthält Funktionalitäten, die von den anderen Funktionen häufig verwendet werden.

Von der Library werden zwei unterschiedliche Arten von Funktionen exportiert. Zum einen werden die sogenannten Helperfunktionen angeboten. Helperfunktionen unterstützen das Programm mit Funktionalitäten, die nur schwer durch den Transpiler alleine abgebildet werden können. Eine genauere Betrachtung dieses Themas findet sich in Kapitel 3.3.1.
