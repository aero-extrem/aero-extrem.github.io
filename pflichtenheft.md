---
title: Pflichtenheft
permalink: /pflichtenheft/
---

## Inhaltsverzeichnis

1. [Zielbestimmungen][1.]
    1. [Musskriterien][1.1.]
    2. [Wunschkriterien][1.2.]
    3. [Abgrenzungskriterien][1.3.]
2. [Produkteinsatz][2.]
3. [Produktumgebung][3.]
    1. [Software][3.1.]
    2. [Hardware][3.2.]
    3. [Entwicklungsumgebung][3.3.]
4. [Produktfunktionen][4.]
    1. [Physikbasierte Simulation][4.1.]
    2. [Datenbankanbindung][4.2.]
5. [Benutzeroberfläche][5.]
    1. [Hauptmenü][5.1.]
    2. [Flugsimulation][5.2.]
    3. [Flugsimulation (Menü)][5.3.]

    [1.]: #1-zielbestimmungen
    [1.1.]: #11-musskriterien
    [1.2.]: #12-wunschkriterien
    [1.3.]: #13-abgrenzungskriterien
    [2.]: #2-produkteinsatz
    [3.]: #3-produktumgebung
    [3.1.]: #31-software
    [3.2.]: #32-hardware
    [3.3.]: #33-entwicklungsumgebung
    [4.]: #4-produktfunktionen
    [4.1.]: #41-physikbasierte-simulation
    [4.2.]: #42-datenbankanbindung
    [5.]: #5-benutzeroberfläche
    [5.1.]: #51-hauptmenü
    [5.2.]: #52-flugsimulation
    [5.3.]: #53-flugsimulation-menü

## 1. Zielbestimmungen

Das Ziel des Projektes ist es, ein Flugzeug zu simulieren, dass vom Benutzer
gesteuert werden kann.

### 1.1. Musskriterien
* Realitätsähnliches Erlebnis
  
   - Das gesteuerte Flugzeug muss sich annähernd realistisch verhalten.
     (Siehe 4.1.)
   - Kollisionen von Objekten innerhalb der Spielewelt dürfen nicht ignoriert
     werden.

* SQL-Anbindung

  Es wird eine SQL-kompatible Datenbank verwendet, um Benutzereinstellungen und
  Flugrouten zu speichern. (Siehe 4.2.)

### 1.2. Wunschkriterien
* Realitätsgetreue 3D-Modelle
  
  Die Flugzeuge sollten in etwa echten Transportflugzeugen entsprechen.

* Effekte
  
  Grafisch-visuell ansprechende Aspekte sollten gegeben sein, um die
  Benutzererfahrung zu verbessern.

* Ausfälle

  Starke Kollisionen sollten nicht ignoriert werden, sondern Ausfälle wie
  Triebwerkschäden oder das Abreißen von Teilen des Flugzeugs verursachen.

### 1.3. Abgrenzungskriterien
* Realititäsgrad

  Eine Annäherung an die Realität ist vorhanden, aber auf komplexe Phänomene
  der Luftfahrt wird verzichtet (zum Beispiel (transsonische) Strömungen, und
  Temperatur).

* Grenzen der Welt

  Die virtuelle Welt ist flach und ihrer Atmosphäre kann nicht entkommen
  werden. Daher sind keine Manöver wie Orbit oder Wiedereintritt möglich.

* Umwelteinflüsse

  Es gibt keine Faktoren wie Wind und Wetter, die das Flugzeug beeinflussen
  würden.

## 2. Produkteinsatz

_Aero EXTREM_ soll spielerisch einen Einblick in die Luftfahrtphysik gewähren
und gängige Missverständnisse aufklären. Das Programm ist für
Spieleinteressierte gut geeignet.

## 3. Produktumgebung

### 3.1. Software
Auf folgender Software basiert _Aero EXTREM_.

