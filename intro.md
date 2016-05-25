# Introduction

## Einführung

Das SetlX.js Projekt entwickelt einen Transpiler für die Sprache SeltX. Die Zielsprache des Transpilers ist JavaScript.

SetlX ist eine höhere Programmiersprache und wird hauptsächlich für Bildungszwecke verwendet. SetlX implemetiert Mengen als First-Class-Objekte. Dadurch können viele Konzepte aus der Mengenlehre direkt angewandt werden. SetlX Code wird standardmäßig von einem Interpreter auf der Java Virtual Machine ausgeführt.

## Motivation

Für die Verwendung von SetlX ist eine Installation auf einem System mit installierter Java Virtual Machine notwendig. Dieser Schritt stellt eine Einstiegshürde da, die durch eine Umwandlung in JavaScript wegfallen kann. JavaScript kann von einem Browser innerhalb einer Webseite ausgeführt werden. Neben Browsern
One of SetlX problems is the high hurdle and the required setup. By transpiling SetlX to Javascript it can run inside the Browser and across devices since Javascript nowdays runs on the web, on servers, PCs, mobile devices and even televisions. As Javascript compiled SetlX could also access the Javascript eco system.

## Background

Javascript became one of the most popular programming languages. Other than languages that run on their own virtual machine or to machine code compiled languages, which depend on the implementation of one compiler, javascript needs to run in different browsers and engines. By the time of this writing five browser make up nearly all of the web traffic: Google Chrome, Firefox, Internet Explorer/MS Edge, Safari and Opera. Only Opera and Chrome share their Javascript implementation. Also many old browsers are still in use. Introducing and using new language features for Javascript can take up to many years.

This state lead to many transpilers written for custom languages or existing languages.

[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/About_JavaScript#What_JavaScript_implementations_are_available
[2]: https://github.com/jashkenas/coffeescript/wiki/list-of-languages-that-compile-to-js
