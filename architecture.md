# 3. Design

Das Projekt ist in 3 Pakete aufgeteilt. Jedes Paket wird über NPM einzeln zur Verfügung gestellt und in einem getrennten Repository verwaltet. Die Pakete hängen teilweise von einander ab. Im Folgenden sind die Aufgaben und Funktionen der Bestandteile näher erläutert.

## 3.1 Setlx Transpiler

Der Transpiler bildet das Kernstück des Projekts. Er enthält einen Parser für SetlX und alle Funktionen, die benötigt werden um SetlX in JavaScipt zu übersetzen. Allerdings bietet dieses Paket nach außen hin nur Programmierschnittstellen an, die von anderen Programmen verwendet werden können. Diese Schnittstelle ist im Kapitel Design näher dokumentiert. Als Benutzerschnittstellen sind viele unterschiedliche Möglichkeiten denkbar. Zum Beispiel kann der Transpiler von einem Command Line Interface verwendet werden, in einem Browser on-the-fly aufgerufen werden oder von einem Gulp- oder Webpack-Plugin verwendet werden.

Der Transpiler kann in zwei Bestandteile aufgetrennt werden. Zuerst wird der SetlX Sourcecode von einem Parser in einen Abstrakten Syntaxbaum (AST) umgewandelt. Danach kann der Syntaxbaum vom eigentlichen Transpiler in JavaScript Sourcecode umgewandelt. In diesem generellen Designmerkmal unterscheidet sich der Transpiler nicht von einem Compiler, der Sourcecode zuerst parst in einen Syntax- oder Parsebaum und diesen dann in einem weiteren Schritt entweder in Maschienen- bzw. Bytecode oder eine Programmiersprache niedrigeren Abstraktionslevels. __Literatur, Literatur, Literatur...__

### 3.1.1 Der Parser

#### Auswahl des Parsergenerators

Für die Auswahl eines Parsergenerators wurden drei Optionen betrachtet.

__Antlr 4 mit JavaScript Target__

SetlX wird von einem Javaprogramm interpretiert. Der dazugehörige Parser wird mit Hilfe einer [Antlr 3 Grammatik](https://github.com/herrmanntom/setlX/blob/master/interpreter/core/src/main/antlr/SetlXgrammar.g) erzeugt. [Antlr 4](www.antlr.org) zusammen mit dem [Javascript Target]() zu verwenden ist also naheliegend, da so die SetlX Grammatik für den Java Interpreter vermeindlich übernommen werden kann.

In einem Versuch die Antlr 3 Grammatik in eine Antlr 4 Grammatik umzuwandeln stellte sich schnell auf Grund der Unterschiede als schwierig heraus. Desweiteren

__JISON__

JISON ist einer der bekanntesten Parsergeneratoren. JISON unterstützt im Gegensatz zu Antlr und PEG.js nur kontextfreie Grammatiken. Da die SetlX Grammatik jedoch als kontextsensitive Grammatik formuliert ist, müsste die Grammatik mit großem Aufwand umformuliert werden.

__PEG.js__

PEG.js ist ein Parsergenerator für kontextsensitive 

## 3.2. SetlX CLI

Das SetlX Command Line Interface ist eine Benutzerschnittstelle für das umwandeln von SetlX Code in Javascript mit Hilfe der Console. Das Paket ist speziell für Node.js entwickelt und benötigt im Gegensatz zu den anderen Paketen auch Node.js zur Ausführung. Es verwendet den SetlX Transpiler und stellt die Funktionen und Optionen als Befehle zur Verfügung. Hauptsächlich werden damit ganze SetlX Dateien oder Ordner zu JavaScript umgewandelt.

## 3.3. SetlX Runtime Library

Die SetlX Runtime Library wird von Programmen benötigt, die von SetlX zu JavaScript umgewandelt wurden. Die Runtime Library implementiert zum einen Funktionen aus der SetlX Standard Library und zum anderen auch Helperfunktionen, die für die effektive Kompilierung von SetlX Sourcecode benötigt werden. Über die Herausforderungen und Designentscheidungen der wird in den nächsten Kapiteln näher eingegangen.

### 3.3.1 Herausforderungen

Während sich SetlX und JavaScript in einigen Konzepten und Funktionen sehr ähnlich sind, unterscheiden sie sich an anderen Stellen durchaus von einander. Beide Sprachen sind dynamisch typisierte Scriptsprachen und verwenden dabei ähnliche Typen. In zwei Punkten  stark von JavaScript, was den Einsatz von Helperfunktionen und -Objekten notwendig macht.
SetlX unterstützt als eines seiner Kernmerkmale Mengen als First Class Objects. In JavaScript ist zwar eine Klasse für die Erstellung von Sets vorhanden, allerdings werden durch diese Klasse kaum Operationen unterstützt. Die SetlX Runtime Library stellt deswegen eine eigene Implementierung in Form einer Helperklasse zur Verfügung.
Desweiteren sind Operatoren in SetlX noch viel stärker überladen, als in JavaScript. Beispielsweise kann in beiden Sprachen der "+"-Operator für die Addition zweier Zahlen verwendet werden, oder für die Verkettung von zwei Strings. In SetlX können aber mit dem "+"-Operator auch Listen verkettet werden und Mengen vereinigt. Aus diesem Grund muss zur Laufzeit der Typ einer Variable erkannt werden und die entsprechende Operation ausgefürt werden. Somit kann ein "+" in SetlX nicht durch ein "+" in JavaScript übersetzt werden und eine Hilfsfunktion wird benötigt, die den Typ der Parameter erkennt und die gewünscht Aktion ausführt.