* Runtime: [Java SE 1.7+](https://www.java.com/de/) 

  Das Produkt läuft auf der Java Virtual Machine. Sie wird nicht mit dem
  Projekt geliefert.

* Grafik: [LWJGL](https://www.lwjgl.org)

  LWJGL ermöglicht Zugriff auf OpenGL ES-Funktionen in der Java Umgebung. Es
  wird durch libgdx gesteuert.

* Physik: [Bullet](http://bulletphysics.org)

  Ein Echtzeit-Physiksimulator, in C++ verfasst. Zugriff darauf wird durch
  libgdx ermöglicht.

* „Engine“: [libgdx](https://libgdx.badlogicgames.com)

  Ein plattformübergreifendes Framework für Spieleentwicklung, das LWJGL und
  Bullet zusammenfasst und Resourcen verwaltet.

* Datenbank: [SQLite](https://sqlite.org)

  Eine lokale SQL-Datenbank mit geringen Resourcenanforderungen.
  Eine Java JDBC-Anbindung geschieht durch
  [xerial](https://github.com/xerial/sqlite-jdbc).

### 3.2. Hardware
Die Angaben sind Mindestvoraussetzungen.

* Systemarchitektur: x86(_64) oder ARMv7/ARMv8
* Prozessor: Dual-Core, 600 MHz
* Grafikprozessor mit OpenGL ES 2.0 Unterstützung
* Übliche Büro-Peripheriegeräte: Tastatur und Maus

### 3.3. Entwicklungsumgebung
Für eine effektive Entwicklung müssen auch die zu verwendenden Werkzeuge
definiert werden.
Alle Programme sind frei verfügbar und alle Services sind kostenlos.

* [Git](https://git-scm.com): Versionskontrolle

  Mit Git wird der Fortschritt des Projekts über einen Server (GitHub)
  synchronisiert. Git legt automatisch einen Verlauf der Codebasis an,
  sodass jede Änderung nachvollziehbar ist.

* [GitHub](https://github.com): Enwicklungsplattform

  Diese Plattform übernimmt einige Aufgaben:
  
    - Hosten der Git-Repositorys
      ([Code](https://github.com/aero-extrem/aero-extrem),
      [Website](https://github.com/aero-extrem/aero-extrem.github.io))
    - Issue-Tracker: Liste von fehlenden und fehlerhaften Funktionen
      ([Code](https://github.com/aero-extrem/aero-extrem/issues),
      [Website](https://github.com/aero-extrem/aero-extrem.github.io/issues))
    - Projektfortschritt / Milestones: Überblick über den gesamten
      Entwicklungsprozess
      ([Code](https://github.com/aero-extrem/aero-extrem/milestones),
      [Website](https://github.com/aero-extrem/aero-extrem.github.io/milestones))

* [GitHub Pages](https://pages.github.com): Web-Hosting

  Ein Service von GitHub, der mithilfe von [Jekyll](https://jekyllrb.com)
  Websiten generiert. Er wird für die Bereitstellung von Dokumenten zum
  Projekt verwendet: https://aero-extrem.github.io

* [Gradle](https://gradle.org): Projektverwaltung

  Ein Build-Tool, das folgende Aufgaben übernimmt:
  
    - Erfassung der Projektdateien
    - Download der benötigten Bibliotheken
    - Kompilieren des Quellcodes (und anschließende automatische Unit-Tests)
    - Prozessabläufe bereitstellen (ausführen, javadoc…)

* [JUnit](http://junit.org): Unit-Tests

  Ein Framework, das nützliche Funktionen für Unit-Tests enthält.

* [Travis CI](https://travis-ci.org): Integrationstests

  Ein Service, der nach jeder Änderung der Code-Basis einen vollständigen
  Integrationstest durchführt. Falls wichtige Funktionen ausfallen, werden
  die Entwickler benachrichtigt.

* [IntelliJ IDEA](https://www.jetbrains.com/idea/): Entwicklungsumgebung
  (optional)
  
  Diese Entwicklungsumgebung soll den Umgang mit Git, Gradle, JUnit und Java
  erleichtern. Sie wird aber nicht benötigt und kann mit einer Shell ersetzt
  werden.

## 4. Produktfunktionen

### 4.1. Physikbasierte Simulation

Das gesteuerte Flugzeug muss nach den vier fundamentalen Kräften der
Aerodynamik simuliert werden: Masse, Schub, Luftwiderstand und Lift.

### 4.2. Datenbankanbindung

Per SQLite werden alle benutzerdefinierten Variablen gespeichert, sodass sich 
die Konfiguration des Programms nicht durch einen Neustart verändert.
Außerdem gibt es eine Funktion, Flugrouten aufzuzeichnen und abzuspielen.
Diese werden mit SQL hinterlegt.

## 5. Benutzeroberfläche

Die Benutzeroberfläche setzt sich aus drei wesentlichen Teilen zusammen.

### 5.1. Hauptmenü

Das Hauptmenü ist der Startbildschirm des Programms. Hier konfiguriert der
Benutzer das Programm und startet Simulationen.

Beispiel-Layout:

![Hauptmenü](/assets/img/proto_hauptmenü.png)

### 5.2. Flugsimulation

Die Flugsimulation ist der zentrale Teil des Programms.  

Sie besteht aus zwei Teilen, der 3D-Anzeige und dem Overlay, das über der
3D-Anzeige zu sehen ist. Im Overlay sieht der Benutzer seine Eingaben.

### 5.3. Flugsimulation (Menü)

Das Menü der Flugsimulation pausiert die Simulation und lässt den Benutzer
die Simulation beenden und grobe Einstellungen vornehmen.

Beispiel-Layout:

![Flugsimulation-Menü](/assets/img/proto_ingamemenü.png)
