---
title: Protokoll vom 31.05.2017
permalink: /protokoll/31.05.07/
---

# Resourcenmanagement: Engine

## Klassen

### Core

Erzeugt zentrale Teile der Engine: __ResourceManager__ und __Simulation__

### Simulation

1. Lädt benötigte _Resourcen_ in den __ResourceManager__
2. Erzeugt mittels __ResourceManager__ _Objekte der Welt_
3. Erstellt _Behaviours_, die das Verhalten der _Objekte der Welt_ bestimmen

### ResourceManager

Enthält alle _Texturen_ und _Hitboxen_

# Physikalische Eigenschaften des Flugzeugs

## The 4 allmighty forces / Die 4 allmächtigen Kräfte

### Mass / Masse

* Nach unten zeigende Kraft
* Abhängig von der __Gesamtmasse__ des Flugzeuges
* Unabhängig von der Flugrichtung

### Thrust / Schub

* Antreibende Kraft der Triebwerke
* Entgegengesetzt der __Drag-/Luftwiederstandsrichtung__

### Drag

* Bremsende Kraft des Flugzeugs
* Abhängig der __Velocity/Geschwindigkeit__ des Flugzeugs
* Entgegengesetzt der __Thrust-/Schubrichtung__

### Lift

* Bestimmt Auftrieb des Flugzeugs
* Ist im rechten Winkel zu den Flügeln
* Abhängig von der __Velocity/Geschwindigkeit__ des Flugzeugs
