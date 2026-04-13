
# Clojure Cheat Sheet

Ein praktischer Überblick über die wichtigsten Features von Clojure mit Beispielen zum Ausprobieren.

---

## 1. Grundaufbau & Syntax

### S-Expressions (Symbolic Expressions)
Clojure basiert auf S-Expressions - alles ist eine Liste in Klammern.

```clojure
; (function arg1 arg2 ...)
(+ 1 2 3)           ; => 6
(* 4 5)             ; => 20
(str "Hallo " "Welt")  ; => "Hallo Welt"
```

### Kommentare
```clojure
; Single-line Kommentar

#_
Diese Zeile wird ignoriert
#_
```

### Variablen (Def)
```clojure
(def x 42)
(def name "Alice")
(def zahlen [1 2 3 4 5])

x                   ; => 42
```

---

## 2. Datentypen

### Zahlen
```clojure
42                  ; Integer
3.14                ; Float
22/7                ; Rational (Bruch)
1E3                 ; Wissenschaftliche Notation => 1000

; Arithmetic
(+ 10 5)            ; => 15
(- 10 5)            ; => 5
(* 3 4)             ; => 12
(/ 10 2)            ; => 5
(mod 10 3)          ; => 1
(quot 10 3)         ; => 3
```

### Strings
```clojure
"Hallo"                    ; String
(str "Hello" " " "World")  ; Verkettung
(count "Hallo")            ; => 5
(uppercase "hello")        ; => nicht vorhanden, aber:
(clojure.string/upper-case "hello")  ; => "HELLO"
```

### Keywords
```clojure
:name               ; Keyword (oft als Keys in Maps verwendet)
:active?            ; Keywords können ? oder ! enthalten
:my-key             ; Hyphens sind erlaubt
```

### Boolesche Werte
```clojure
true
false
nil                 ; Falsy value

(= 5 5)             ; => true
(> 10 5)            ; => true
(< 10 5)            ; => false
```

### Vektoren (Arrays-ähnlich)
```clojure
[1 2 3 4 5]                      ; Vektor
[:a :b :c]                       ; Keywords
[1 "zwei" :three 4.0]            ; Gemischte Typen

(vector 1 2 3)                   ; => [1 2 3]
(count [1 2 3])                  ; => 3
(first [1 2 3])                  ; => 1
(rest [1 2 3])                   ; => (2 3)
(nth [1 2 3] 1)                  ; => 2 (Index-Zugriff)
(conj [1 2] 3)                   ; => [1 2 3] (Element hinzufügen)
```

### Listen
```clojure
'(1 2 3)                    ; Liste (quoted)
(list 1 2 3)                ; => (1 2 3)
(first '(1 2 3))            ; => 1
(rest '(1 2 3))             ; => (2 3)
(cons 0 '(1 2 3))           ; => (0 1 2 3)
```

### Maps (Dictionaries)
```clojure
{:name "Alice" :age 30}              ; Map
{:a 1 :b 2 :c 3}                     ; Keyword Keys

(def person {:name "Bob" :age 25})
(:name person)                       ; => "Bob" (Zugriff mit Keyword)
(:age person)                        ; => 25
(get person :name)                   ; => "Bob"
(keys person)                        ; => (:name :age)
(vals person)                        ; => ("Bob" 25)
(assoc person :city "Berlin")        ; => {:name "Bob" :age 25 :city "Berlin"}
(dissoc person :age)                 ; => {:name "Bob"}
(merge person {:city "Berlin"})      ; => {:name "Bob" :age 25 :city "Berlin"}
```

### Sets (Mengen)
```clojure
#{1 2 3 4}                   ; Set
(set [1 2 3 2 1])            ; => #{1 2 3} (Duplikate entfernt)
(conj #{1 2} 3)              ; => #{1 2 3}
(contains? #{1 2 3} 2)       ; => true
(union #{1 2} #{2 3})        ; Union braucht clojure.set
```

---

## 3. Bedingungen

### if
```clojure
(if true
  "Ja"
  "Nein")                     ; => "Ja"

(if false
  "Ja"
  "Nein")                     ; => "Nein"

(if (> 10 5)
  "10 ist größer"
  "10 ist nicht größer")      ; => "10 ist größer"
```

### when (wenn ohne else)
```clojure
(when (> 10 5)
  (println "10 > 5")
  "Wert ist größer")          ; Gibt "Wert ist größer" aus

(when false
  "wird nicht ausgeführt")    ; => nil
```

