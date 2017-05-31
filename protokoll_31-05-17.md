---
title: Protokoll vom 31.05.2017
permalink: /protokoll/31.05.07/
---

Inhalt:
  1. Resourcenmanagement / Anwendungsmodell
     1.1. Problem
	 1.2. Ansatz / Entwurf
  2. Physikalische Eigenschaften
     2.1. Die vier fundamentalen Kräfte des Flugzeugs
	 2.2. Debatte: 3 oder 4 Kräfte
	 2.3. Ergebnis

# Resourcenmanagement / Anwendungsmodell

## Problem

Die Idee bis jetzt war, für jedes erstellte Objekt in der Welt alle benötigten Resourcen zu laden.
Der "Core" der Anwendung hält Referenzen auf die Objekte und behinhaltet sämtliche Programmlogik.
Das würde minimalen Programmieraufwand bedeuten, doch beim Diskutieren einer praktischen Umsetzung
sind uns Nachteile und Lösungsansätze aufgefallen:

1. Falls ein Objekt mehrmals in die Welt gesetzt wird, werden die Resourcen ineffizient verwaltet,
   die Texturen und Formen des Objekts sind mehrfach abgespeichert.
   
   __Lösung:__ Texturen nur einmal abspeichern und den Objekten zur Verfügung stellen.
   
2. Sehr viel doppelter Code würde bei der Implementierung anfallen, da jedes Objekt einzeln
   referenziert wird.
   
   __Lösung:__ Die Struktur der Resourcen/Objekte der Welt abstrahieren und in Listen halten.
   
3. Der Kern/Core wird einfach viel zu groß, im Hauptmenü hätte man schon Zugriff auf die Flugzeug
   Logik, was unnötig ist.
   
   __Lösung:__ Den Kern in Szenarien spalten.

## Ansatz / Entwurf

Wir haben versucht, diese drei Ansätze zusammenzufassen und haben folgende Abstraktionen gezogen:

* __Szenarien:__ _Momentaner Programmmodus._ Szenarien sind komplett unabhängig und es kann nur
   ein Szenario zur selben Zeit geladen und aktiv sein. Ein Szenario ist nach dem MVC-Pattern
   der _Controller_ der Anwendung.
   
   Beispiele für Szenarien: Hauptmenü, Simulation …
   
* __Resource:__ _Datenmodell, das ge-/entladen werden kann._ Das heißt, bevor eine Resource zur
   Verfügung steht, muss sie erst berechnet/von der Festplatte geladen werden. Das Szenario lädt
   beim Starten die benötigten Resourcen und gibt diese wieder frei beim Beenden.
   
   Beispiele für Resourcen: Flugzeug (Modell und Physik), Terrain, Himmel …
   
   Das Wichtige dabei ist, dass es von jeder Resource an sich nur eine Instanz geben kann.
   Das heißt z.B., dass das Modell des Flugzeugs nur 1x geladen ist.
 
* __(Resource) Parts:__ Um den Code übersichtlich zu halten, werden Resourcen in Teile
   aufgespaltet.
   
   Beispiele für Parts: Flügel des Flugzeugs, Räder des Flugzeugs, Form des Terrains,
   Textur des Himmels …
 
* __Object Instance:__ _Allgemeines Objekt, das durch eine Resource beschrieben ist._
   Es wird automatisch erstellt und enthält an sich keine Logik. Es kann mehrere Object Instances
   in der Anwendung geben. Jede Object Instance kann mehrere Behaviours beinhalten.
   
   Beispiel für eine Object Instance: "4 Flugzeuge in der Simulation, die alle auf dieselbe 
   Resource zeigen.".

* __Behaviour__ _Verhaltensweise einer Object Instance._ Da jede Resource nur eine Instanz haben
   kann, und Object Instances keine Logik enthalten, muss das Verhalten durch ein Behaviour-Objekt
   definiert werden. Das Behaviour wird dann in eine Object Instance eingesetzt.
   Dadurch kommt das Object Instance "zum Leben" und kann speziellen Code ausführen.
   
   Beispiele für Behaviours: Aerodynamik des Flugzeugs, Steuerung des Flugzeugs
   
   (Der entscheidende Vorteil von Behaviours ist, dass sie dynamisch eingesetzt werden können. 
   __Folgende Überlegung__: Es gibt mehrere Flugzeuge im Spiel. Nur eins davon soll gesteuert werden.
   Deswegen ist nur in diesem Flugzeug ein Benutzersteuerungs-Behaviour. Die anderen können z.B.
   Behaviours für Autopiloten haben. Das wäre mit Code in der Object Instance nur bedingt möglich,
   denn dieser würde in allen Instanzen geladen sein.)
   
   (Übrigens ist dieses Beispiel nicht Ziel des Projekts, sondern nur eine Erklärung für die
   Anwendung von Behaviours)

* __Engine:__ _Nichtspezifischer Code._ Alles, was nicht eine Resource, Part, Szenario oder Behaviour
   ist, gehört zur Engine.

  * __Core:__ _Primärer Controller._ Erzeugt zentrale Teile der Engine.
  
  * __ResourceManager:__ Enthält alle _Resourcen_.

# Physikalische Eigenschaften

