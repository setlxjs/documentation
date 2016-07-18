# 4 Design

In diesem Abschnitt werden grundlegende Architekturentscheidungen dargestellt und begründet.
Das Projekt ist in 3 Pakete aufgeteilt. Jedes Paket wird über NPM einzeln zur Verfügung gestellt und in einem getrennten Repository verwaltet. Die Pakete hängen teilweise von einander ab. Im Folgenden sind die Aufgaben und Funktionen der Bestandteile näher erläutert.

## 4.1 SetlX.js Transpiler

Der Transpiler bildet das Kernstück des Projekts. Er enthält einen Parser für SetlX und alle Funktionen, die benötigt werden um SetlX in JavaScript zu übersetzen. Allerdings bietet dieses Paket nach außen hin nur Programmierschnittstellen an, die von anderen Programmen verwendet werden können. Diese Schnittstelle ist im Kapitel Design näher dokumentiert. Als Benutzerschnittstellen sind viele unterschiedliche Möglichkeiten denkbar. Zum Beispiel kann der Transpiler von einem Command Line Interface verwendet werden, in einem Browser on-the-fly aufgerufen werden oder von einem Gulp- oder Webpack-Plugin verwendet werden.

Der Transpiler kann in zwei Bestandteile aufgetrennt werden. Zuerst wird der SetlX Sourcecode von einem Parser in einen Abstrakten Syntaxbaum (AST) umgewandelt. Danach kann der Syntaxbaum vom eigentlichen Transpiler in JavaScript Sourcecode umgewandelt. In diesem generellen Designmerkmal unterscheidet sich der Transpiler nicht von einem Compiler, der Sourcecode zuerst parst in einen Syntax- oder Parsebaum und diesen dann in einem weiteren Schritt entweder in Maschinen- bzw. Bytecode oder eine Programmiersprache niedrigeren Abstraktionslevels.

### 4.1.1 Der Parser

#### 4.1.1.1 Auswahl des Parsergenerators

Für die Auswahl eines Parsergenerators wurden drei Optionen betrachtet.

__Antlr 4 mit JavaScript Target__

