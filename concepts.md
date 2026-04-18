# Konzepte

## 1. Lisp-Grundlagen (Code as Data / Homoiconizität + Makros)
Clojure behandelt Code als Daten. Du kannst Code-Listen manipulieren und mit Ma
Mkros neue Sprachkonstrukte erzeugen.

**Beispiel: Einfaches Makro für eine „unless“-Bedingung (Gegenteil von if):**

```clojure
(defmacro unless [condition & body]
  `(if (not ~condition)
     (do ~@body)))

;; Verwendung:
(unless (= 1 2)
  (println "1 ist nicht gleich 2 – alles gut!")
  (println "Noch eine Anweisung"))
```
Das Makro wandelt den Code zur Compile-Zeit in ein normales if um. Du siehst: Der übergebene Code ist einfach eine Liste, die du transformieren kannst.

## 2. Funktionale Programmierung (Pure Functions + Higher-Order Functions)

Funktionen sind Werte, und wir bevorzugen pure Funktionen (keine Nebenwirkungen) sowie `map, filter, reduce`.

**Beispiel: Verarbeitung einer Liste von Personen**
```clojure
(def people [{:name "Anna" :age 28 :active true}
             {:name "Ben"  :age 35 :active false}
             {:name "Clara":age 22 :active true}])

;; Pure Funktion + Higher-Order
(defn adult-active? [person]
  (and (>= (:age person) 18) (:active person)))

(->> people
     (filter adult-active?)
     (map (fn [p] (str (:name p) " ist erwachsen und aktiv")))
     (clojure.string/join "\n")
     println)
```

Ausgabe:
```
Anna ist erwachsen und aktiv
Clara ist erwachsen und aktiv
```

## 3. Immutabilität und persistente Datenstrukturen
Alle Standard-Datenstrukturen sind unveränderlich. „Änderungen“ erzeugen eine neue Version, die alte bleibt erhalten (strukturelles Teilen = effizient).

**Beispiel:**
```clojure
(def original {:name "Max" :punkte 100 :items ["Schwert" "Schild"]})

;; "Änderung" erzeugt neue Map + neuen Vector
(def updated (-> original
                 (update :punkte + 50)
                 (update :items conj "Trank")))

(println "Original unverändert:" original)
(println "Neue Version:" updated)
```
Ausgabe zeigt: `original` bleibt exakt gleich – kein Mutieren!
Das funktioniert extrem effizient dank Persistent Data Structures (Hash Array Mapped Tries für Maps/Sets, Tries für Vectors).

##4. Zustandsmanagement und Concurrency (Atoms, Refs, Agents)
Statt globaler mutierbarer Variablen nutzen wir Referenzen auf unveränderliche Werte.
**Beispiel mit Atom (einfachster, synchroner, atomarer Zustand – am häufigsten verwendet):**

```clojure
(def counter (atom 0))   ; Identität mit aktuellem Wert 0

;; Thread-sicher erhöhen
(swap! counter inc)      ; → 1
(swap! counter + 10)     ; → 11

;; Mit komplexer Funktion
(swap! counter (fn [alt] (* alt 2)))   ; → 22

(println "Aktueller Wert:" @counter)   ; @ ist Kurzform für deref
```
**Beispiel mit Refs + STM (für koordinierte Änderungen mehrerer Werte):
Clojure**

```clojure
(def konto1 (ref 1000))
(def konto2 (ref 500))

(defn ueberweisen [von nach betrag]
  (dosync
    (alter von - betrag)
    (alter nach + betrag)))

;; Gleichzeitig in mehreren Threads sicher:
(ueberweisen konto1 konto2 300)

(println "Konto1:" @konto1)   ; 700
(println "Konto2:" @konto2)   ; 800
````
Die `dosync`-Transaktion sorgt dafür, dass entweder beide Änderungen passieren oder keine (atomar und konsistent).