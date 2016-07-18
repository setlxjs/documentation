# 1 Einleitung

## 1.1 Über das Thema der Arbeit

SetlX ist eine höhere Programmiersprache und wird hauptsächlich für Bildungszwecke verwendet. SetlX implemetiert Mengen als First-Class-Objekte. Dadurch können viele Konzepte aus der Mengenlehre direkt angewandt werden. SetlX Code wird standardmäßig von einem Interpreter auf der Java Virtual Machine ausgeführt. Das SetlX.js Projekt entwickelt einen Transpiler für die Sprache SeltX. Die Zielsprache des Transpilers ist JavaScript. Dadurch kann SetlX erstmals auf virtuellen Maschinen für JavaScript ausgeführt werden.

## 1.2 Motivation

Für die Verwendung von SetlX ist eine Installation auf einem System mit installierter Java Virtual Machine notwendig. Dieser Schritt stellt eine Einstiegshürde da, die durch eine Umwandlung in JavaScript wegfallen kann. JavaScript kann von einem Browser innerhalb einer Webseite ausgeführt werden. Aber nicht nur im Browser ist JavaScript anzutreffen, sondern auch auf Servern, PCs, mobilen Engeräten und sogar Fernsehern. Mit dem größten Anzahl an Paketen unter den Paketmanagern der populären Sprachen bietet JavaScript außerdem ein riesiges Ökosystem und eine starke Community.

Teile dieses Umfeldes könnten durch SetlX genutzt werden, wenn SetlX Code in JavaScript umgewandelt wird.

## 1.3 Hintergrund

In den letzten Jahren ist JavaScript von einer Scriptsprache, die nur Anwendung im Browser fand, zu einer der beliebtesten Programmiersprachen aufgestiegen. Gleichzeitig ist JavaScript die Sprache mit den meisten Transpilern. Ein Grund dafür ist die die Tatsache, dass JavaScript auf [vielen verschiedenen virtuellen Maschinen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/About_JavaScript#What_JavaScript_implementations_are_available) läuft. Für viele Jahre wurde die Entwicklung der EcmaScript Definition nur langsam vorrangetrieben. Hinzu kam die Tatsache, dass die Umsetzung der neuen Definitionen nur in den neusten Browserversionen gemacht werden konnte. Dieser Zustand führte so zu einem Anstieg an Transpilern, die Code unterschiedlichster Sprachen in JavaScript verwandeln.

Kaum eine der bekanntesten Programmiersprachen kann heute nicht durch einen Transpilierungsvorgang im Browser ausgeführt werden. Mit dieser Arbeit kann auch SetlX auf der Liste dieser Sprachen erscheinen.
