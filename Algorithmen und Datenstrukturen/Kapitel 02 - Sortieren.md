[[Kapitel 01 - Introduction|vorheriges Kapitel]]
# Das Sortierproblem
**Gegeben:** Eine Folge von Objekten mit Schlüsselwerten
**Gesucht:** Sortierung der Objekte (gemäß ausgewiesenen Schlüsselwerts)

## Schlüsselproblem
Schlüssel(werte) müssen nicht eindeutig sein, aber müssen *sortierbar* sein.

**Annahme im Folgenden:**
Es gibt eine totale Ordnung $\leq$ auf der Menge $M$ aller möglichen Schlüsselwerte.

### Totale Ordnung
Sei $M$ eine nicht leere Menge und $\leq\subseteq M\cdot M$ eine binäre Relation auf $M$.
Die Relation $\leq$ auf $M$ ist genau dann eine totale Ordnung, wenn gilt:
**Reflexivität:** $\forall x \in M:\space x\leq x$
**Transitivität:** $\forall x,y,z \in M:\space x \leq y \space\land\space y \leq z \space\Rightarrow\space x \leq z$
**Antisymmetrie:** $\forall x,y \in M:\space x\leq y \space\land\space y \leq x \space\Rightarrow\space x = y$
**Totalität:** $\forall x,y \in M: \space x \leq y \space \lor \space y \leq x$     (ohne Totalität: partielle Ordnung)

### Darstellung und Konventionen dieses Kapitels
- Eingabe der Objekte btw. Schlüsselwerte in Form eines Arrays $A$:
	``A = [53, 12, 17, 44, 33, 25, 17, 4, 76]``

- Lese/Schreibzugriffe per Index erfolgen in konstanter Zeit.
- ``y=A[2]`` weist $y$ den Wert 17 zu, ``A[4]=99`` überschreibt 33 mit 99.
- ``A.length`` gibt eine (fixe) Länge des Arrays zurück (hier 9)
- ``A[i..j]`` beschreibt ein Teil-Array vom Index $i$ bis $j$, mit $0\leq i \leq j \leq A.length-1$

```c++
insertionSort(A)
	FOR i=1 TO A.length-1 DO
		// insert A[i] in pre-sorted sequence A[0..i-1]
		key=A[i];
		j=i-1; // search for insertion point backwards
	
		WHILE j>=0 AND A[j]>key DO
			A[j+1]=A[j]; // move elements to right
			j=j-1;
		A[j+1]=key;
```

- $A$ ist ein Array/Liste
- $A[0..i-1]$ ist immer bereits sortiert
- Wert an der Stelle $A[i]$ wird dann im sortierten Bereich an der richtigen Stelle eingefügt. Dabei wird alles immer verschoben.
### Terminierung
Jede Ausführung der **WHILE**-Schleife erniedrigt $j < n$ in jeder Iteration um 1 und bricht ab, wenn $j < 0$.  -> terminiert also immer.

Die **FOR**-Schleife wird nur endlich oft durchlaufen, terminiert also auch.

### Korrektheit
Schleifeninvariante (der **FOR**-Schleife):
Bei jedem Eintritt für einen Zählerwert $i$ (und nach letzter Ausführung) entsprechen die aktuellen Einträge in ``A[0], ..., A[i-1]`` den sortierten ursprünglichen Eingabewerten ``a[0], ..., a[i-1]``.
Ferner gilt ``A[i] = a[i], ..., A[n-1] = a[n-1]``.

#### Beweis per Induktion
**Induktionsbasis** $i=1$:
Beim ersten Eintritt ist ``A[0] = a[0]`` und "sortiert". 
Ferner gilt noch ``A[1] = a[1], ..., A[n-1] = a[n-1]``.

**Induktionsschritt** von $i-1$ auf $i$:
Vor der $(i-1)$-ten Ausführung galt die Schleifeninvariante nach der Voraussetzung. Insbesondere war ``A[0], ..., A[i-2]`` die sortierte Version von ``a[0], ..., a[i-2]`` und ``A[i-1] = a[i-1], ..., A[n-1] = a[n-1].
Durch die **WHILE**-Schleife wurde ``A[i-1] = a[i-1]`` nach links einsortiert und größere Elemente von ``A[0], ..., A[i-2]`` um jeweils eine Position nach rechts verschoben. Elemente ``A[i], ..., A[n-1]`` wurden nicht geändert. 
Also gilt die Invariante auch für $i$.

Aus der Schleifeninvariante folgt für die letzte Ausführung (also quasi vor gedanklichem Eintritt der Schleife für $i=n$):
	``A[0], ..., A[n-1]`` ist die sortierte Version von ``a[0], ..., a[n-1]``
und somit ist am Ende das Array sortiert.

# Laufzeitanalysen: O-Notation
Wie viele Schritte macht ein Algorithmus in Abhängigkeit von der Eingabekomplexität?
- Man nimmt meist *Worst-Case* für alle Eingaben gleicher Komplexität
- (Worst-Case-)Laufzeit: $T(n) = \max{\{Anzahl Schritte\}}$ für eine Aufgabe
- Komplexität wird meist von einem Faktor dominiert, wie z.B. der Anzahl zu sortierenden Elemente $n$

