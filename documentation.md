# 6 Dokumentation

## 6.1 Setup

Für die Verwendung von SetlX.js sollte das folgende empfohlene Setup verwendet werden. SetlX.js wird auf Node.js ausgeführt. Für die Verwendung von SetlX.js muss also Node.js installiert sein.

Danach kann SetlX.js mit NPM in der Konsole installiert werden.

```
npm install -g setlxjs-cli
```

Das `setlxjs` Kommando sollte nun vefügbar sein. Eine Überprüfung kann mit dem Aufruf der Hilfe gemacht werden.

```
setlxjs --help
```

## 6.2 Transpilieren von Dateien

In der Regel sollten Dateien zuerst kompiliert werden und dann von Node.js ausgeführt werden. Das` transpile` Kommando wird mit dem Pfad der zu kompilierenden Datei aufgerufen

```bash
# In file.stlx; Out file.js
setlxjs transpile file.stlx

# In file.stlx; Out transpiled.js
setlxjs transpile -o transpiled.js file.stlx

# Takes all files in src/*.stlx and converts them to src/*.js
setlxjs transpile src/

# Takes all files in src/*stlx and converts them to bin/*.js
setlxjs transpile -o bin/ src/
```

## 6.3 Ausführen von transpilierten JavaScript Dateien

JavaScript Dateien können direkt von Node.js ausgeführt werden. Dafür muss alledings die SetlX.js Library im Ordner der Datei oder einem übergeordnetem Ordner installiert sein. Um `setlxjs-lib` lokal zu installieren sollte NPM verwendet werden.

```
npm install setlx-lib
```

## 6.4 Ausführen von SetlX Dateien

SetlX Dateien können auch direkt ausgeführt werden. Dafür wird der `run` Befehl verwendet.

```
setlxjs run file.stlx
```

## 6.5 Parseransicht für Statements

Der Parsebaum eines Ausdrucks kann mit Hilfe des `tree` Kommandos ausgegeben werden:

```
setlxjs tree "{1, 3} + {2, 3};"
```