### case
```clojure
(def tag :montag)

(case tag
  :montag "Mo"
  :dienstag "Di"
  :mittwoch "Mi"
  "Unbekannt")                ; => "Mo"
```

### cond
```clojure
(def alter 25)

(cond
  (< alter 18) "Jugendlich"
  (< alter 65) "Berufstätig"
  :else "Rentner")            ; => "Berufstätig"
```

### Vergleichsoperatoren
```clojure
(= 5 5)                  ; => true (Gleichheit)
(not= 5 3)               ; => true
(< 3 5)                  ; => true
(<= 5 5)                 ; => true
(> 5 3)                  ; => true
(>= 5 5)                 ; => true
(and true true false)    ; => false
(or false false true)    ; => true
(not true)               ; => false
```

---

## 4. Schleifen und Wiederholungen

### loop/recur (rekursive Schleife)
```clojure
(loop [i 0]
  (println i)
  (if (< i 3)
    (recur (inc i))))        ; Gibt 0, 1, 2 aus
```

### doseq (Iteration mit Effekten)
```clojure
(doseq [x [1 2 3]]
  (println x))               ; Gibt 1, 2, 3 aus

(doseq [x [1 2] y [:a :b]]
  (println x y))             ; Nested: 1 :a, 1 :b, 2 :a, 2 :b
```

### map (Transformation)
```clojure
(map inc [1 2 3])                    ; => (2 3 4)
(map * [1 2 3] [10 20 30])           ; => (10 40 90)
(map (fn [x] (* x 2)) [1 2 3])       ; => (2 4 6)
```

### filter
```clojure
(filter even? [1 2 3 4 5])           ; => (2 4)
(filter (fn [x] (> x 3)) [1 2 3 4 5]) ; => (4 5)
```

### reduce
```clojure
(reduce + [1 2 3 4])                 ; => 10
(reduce (fn [acc x] (+ acc x)) 0 [1 2 3 4]) ; => 10
```

### for (Comprehension)
```clojure
(for [x [1 2 3] y [:a :b]]
  [x y])                             ; => ([1 :a] [1 :b] [2 :a] [2 :b])

(for [x (range 5) :when (even? x)]
  x)                                 ; => (0 2 4)
```

### repeatedly
```clojure
(take 3 (repeatedly #(rand-int 10)))  ; 3 zufällige Zahlen
```

---

## 5. Funktionen

### Funktionen definieren (defn)
```clojure
(defn greet [name]
  (str "Hallo, " name "!"))

(greet "Alice")              ; => "Hallo, Alice!"
```

### Mehrere Parameter
```clojure
(defn add [a b]
  (+ a b))

(add 3 4)                    ; => 7
```

### Optionale Parameter & Docstrings
```clojure
(defn power
  "Berechnet a hoch b"
  [a b]
  (Math/pow a b))

(power 2 3)                  ; => 8.0
```

### Variadic functions (beliebige Anzahl von Argumenten)
```clojure
(defn sum [& numbers]
  (reduce + numbers))

(sum 1 2 3 4 5)              ; => 15
```

### Keyword arguments
```clojure
(defn config [& {:keys [host port]}]
  {:host host :port port})

(config :host "localhost" :port 8080)  ; => {:host "localhost" :port 8080}
```

### Anonyme Funktionen
```clojure
(fn [x] (* x 2))             ; Anonyme Funktion
#(* % 2)                     ; Shorthand: % ist das erste Argument
#(* %1 %2)                   ; %1, %2 für mehrere Argumente

(map #(* % 2) [1 2 3])       ; => (2 4 6)
```

### Higher-order functions
```clojure
(defn twice [f x]
  (f (f x)))

(twice inc 5)                ; => 7
(twice (fn [x] (* x 2)) 3)   ; => 12
```

### Default-Werte
```clojure
(defn greet-with-default
  ([name] (greet-with-default name "Guten Tag"))
  ([name greeting] (str greeting ", " name)))

(greet-with-default "Bob")              ; => "Guten Tag, Bob"
(greet-with-default "Alice" "Hallo")    ; => "Hallo, Alice"
```

---

## 6. Namespaces

### Namespace definieren
```clojure
(ns my-app.core)                    ; Namespace am Anfang der Datei

(def version "1.0")
(defn start [] (println "Starting..."))
```

### In anderen Namespaces
```clojure
(ns my-app.main
  (:require [my-app.core :as core]))

core/version                        ; => "1.0"
(core/start)                        ; Ruft die Funktion auf
```

### Imports
```clojure
(ns my-app.utils
  (:import [java.io File]
           [java.util Date]))

(def f (File. "/tmp/test.txt"))
(def d (Date.))
```

