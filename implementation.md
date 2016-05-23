# 4 Implementierung

## 4.1 SetlX Transpiler

### 4.1.1 Werkzeuge

Für die Erstellung des Transpilers kommen unterschiedliche Tools aus dem Node.js Ökosystem zum Einsatz.

__Gulp__
[Gulp?](gulpjs.com) ist ein sogenannter Taskrunner. Gulp wird verwendet um die unterschiedlichen Buildmethoden zu vereinen und Builds automatisch und nach bedarf zu starten. Gulp verwendet Plugins um Babel und PEG.js anzusteuern. Gulp verwendet sogenannte Streams um Sourcedateien zu transformieren und im Zielverzeichnes abzulegen. Gulp kann außerdem das Dateisystem beobachten und Änderungen dann in Echtzeit in neuen Builds zusammenstellen.

__Babel__
[Babel?](babel.io) ist der bekannteste Transpiler für zukünftige JavaScriptversionen. Babel verwandelt so zum beispiel ES2015 Code in ES5 Code um. Auf diese Weise kann der Code auch auf Umgebungen  ausgeführt werden, die die neuen Features der aktuellen EcmaScript Definition noch nicht unterstützen. Zum Zeitpunkt des Projektes unterstützen die wenigsten Umgebungen alle neuen Features.

__PEG.js__
Ist ein Parsergenerator für kontextsensitive Grammtiken. Im Kapitel 3.1.1.1 wurde dieses Werkzeug bereits vorgestellt. Wichtig ist an dieser Stelle noch, dass PEG.js auf Node.js läuft und wie Babel mit Hilfe eines Gulp Plugins Builds in Echtzeit erzeugen kann.

__Mocha + Should.js__
Mocha ist ein Unit Test Framework. Mocha macht es möglich definierte Unit Tests auszuführen und die Ergebnisse dann in der Konsole zu

### 4.1.2 Grammatik

Die SetlX.js Grammatik bedient sich der Struktur der originalen SetlX Grammatik. Die Grammatik

## SetlX.js Library

Die SetlX.js Library ist in ES2015 geschrieben und wird, wie der Transpiler für die Verwendung im Browser und Node.js zuerst von Babel transpiliert. Von der Bibliothek werden Funktionen bereitgestellt, die ein in JavaScript übersetztes SetlX Programm brauchen könnte.

### Struktur

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


##  SetlX.js CLI

Das SetlX.js Command Line Interface wurde speziell für Node.js entwickelt und ist in ES5 geschrieben. Es läuft so nativ auf V8, der Virtuellen Maschiene in Node.js. SetlX.js bildet einen schmalen Wrapper um den Transpiler und stellt Funktionen und Optionen über Flags bereit.
