# 7 Diskussion

## 7.1 Bewertung des Ergebnisses

Mit dieser Arbeit ist ein solider Transpiler für SetlX entstanden. SetlX Programme lassen sich nun auf Node.js ausführen. Mit den zur Verfügung gestellten Paketen ist die Installation von SetlX.js besonders einfach, wenn Node.js bereits auf der Maschine vorhanden ist. Mit NPM lassen sich die Pakete mit nur wenigen Zeilen in der Konsole einrichten.

Durch die Strukturierung der Tests und eine Architektur, die speziell für Testbarkeit entworfen wurde, können die Pakete nicht nur einfach modular erweitert werden, sondern auch gleich getestet werden.

In der ersten Version kommt jedoch die definierte Untermenge von SetlX schnell an seine Grenzen. Nur wenige Scripte aus Professor Stroetmanns Logikvorlesung können direkt ausgeführt werden. Viele Programme müssten umformuliert werden, damit sie mit der geringen SetlX Version auskommen. Dadurch wird SetlX.js als Konsolenanwendung wenig attraktiv. Wodurch SetlX.js Vorteile gegenüber der Standardimplementierung erhalten kann, wird im folgenden Kapitel näher erläutert.

## 7.2 Ausblick

SetlX.js setzt viele Grundfunktionen von SetlX um. Für eine Unterstützung sämtlicher Funktionen muss allerdings der Transpiler noch stark erweitert werden. Mit der modularen Architektur ist eine Umsetzung allerdings keine Schwierigkeit. Einzelne Features können durch das Hinzufügen weniger Dateien und Zeilen Code schnell realisiert werden. Auch die SetlX.js Library macht es möglich Feutures umzusetzen, die von JavaScript nicht standardmäßig angeboten werden.

Diese Arbeit bietet nur Umgebungen für die Ausführung auf Node.js an. Allerdings könnte der generierte Code auch in einem Browser ausgeführt werden. Dafür müssten allerdings zuerst die Dateien mit einem Modulebundler wie Webpack oder Browserify zu einer einzigen Datei zusammengefügt werden. Eine Ausführung im Browser ist in diesem Sinne jedoch nicht von großem Nutzen. Interessant ist es allerdings den SetlX Code im Browser zu übersetzen und ihn dann auszuführen, etwa in einem Browser REPL. Neben einer entsprechenden Website müssten allerdings auch noch Veränderungen wie spezielle Plugins für den Browser erstellt werden.

Die Anzahl an Bibliotheken und Pakete für JavaScript hat in den letzten Jahren stark zugenommen. Damit diese aber auch innerhalb von SetlX sinnvoll nutzbar werden können, reicht in dieser Arbeit vorgestellte Untermenge der SetlX-Syntax nicht aus. JavaScript ist vor allem eine objektorientierte Programmiersprache. Da SetlX.js in seiner ersten Version keine Objekte unterstützt können die wenigsten Pakete direkt benutzt werden. Allerdings könnten Schnittstellen in JavaScript formuliert werden, die eine rein prozedurale bzw. funktionale API zur Verfügung stellen.