### Aliasing
```clojure
(ns my-app.main
  (:require [clojure.string :as str]
            [clojure.math :as math]))

(str/upper-case "hello")             ; => "HELLO"
(math/abs -42)                       ; => 42
```

---

## 7. Immutability & Atom/Refs

### Immutable Datenstrukturen
```clojure
(def original [1 2 3])
(def modified (conj original 4))

original                            ; => [1 2 3] (unverändert!)
modified                            ; => [1 2 3 4]
```

### Atoms (Mutable Reference)
```clojure
(def counter (atom 0))

@counter                            ; => 0 (Dereferencing mit @)
(swap! counter inc)                 ; => 1 (in-place Änderung)
@counter                            ; => 1
(reset! counter 10)                 ; => 10
@counter                            ; => 10
```

### Atom Updates
```clojure
(def state (atom {:count 0 :name "Test"}))

(swap! state assoc :count 5)        ; => {:count 5 :name "Test"}
(swap! state update :count inc)     ; => {:count 6 :name "Test"}
```

---

## 8. Nützliche Funktionen

### Sequenzen
```clojure
(range 5)                           ; => (0 1 2 3 4)
(range 1 5)                         ; => (1 2 3 4)
(range 0 10 2)                      ; => (0 2 4 6 8) (Step 2)
(repeat 3 "x")                      ; => ("x" "x" "x")
(take 3 (range))                    ; => (0 1 2) (aus unendlicher Sequenz)
(drop 2 [1 2 3 4 5])                ; => (3 4 5)
(partition 2 [1 2 3 4 5 6])         ; => ((1 2) (3 4) (5 6))
```

### String-Operationen
```clojure
(require '[clojure.string :as str])

(str/upper-case "hello")            ; => "HELLO"
(str/lower-case "HELLO")            ; => "hello"
(str/capitalize "hello")            ; => "Hello"
(str/split "a,b,c" #",")            ; => ["a" "b" "c"]
(str/join "-" ["a" "b" "c"])        ; => "a-b-c"
(str/trim "  hello  ")              ; => "hello"
(str/replace "hello" "l" "L")       ; => "heLLo"
```

### Typchecks
```clojure
(number? 42)                        ; => true
(string? "hello")                   ; => true
(vector? [1 2 3])                   ; => true
(map? {:a 1})                       ; => true
(nil? nil)                          ; => true
(boolean? true)                     ; => true
```

### Nützliche Prädikte
```clojure
(even? 4)                           ; => true
(odd? 3)                            ; => true
(zero? 0)                           ; => true
(pos? 5)                            ; => true
(neg? -5)                           ; => true
```

---

## 9. Threading Macros (Vereinfachte Syntax)

### Thread-first (->)
```clojure
(-> 5
    (+ 3)                          ; 5 + 3 = 8
    (* 2)                          ; 8 * 2 = 16
    (- 1))                         ; 16 - 1 = 15

; Equivalent to: (- (* (+ 5 3) 2) 1)
```

### Thread-last (->>)
```clojure
(->> [1 2 3 4 5]
     (filter even?)                ; [2 4]
     (map #(* % 2))                ; (4 8)
     (reduce +))                   ; 12
```

---

## 10. Exception Handling

### try/catch
```clojure
(try
  (/ 10 0)
  (catch ArithmeticException e
    (println "Error:" (.getMessage e))))  ; => Error: Division by zero

(try
  (/ 10 0)
  (catch Exception e
    "Ein Fehler ist aufgetreten")
  (finally
    (println "Finally Block")))
```

---

## 11. Praktische Beispiele zum Ausprobieren

### Fibonacci-Sequenz
```clojure
(defn fib [n]
  (cond
    (= n 0) 0
    (= n 1) 1
    :else (+ (fib (- n 2)) (fib (- n 1)))))

(map fib (range 10))                ; => (0 1 1 2 3 5 8 13 21 34)
```

### Wörterbuch/Schleife
```clojure
(defn word-frequencies [text]
  (frequencies (clojure.string/split text #"\s+")))

(word-frequencies "hello world hello clojure world")
; => {"hello" 2 "world" 2 "clojure" 1}
```

### Faktorisierung
```clojure
(defn factors [n]
  (filter #(zero? (mod n %)) (range 2 (inc (/ n 2)))))

(factors 12)                        ; => (2 3 4 6)
```