Genau so wichtig wie die Struktur des Programms ist die Funktionsweise.
Wir haben uns in den letzten 30 Minuten überlegt, wie die Aerodynamik so zu vereinfachen ist, sodass
sie einfach anzuwenden ist.

## Die vier fundamentalen Kräfte des Flugzeugs

Fast jedes Lehrbuch vereinfacht die Aerodynamik in etwa so:

### Mass / Gewicht

* Schwerkraft
* Nur abhängig von der __Gesamtmasse__ des Flugzeuges

### Thrust / Schub

* Antreibende Kraft der Triebwerke
* Abhängig von Drehzahl des Motors (Luftdichte, Treibstoff)
* Zeigt in Richtung des Flugzeugs

### Drag / Luftwiderstand

* Bremsende Kraft des Flugzeugs
* Abhängig von der __Geschwindigkeit__ des Flugzeugs
* Abhängig von der __Querschnittsfläche__ des Flugzeugs aus der Sicht des Luftstroms
* Entgegengesetzt der __Geschwindigkeitsrichtung__

### Lift / Auftrieb

* Kraft, die das Flugzeug nach oben drückt
* Ist im rechten Winkel zu den Flügeln
* Abhängig vom Luftstrom an den Flügeln
* Faktor, der das Flugzeug zum Flugzeug macht

Anmerkung: Der Luftstrom ist das _entgegengesetzte Tempo_ des Flugzeugs. Das heißt es wird angenommen
das die Luft im Raum steht.

## Debatte: 3 oder 4 Kräfte

Gewicht und Schub sind relativ einfach zu implementieren. Ein Problem bilden jedoch der Luftwiderstand
und der Auftrieb.

* Die __Querschnittsfläche__ eines 3D-Modells in Echtzeit zu berechnen ist zu aufwändig für das
  gewünschte Ergebnis. (Die Lösung wäre "Pixel count using Orthografic Projection in OpenGL", also
  das Betrachten des Flugzeugs aus einer [Parallelperspektive](https://de.wikipedia.org/wiki/Parallelprojektion)
  unter Zählung der Fläche)
* Der __Auftrieb__ eines Flugzeugs hat viel mehr Faktoren als jede andere Kraft im Flugzeug:
  * Form des Flügels (["Airfoil"](https://de.wikipedia.org/wiki/Profil_(Strömungslehre)))
  * Luftstromeffekte: ([Strömungsabriss](https://de.wikipedia.org/wiki/Strömungsabriss), 
    Effektivität des Anstellwinkels / Angle of Attack)
* In speziellen Fällen wirken Widerstand und Auftrieb ähnlich oder wechselwirken (Beide werden ja auch
  durch Luft verursacht)
  
Ein Vorschlag war es, Drag und Lift in eine Form zusammenzufassen, in etwa "Aero" (extrem :P).
Jedes Teil hat dann z.B. drei Aero-Kurven für alle Luftstromrichtungen, oder bildlich dargestellt:
Eine bunte Kugel um das Teil, dass für jeden Winkel (also Punkt auf der Kugel) einen 
(Rot/Grün)Wert hat, die Winkel und Multiplikator entsprechen.
So könnte man die ganze vorher genannte Kritik schlagen.

Ein neuer Kritikpunkt war aber der Aufwand, der in dieses System reingeht. Wir haben eine Deadline
und deswegen muss man auch mal nachgeben und sich mit einer einfacheren Lösung zufrieden geben.

## Ergebnis

Das Modell, worauf wir uns geeinigt haben, ist eine Mischung zwischen der 3/4-Kräfte Situation und sieht
folgendermaßen aus:

* Jedes Teil, das nicht ein Flügel ist hat keinen Auftrieb.
* Flügel haben keinen direkten Luftwiderstand, sondern nur Auftrieb. Der Auftrieb ist immer im Lot auf dem Flügel,
  sodass bei hohen Anstellwinkeln (Unterschied Luftströmung/Flugzeugneigung) durch den Auftrieb auch ein
  realistischer Widerstand entsteht.
* Obwohl es auf den ersten Blick unlogisch erscheinen mag, haben die Flügel bei einem Anstellwinkel von
  0° einen Lift-Faktor von 0. Das heißt, dass bei perfekt geradem Flug das Flugzeug langsam nach unten sinkt
  (Fundamentale Schwerkraft zieht das Flugzeug nach unten). Durch die resultierende vertikale Geschwindigkeit
  ensteht automatisch ein Anstellwinkel, der das Flugzeug wieder stabilisiert. Es gibt eine Gegenkraft nach oben
  durch Lift. Das Ergebnis ist ein gerader Flug mit einem (sichtbaren) Anstellwinkel – wie man ihn von Jets bei
  Unterschall kennt.
  ![F16 vs. F35 – US Air Force](/assets/img/aoa.jpg)
  
  __Quelle: US Air Force (Facebook)__ Ein klassisches Beispiel ist dieses Bild. Alle Flugzeuge fliegen auf
  gleicher Höhe, die F16 (weiß-rot) brauchen jedoch einen niedrigeren Anstellwinkel als die F35 (tarnkappenfarben).
  Unser Flugzeug ist also eher mit einem modernen F35 zu vergleichen!
