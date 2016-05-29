# Introduction

## Einführung

Das SetlX.js Projekt entwickelt einen Transpiler für die Sprache SeltX. Die Zielsprache des Transpilers ist JavaScript.

SetlX ist eine höhere Programmiersprache und wird hauptsächlich für Bildungszwecke verwendet. SetlX implemetiert Mengen als First-Class-Objekte. Dadurch können viele Konzepte aus der Mengenlehre direkt angewandt werden. SetlX Code wird standardmäßig von einem Interpreter auf der Java Virtual Machine ausgeführt.

## Motivation

Für die Verwendung von SetlX ist eine Installation auf einem System mit installierter Java Virtual Machine notwendig. Dieser Schritt stellt eine Einstiegshürde da, die durch eine Umwandlung in JavaScript wegfallen kann. JavaScript kann von einem Browser innerhalb einer Webseite ausgeführt werden. Aber nicht nur im Browser ist JavaScript anzutreffen, sondern auch auf Servern, PCs, mobilen Engeräten und sogar Fernsehern. Mit dem größten Anzahl an Paketen unter den Paketmanagern der populären Sprachen bietet JavaScript ein riesiges Ökosystem und eine starke Community.

## Hintergrund

In den letzten Jahren ist JavaScript von einer Scriptsprache, die nur Anwendung im Browser fand, zu einer der beliebtesten Programmiersprachen aufgestiegen. Gleichzeitig ist JavaScript die Sprache mit den meisten Transpilern. Ein Grund dafür ist die
Programmiersprachen, die auf ihrer eigenen virtuellen Maschiene laufen oder . Other than languages that run on their own virtual machine or to machine code compiled languages, which depend on the implementation of one compiler, javascript needs to run in different browsers and engines. By the time of this writing five browser make up nearly all of the web traffic: Google Chrome, Firefox, Internet Explorer/MS Edge, Safari and Opera. Only Opera and Chrome share their Javascript implementation. Also many old browsers are still in use. Introducing and using new language features for JavaScript can take up to many years.

This state lead to many transpilers written for custom languages or existing languages.

[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/About_JavaScript#What_JavaScript_implementations_are_available
[2]: https://github.com/jashkenas/coffeescript/wiki/list-of-languages-that-compile-to-js
