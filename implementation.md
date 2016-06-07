# 5 Implementierung

## 5.1 SetlX Transpiler

### 5.1.1 Werkzeuge

Für die Erstellung des Transpilers kommen unterschiedliche Tools aus dem Node.js Ökosystem zum Einsatz.

[__Gulp__](http://gulpjs.com/) ist ein sogenannter Taskrunner. Gulp wird verwendet um die unterschiedlichen Buildmethoden zu vereinen und Builds automatisch und nach Bedarf zu starten. Gulp verwendet Plugins um Babel und PEG.js anzusteuern. Gulp verwendet sogenannte Streams um Sourcedateien zu transformieren und im Zielverzeichnes abzulegen. Gulp kann außerdem das Dateisystem beobachten und Änderungen dann in Echtzeit in neuen Builds zusammenstellen.

[__Babel__](https://babeljs.io) ist der bekannteste Transpiler für zukünftige JavaScriptversionen. Babel verwandelt so zum beispiel ES2015 Code in ES5 Code um. Auf diese Weise kann der Code auch auf Umgebungen  ausgeführt werden, die die neuen Features der aktuellen EcmaScript Definition noch nicht unterstützen. Zum Zeitpunkt des Projektes unterstützen die wenigsten Umgebungen alle neuen Features.

[__PEG.js__](http://pegjs.org/) ist ein Parsergenerator für kontextsensitive Grammtiken. Im Kapitel 3.1.1.1 wurde dieses Werkzeug bereits vorgestellt. Wichtig ist an dieser Stelle noch, dass PEG.js auf Node.js läuft und wie Babel mit Hilfe eines Gulp Plugins Builds in Echtzeit erzeugen kann.

[__Mocha__](https://mochajs.org/) ist ein Unit Test Framework. Mocha macht es möglich definierte Unit Tests auszuführen und die Ergebnisse dann in der Konsole darzustellen. Mocha selbst hat keine Assertion Bibliothek, deshalb wird zusätzlich [__Should.js__](https://shouldjs.github.io/) verwendet. Mit Hilfe von [__Sinon.js__](http://sinonjs.org/) werden außerdem Funktionsaufrufe validiert.

### 5.1.2 Grammatik

Die SetlX.js Grammatik bedient sich der Struktur der originalen SetlX Grammatik. Jede Regel, die zu einem unterstütztem Bestandteil von SetlX.js gehört, wurde von Antlr zu PEG.js übersetzt. In der sogenannten _Action_ der wird der abstrakte Syntaxbaum erstellt.

```js
// Beispiel einer Regel
Iterator
  = as:Assignable WS 'in' WS expr:Expression
    { return Iterator(as, expr); }
```

### 5.1.3 Syntaxbaum

Für die Erstellung von Knoten im AST werden für jede Art von Token eine eigene Klasse angelegt. Die Klasse verfügt über einen Konstruktor, der die Attribute festlegt. Als Attribut erhält jeder Knoten das Feld `token`. Es enthält einen konstanten String, der verwendet wird um zu entscheiden, welche Transpilerfunktion auf den Knoten angewandt werden muss.

Die Übergabeparameter unterscheiden sich je nach Art der Operation. Im unteren Beispiel

Außerdem wird die `toString` Methode implementiert, die in JavaScript dann aufgerufen wird, wenn ein Objekt impliziert zu einem String umgewandelt wird. Die `toString` Methode erzeugt einen String, der dem Kontrukturaufruf nahe kommt. Auf diese Weise kann diese Ansicht dazu verwendet werden den Syntaxbaum zu inspizieren.

```js
import { ITERATOR } from '../constants/tokens';

class Iterator {
  constructor(assignable, expression) {
    // String to identify type of Node
    this.token = ITERATOR;
    this.assignable = assignable;
    this.expression = expression;
  }

  toString() {
    return `Iterator( ${this.assignable}, ${this.expression} )`;
  }
}

// Constructor function gets exported to avoid new calls in requiring module
export default function creator(assignable, expression) {
  return new Iterator(assignable, expression);
}
```

### 5.1.3 createTranspiler

Die Funktion `createTranspiler` bindet die Plugins an die Transpilerfunktion. Durch die Dependency Injection kann der Transpiler einfach durch eigene Implementierungen der Plugins erweitert werden und besonders einfach getestet werden.

```js
import transpilers from './transpilers';

// [...]

export default function createTranspiler(plugins) {
  // override default plugins with plugins provided
  const mergedPlugins = Object.assign({}, defaultPlugins(), plugins);

  return function transpile(tree) {
    // look up matching transpiler function
    if (typeof transpilers[tree.token] !== 'function') {
      throw new Error(`Could not find transpiler for token type ${tree.token}`);
    }
    return transpilers[tree.token](tree, transpile, mergedPlugins);
  };
}
```

Die Funktion `transpile` entscheidet an der Art des Knotens welche Transpilerimplementierung verwendet werden muss. `src/transpilers/index.js` exportiert dafür eine Hashmap, die alle verfügbaren Transpiler bereitstellt. Danach wird die passende Funktion aufgerufen und neben dem Knoten auch Plugins und die `transpile` Funktion selbst übergeben.

### 5.1.4 Plugins

Transpilerfunktionen sind als pure Functions designt. Während die meisten Funktionen auch keine Seiteneffekte haben, müssen aber einige Funktionen Einfluss auf den globalen Zustand des Programms haben. In SetlX.js wird der globale Zustand mit Hilfe von Plugins verwaltet.

#### 5.1.4.1 Import Plugins

SetlX.js verwendet zwei Plugins, die unterschiedliche Arten von imports aus der SetlX.js Library bereitstellen. Das `StdLibPlugin` verwaltet Aufrufe der Standard Library, also Funktionen, die in SetlX global durch den Interpreter zur Verfügung gestellt werden. Das `HelperPlugin` stellt Funktionen zur Verfügung, die von dem übersetzten Programm benötigt werden, um alle SetlX-Funktionalitäten abzubilden. Diese Funktionen werden im Kapitel 4.2 näher vorgestellt.

#### 5.1.4.2 Scope Plugins

Das `ScopePlugin` verwaltet den Variablenscope. Anders als in SetlX müssen Variablen in JavaScript zuerst deklariert werden. SetlX.js deklariert dafür Variablen im Funktionsscope mit dem Keyword `var`. Ist eine Variable schon im höheren Scope deklariert, wird die Variable nicht erneut im aktuellen Scope deklariert. Auf diese Weise entsteht ein Closure. SetlX unterstützt in der Regel nur Closures, wenn explizit das Keywort `closure` anstatt `procedure` bzw. die _thin arrow function syntax_ (`|->`) verwendet wird. Allerdings erhält die Prozedur auch weiterhin Zugriff auf andere Funktionen aus dem oberen Scope. Da ein Idenfier weder den Typ des Wertes kennt, den er innehält, noch seinen Verwendungszweck wird hier einfach der gesamte obere Scope zur Verfügung gestellt. Mit einer Implementierung auf Ebene der Assignments könnte eine 1:1 Umsetzung zwar realisiert werden, jedoch wird hier aufgrund der Komplexität dieser Implementierung darauf verzichtet.

### 5.1.5 Transpilerfunktionen

Transpilerfunktionen werden für einen Tokentype geschrieben. Die Funktion kann im Fall eines Aufrufs sicher sein, dass der oberste Knoten des übergebenen Syntaxbaums vom Typ des spezifizierten Tokens ist. Jede Transpilerfunktion gibt als Ergebniss das übersetzte Programm für den übergebenen Teilbaum als String zurück.

Neben dem Baum erhält die Transpilerfunktion als zweiten Parameter die `transpile` Funktion, um rekursiv Unterbäume zu übersetzen. So übersetzt die im ersten Beispiel dargestellte Funktion für Disjunktionen nur die Disjunktion selbst. Die Parameter werden von `transpile` gehandhabt.

```js
// Recursive transpile calls in transpiler function
export default function disjunction(tree, transpile) {
  return transpile(tree.lefthand) + ' || ' + transpile(tree.righthand);
}
```

Andere Transpilerfunktionen verwenden Plugins im den globalen Zustand des Programms zu beeinflussen. Die `identifier` Funktion überprüft zuerst, ob es sich beim Namen des Identifiers um einen reservierten Namen aus der Standardbibliothek handelt. Ist das der Fall wird die Funktion automatisch vom Plugin importiert. In der Regel ist dies aber nicht der Fall und der Name wird im `ScopePlugin` registriert. Zum Schluss kann der Identifiername als String zurückgegeben werden.

```js
// Global state manipulation with plugins
export default function identifier({ name }, transpile, { scopePlugin, stdLibPlugin }) {
  if (!stdLibPlugin.isStd(name)) {
    scopePlugin.register(name);
  }
  return name;
}
```

Andere Transpiler erstellen Ausgaben mit Hilfe der Plugins. Jede Funktion erhält ihren eigenen Scope. Damit die Variablen deklariert werden muss die Deklaration als erstes im Funktionsbody ausgegeben werden. Der Transpiler für Prozeduren erstellt deshalb einen neuen Scope im `ScopePlugin`. Nachdem der Funktionsblock übersetzt wurde kann der Scope wieder geschlossen werden. Alle registrierten Variablen können dann an den Anfang des Funktionsblocks gesetzt werden.

```js
import indent from '../util/indent';

export default function procedure({ params, block }, transpile, { scopePlugin }) {
  // create a new scope and register function parameter names
  scopePlugin.newScope(params.map(param => param.name));
  const body = transpile(block);
  // close the scope and again and receive the varable definitions
  const varDefs = scopePlugin.closeScope();
  const fullBody = indent(2, (varDefs ? varDefs + '\n' : '') + body);
  return `function(${params.map(transpile).join(', ')}) {\n${fullBody}}`;
}
```

### 5.1.6 Tests

Der Transpiler wird mit rund 150 Unittests auf Korrektheit überprüft. Dabei werden Parser und Transpilerfunktionen getrennt getestet.

Parsertests überprüfen, ob ein String mit SetlX Programmcode den erwarteten Syntaxbaum zurückliefert. Die Parserfunktion wird mit dem String aufgerufen und das erwartete Ergebnis aus den Syntaxklassen zusammengebaut.

```js
// very simple parser test
describe('syntaxtree/Break', () => {
  it('should parse break statements', () => {
    parser('break;').should.eql(
      InitBlock([
        Break(),
      ])
    );
  });
});
```

Im Gegensatz dazu rufen Transpilertests die Transpilerfunktion mit einem solchen von Hand generiertem Syntaxbaum auf und überprüfen, ob das Ergebnis der erwarteten Ausgabe entspricht. Außerdem kann überprüft werden, ob der Transpiler wie erwartet mit den eingesetzten Plugins interagiert.

```js
describe('transpilers/identifier', () => {
  it('should transpile to the provided name correctly', () => {
    const name = 'myid';
    createTranspiler()(Identifier(name)).should.be.exactly(name);
  });

  it('should register the identifier name to the scope plugin', () => {
    const scopePlugin = { register: sinon.spy() };
    const stdLibPlugin = { isStd: sinon.stub().returns(false) };
    const transpile = createTranspiler({ scopePlugin, stdLibPlugin });
    transpile(Identifier('myid'));

    sinon.assert.calledOnce(scopePlugin.register);
    sinon.assert.calledWith(scopePlugin.register, 'myid');

    sinon.assert.calledOnce(stdLibPlugin.isStd);
    sinon.assert.calledWith(stdLibPlugin.isStd, 'myid');
  });
});
```

## 5.2 SetlX.js Library

Die SetlX.js Library ist in ES2015 geschrieben und wird, wie der Transpiler für die Verwendung im Browser und Node.js zuerst von Babel transpiliert. Von der Bibliothek werden Funktionen bereitgestellt, die ein in JavaScript übersetztes SetlX Programm brauchen könnte.

### 5.2.1 Struktur

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

### 5.2.2 Utility Funktionen

Utility Funktionen übernehmen Aufgaben, die von anderen Funktionen verwendet werden. Die Funktion `typify` ist besonders wichtig für die Umsetzung der SetlX Typen in JavaScript. Mit Hilfe von `typify` wird der SetlX Typ einer Variable ermittelt. Die meisten Funktionen in der Bibliothek sind überladen und müssen deshalb zuerst den Typ ihrer Übergabeparameter feststellen.

```js
import { Set, List } from 'immutable';

export const STRING = 'string';
export const NUMBER = 'number';
export const LIST = 'list';
export const SET = 'set';
export const BOOLEAN = 'boolean';

/**
 * To represent SetlX types in Javascript the transpiler created different
 * Javascript types. To make it easier for other functions to recognise
 * their input parameters types this function answeres with the type.
 */
const typeMap = { string: STRING, number: NUMBER, boolean: BOOLEAN };

export default function typify(value) {
  if (List.isList(value)) {
    return LIST;
  } else if (Set.isSet(value)) {
    return SET;
  }
  return typeMap[typeof value];
}
```

## 5.3 SetlX.js CLI

Das SetlX.js Command Line Interface wurde speziell für Node.js entwickelt und ist in ES5 geschrieben. Es läuft so nativ auf V8, der Virtuellen Maschiene in Node.js. SetlX.js bildet einen schmalen Wrapper um den Transpiler und stellt Funktionen und Optionen über Flags bereit. SetlX.js CLI verwendet die JavaScript Library [Commander.js](https://github.com/tj/commander.js) um die Übergabeparameter zu parsen.

Zum Transpilieren wird der SetlX.js Transpiler aufgerufen. Dateisystemoperationen werden mit den standardmäßigen Node.js Paketen `path` und `fs` umgesetzt. Für das Ausführen des Ergebniscodes wird ein einfacher `eval` Aufruf verwendet.