### Nested Map Zugriff
```clojure
(def user
  {:name "Alice"
   :address {:city "Berlin" :zip "10115"}})

(get-in user [:address :city])      ; => "Berlin"
(assoc-in user [:address :country] "Germany")
; => {:name "Alice" :address {:city "Berlin" :zip "10115" :country "Germany"}}
```

### Bedingte Datenbearbeitung
```clojure
(defn process-users [users min-age]
  (->> users
       (filter #(>= (:age %) min-age))
       (map :name)
       (sort)))

(process-users
  [{:name "Alice" :age 25}
   {:name "Bob" :age 17}
   {:name "Charlie" :age 30}]
  18)
; => ("Alice" "Charlie")
```

---

## 12. Cheat Sheet Quick Reference

| Feature | Syntax | Beispiel |
|---------|--------|----------|
| Vektor | `[...]` | `[1 2 3]` |
| Map | `{:key value}` | `{:a 1 :b 2}` |
| Set | `#{...}` | `#{1 2 3}` |
| Liste | `'(...)` | `'(1 2 3)` |
| Funktion | `(defn name [args] ...)` | `(defn add [a b] (+ a b))` |
| If | `(if cond true-val false-val)` |  |
| Loop | `(loop [x 0] ...)` |  |
| Map | `(map f coll)` | `(map inc [1 2 3])` |
| Filter | `(filter pred coll)` | `(filter even? [1 2 3])` |
| Reduce | `(reduce f coll)` | `(reduce + [1 2 3])` |
| Atom | `(atom value)` | `(atom 0)` |
| Dereference | `@atom` | `@counter` |

---

## 13. Projekt anlegen mit deps.edn

`deps.edn` ist das offizielle Konfigurationsformat der Clojure CLI. Es ermöglicht das Verwalten von Abhängigkeiten und das Definieren von Aliases, ohne ein externes Build-Tool wie Leiningen zu benötigen.

### Projektstruktur

```
mein-projekt/
├── deps.edn          ; Abhängigkeiten & Konfiguration
└── src/
    └── mein_projekt/
        └── core.clj  ; Hauptdatei (Unterstriche statt Hyphens im Verzeichnisnamen)
```

### Minimales deps.edn

```edn
{:paths ["src"]
 :deps {org.clojure/clojure {:mvn/version "1.12.0"}}}
```

### Abhängigkeiten hinzufügen

```edn
{:paths ["src" "resources"]
 :deps {org.clojure/clojure    {:mvn/version "1.12.0"}
        metosin/malli          {:mvn/version "0.16.4"}
        ring/ring-core         {:mvn/version "1.12.2"}}}
```

### Aliases definieren

```edn
{:paths ["src"]
 :deps {org.clojure/clojure {:mvn/version "1.12.0"}}
 :aliases
 {:dev  {:extra-paths ["dev"]
         :extra-deps  {nrepl/nrepl {:mvn/version "1.3.1"}}}
  :test {:extra-paths ["test"]
         :extra-deps  {io.github.cognitect-labs/test-runner
                       {:git/tag "v0.5.1" :git/sha "dfb30dd"}}}}}
```

### REPL starten

```bash
# Einfaches REPL
clojure

# REPL mit Dev-Alias
clojure -M:dev

# REPL mit nREPL-Server (für Editor-Integration)
clojure -M:dev -m nrepl.cmdline --interactive
```

### Hauptdatei (src/mein_projekt/core.clj)

```clojure
(ns mein-projekt.core)

(defn -main [& args]
  (println "Hallo, Welt!"))
```

### Programm ausführen

```edn
; In deps.edn einen run-Alias hinzufügen:
:aliases
{:run {:main-opts ["-m" "mein-projekt.core"]}}
```

```bash
# Programm starten
clojure -M:run

# Direkt ohne Alias
clojure -M -m mein-projekt.core
```

### Git-Abhängigkeiten (ohne Maven)

```edn
{:deps {io.github.user/mein-lib
        {:git/url "https://github.com/user/mein-lib"
         :git/sha "abc1234def"}}}
```

### Lokale Abhängigkeiten

```edn
{:deps {mein/lokales-projekt {:local/root "../lokales-projekt"}}}
```

---

## Ressourcen zum Lernen

- **REPL ausprobieren**: Clojure REPL lokal starten: `clojure`
- **Try Clojure**: https://tryclojure.org/
- **OneCompiler/clojure**: https://onecompiler.com/clojure
- **Online REPL**: https://repl.it/languages/clojure
- **Offizielle Docs**: https://clojure.org/
- **Cheatsheets**: https://clojure.org/cheatsheet
- **Community**: r/Clojure auf Reddit