SetlX wird von einem Javaprogramm interpretiert. Der dazugehörige Parser wird mit Hilfe einer [Antlr 3 Grammatik](https://github.com/herrmanntom/setlX/blob/master/interpreter/core/src/main/antlr/SetlXgrammar.g) erzeugt. [Antlr 4](http://www.antlr.org) zusammen mit dem [Javascript Target]() zu verwenden ist also naheliegend, da so die SetlX Grammatik für den Java Interpreter vermeindlich übernommen werden kann.

In einem Versuch die Antlr 3 Grammatik in eine Antlr 4 Grammatik umzuwandeln stellte sich schnell auf Grund der Unterschiede zwischen den Versionen als aufwändig heraus. Desweiteren erreichte der generierte Parser sehr schnell das Speicherlimit der Node.js Virtual Machine. Eine Umsetzung mit Antlr 4 wurde aus diesen Gründen nicht weiter verfolgt.

__JISON__

JISON ist einer der bekanntesten Parsergeneratoren und wird auch von anderen Transpilern verwendet. JISON unterstützt im Gegensatz zu Antlr und PEG.js nur kontextfreie Grammatiken. Da die SetlX Grammatik jedoch als kontextsensitive Grammatik formuliert ist, müsste die Grammatik mit großem Aufwand umformuliert werden.

__PEG.js__

PEG.js ist ein Parsergenerator für kontextsensitive Grammatiken in JavaScript. Der Name impliziert bereits, dass PEG ähnlich wie Antlr für _Parsing Expression Grammar_ entwickelt wurde. Das hat den Vorteil, dass auch hier die SetlX Grammatik bis auf einige Syntaxänderungen in ihrer Struktur erhalten bleiben kann. Lediglich die Aktionen müssen an Javascript angepasst werden.

PEG.js ist in JavaScript geschrieben und integriert sich im Gegensatz zu Antlr problemlos in den Softwarestack. Mit Hilfe eines [Plugins](https://github.com/lazutkin/gulp-peg) für [Gulp](http://gulpjs.com/) kann der Buildprozess automatisiert werden.

PEG.js stellt sich als mächtige Lösung heraus, die sich in die verwendete Technologien ohne Probleme einfügen lässt. SetlX.js setzt deshalb auf PEG.js als Parsergenerator.

#### 4.1.1.2 Syntaxbaumklassen

Das Ergebnis des Parsers muss für die nächsten Schritte in einer Datenstruktur festgehalten werden. Dafür werden in der Regel Syntaxbäume verwendet. Um die Erstellung des Syntaxbaums einfacher zu gestalten werden Klassen angelegt, mit denen die Knoten im Baum einfach generiert werden können. Für fast jeder Regel wird dafür eine Klasse erstellt. Eine Außnahme bilden die Listenregeln, etwa _ExpressionList_. Sie geben statt einer eigenen _ExpressionList_-Klasse einfach eine Liste bzw. einen JavaScript Array von _Expressions_ zurück. So können neue Features durch die Erstellung von Parseregeln in der Grammatik und entsprechenden Klassen hinzugefügt werden, ohne mit bestehenden Bestandteilen in Konflikt zu geraten.

### 4.1.2 Der Transpiler

Für den Transpiler wird ebenso ein modularer Aufbau verwendet. Für jeden Knotentyp im AST wird eine Transpilerfunktion erstellt. Eine solche Funktion kümmert sich im Idealfall auch nur um den Knoten selbst, nicht um darunter oder darüber liegende Knoten. Wenn eine neue Art von Token durch den Transpiler übersetzt werden soll, kann einfach eine Transpilerfunktion hinzugefügt werden.

Eine Transpilerfunktion erhält also einen Teilbaum des AST als Übergabeparameter und gibt einen String zurück. Dieser String enthält den übersetzten JavaScript Code. Statt direkt das Ergebnis zurückzuliefern hätte auch ein JavaScript Syntaxbaum generiert werden können, der dann in einem weiterem Schritt aus diesem JavaScript generiert. Eine solche Architektur wird zum Beispiel von Babel verwendet und ist besonders dann vorteilhaft, wenn nur Teile des Baums modifiziert werden müssen, aber der größere Teil wiederverwendet werden kann. Dadurch können auch mehrere Transpiler einfach hintereinander geschaltet werden. Für SetlX.js ist es jedoch völlig ausreichend gleich das Ergebnis zu erstellen.

## 4.2 SetlX.js CLI

Das SetlX Command Line Interface ist eine Benutzerschnittstelle für das Umwandeln von SetlX Code in JavaScript mit Hilfe der Console. Das Paket ist speziell für Node.js entwickelt und benötigt im Gegensatz zu den anderen Paketen auch Node.js zur Ausführung. Es verwendet den SetlX Transpiler und stellt die Funktionen und Optionen als Befehle zur Verfügung. Hauptsächlich werden damit ganze SetlX Dateien oder Ordner zu JavaScript umgewandelt. Zu Beginn werden drei Kommandos zur Verfügung gestellt: Ein Kommando zur Ansicht des AST, hauptsächlich für Debug- und Präsentationszwecken. Ein Befehl zum transpilen von SetlX Sourcecode. Die daduch entstandenen JavaScriptdateien können dann von Node.js ausgeführt werden. Außerdem kann mit einem weiterem Befehl auch das Programm direkt ausgeführt werden.

## 4.3 SetlX.js Runtime Library

Die SetlX Runtime Library wird von Programmen benötigt, die von SetlX zu JavaScript umgewandelt wurden. Die Runtime Library implementiert zum einen Funktionen aus der SetlX Standard Library und zum anderen auch Helperfunktionen, die für die effektive Kompilierung von SetlX Sourcecode benötigt werden. Über die Herausforderungen und Designentscheidungen der wird in den nächsten Kapiteln näher eingegangen.

### 4.3.1 Herausforderungen

Während sich SetlX und JavaScript in einigen Konzepten und Funktionen sehr ähnlich sind, unterscheiden sie sich an anderen Stellen durchaus von einander. Beide Sprachen sind dynamisch typisierte Scriptsprachen und verwenden dabei ähnliche Typen. In zwei Punkten unterscheidet sich SetlX stark von JavaScript, was den Einsatz von Helperfunktionen und -Objekten notwendig macht.

* SetlX unterstützt als eines seiner Kernmerkmale Mengen als First Class Objects. In JavaScript ist zwar eine Klasse für die Erstellung von Sets vorhanden, allerdings werden durch diese Klasse kaum Operationen unterstützt. Die SetlX Runtime Library stellt deswegen eine eigene Implementierung in Form einer Helperklasse zur Verfügung.
* Desweiteren sind Operatoren in SetlX noch viel stärker überladen, als in JavaScript. Beispielsweise kann in beiden Sprachen der "+"-Operator für die Addition zweier Zahlen verwendet werden, oder für die Verkettung von zwei Strings. In SetlX können aber mit dem "+"-Operator auch Listen verkettet und Mengen vereinigt  werden. Aus diesem Grund muss zur Laufzeit der Typ einer Variable erkannt und dann die entsprechende Operation ausgeführt werden. Somit kann ein "+" in SetlX nicht durch ein "+" in JavaScript übersetzt werden.  Statt dessen wird eine Hilfsfunktion benötigt, die den Typ der Parameter erkennt und die gewünscht Aktion ausführt.

### 4.3.2 Standard Library

SetlX hat eine Standard Library, die grundlegende mathematische Funktionen und Operationen bereitstellt. Diese Funktionen sind normalerweise in Java implementiert. Damit die Funktionen auch in SetlX.js zur Verfügung stehen müssen sie entsprechend in JavaScript implementiert werden.
