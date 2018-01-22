## Grundlagen

### Testarten

* Unit tests werden in der Regel für einzelne Funktionen angewandt, die
idealerweise keine Seiteneffekte aufweisen (schwieriger zu testen)
* Unit tests sind v.a. nützlich für das Testen von Services, Pipes und die
Geschäftslogik von Komponenten
* Shallow unit tests testen das Zusammenspiel der Komponente mit dem Template
(also in Angular-Sprech die View)
* Integrationstests werden bspw. für das 


## Isolierte Unit Tests

### Grundlagen

* Ein Unit Test ist eine Annahme über die Funktionsweise von Code
* I.d.R. wird zum Testen einer Klasse eine Reihe von Unit Tests geschrieben
* Wichtig: Auch "edge cases" für Inputs testen!
	* Beispiel für Zahlen: undefined, 0, negative Zahlen, Maximal-/Minimalwerte
  * Beispiel für Strings: undefined, leere Strings, ev. Steuer- und Sonderzeichen
* Tests sind kein Beweis für Fehlerfreiheit
* Tests werden i.d.R. in separater Datei geschrieben
* Jasmine: BDD -> "Natürlichsprachige" Spezifikationen

### Jasmine: Wichtigste Methoden

* describe: Erstellt eine Gruppe von Spezifikationen
* it: Erstellt eine Spezifikation
* expect: Erstellt eine Erwartung für einen Test. Erwartung hat Matchers als Methoden
* beforeEach / beforeAll: Nimmt Funktion entgegen, die vor einer / allen Spezifikationen
ausgeführt wird

### Jasmine: Wichtige Matchers

* expect(tatsächlicher_wert).{matcher}(erwarteter Wert)
* toBeDefined / toBeUndefined: Objekt ist definiert / undefiniert
* toBe: Tatsächlich Wert ist gleich (===) erwarteter Wert
* toContain: Enthält Wert
* toMatch: Regex
* toEqual: Deep equality
* toBeTruthy / toBeFalsy
* toBeGreaterThan[OrEqual] /toBeLessThan[OrEqual]
* toThrow

## "Shallow" Unit Tests

### Grundkomponenten Angular testing utilities

Angular testing utilities

The Angular testing utilities help to create a testing environment that makes writing tests for your various constructs really easy. It consists of the TestBed class and various helper functions, found under the @angular/core/testing namespace.

    The TestBed class is the most important concept and creates its own testing module. In reality, when you test out a construct to detach it from the module it resides in and reattach it to the testing module created by the TestBed. The TestBed class has a configureTestModule() helper method that we use to set up the test module as needed. The TestBed can also instantiate components.
    ComponentFixture is a class wrapping the component instance. This means that it has some functionality on it and it has a member that is the component instance itself.  
    The DebugElement, much like the ComponentFixture, acts as a wrapper. It, however, wraps the DOM element and not the component instance. It's a bit more than that though, as it has an injector on it that allows us to access the services that have been injected into a component. More on this topic later.

This was a brief overview to our testing environment, the frameworks and libraries used, as well as some important concepts that we will use heavily in the upcoming sections.

## Integrationstests

### Mocking

## E2E Tests

* Im Hintergrund: Selenium

## Test-Driven Development

### Was ist TDD?

* Hohe Code Coverage
* Macht dadurch Refactoring einfacher (Regressionstests "for free")
* ...

### Red-Green-Refactor Zyklus

0. Klarheit über Spezifikation des zu entwickelnden Features verschaffen und die
	 dazu notwendigen Entwicklungsschritte verschaffen
1. Test schreiben, der Funktion oder Erweiterung einer Funktion testet
2. Test laufen lassen -> Test muss fehlschlagen, entweder zur Kompilierzeit (weil Testcode nicht kompiliert) oder Laufzeit (Tests werden nicht erfüllt)
3. So viel Code schreiben (und nur so viel!), dass Test erfüllt wird (quick &
	 dirty)
4. Test laufen lassen -> Test läuft durch
5. Refaktorieren, insbesondere: Redundanz im Code (auch zwischen Test und
Implementation) beseitigen; Verschieben von Codeteilen dahin, wo sie logisch
Sinn machen; Codeelemente aufteilen, so dass sie möglichst konzis sind und
nicht mehr als einen Zweck / ein Feature erfüllen; Umbenennen von Klassen, Funktionen etc., so dass der
Name die aktuelle Funktionsweise möglichst exakt beschreibt; 

### Was sollen / müssen wir testen?

* Dient als Diskussionsgrundlage

## Ressourcen
