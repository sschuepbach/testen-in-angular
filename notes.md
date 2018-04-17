# Testen mit Angular: Notizen

## Grundlagen

### Testarten

* Unit tests werden in der Regel für einzelne Funktionen angewandt, die
idealerweise keine Seiteneffekte aufweisen (schwieriger zu testen)
* Unit tests sind v.a. nützlich für das Testen von Services, Pipes und die
Geschäftslogik von Komponenten
* Shallow unit tests testen das Zusammenspiel der Komponente mit dem Template
(also in Angular-Sprech die View)
* Integrationstests werden bspw. für das 

### Tools

### Testen mit Angular CLI

## Isolierte Unit Tests mit Jasmine

### Grundlagen Unit Testing

### Jasmine: Methoden (Auswahl)

### Jasmine: Matchers

### Beispiel

## "Shallow" Unit Tests mit Jasmine und Angular Testing Utilities

### "Isolierte" vs. "Shallow" Unit Tests

### Angular Testing Utilities

Angular testing utilities

The Angular testing utilities help to create a testing environment that makes writing tests for your various constructs really easy. It consists of the TestBed class and various helper functions, found under the @angular/core/testing namespace.

    The TestBed class is the most important concept and creates its own testing module. In reality, when you test out a construct to detach it from the module it resides in and reattach it to the testing module created by the TestBed. The TestBed class has a configureTestModule() helper method that we use to set up the test module as needed. The TestBed can also instantiate components.
    ComponentFixture is a class wrapping the component instance. This means that it has some functionality on it and it has a member that is the component instance itself.  
    The DebugElement, much like the ComponentFixture, acts as a wrapper. It, however, wraps the DOM element and not the component instance. It's a bit more than that though, as it has an injector on it that allows us to access the services that have been injected into a component. More on this topic later.


### Test-Skelett

**** `async`: `async` ist eine Funktion, die den Code in `beforeEach` in einer
asnychronen Testumgebung laufen lässt. Dies ist notwendig, da das Einlesen von
externen Dateien auf dem Dateisystem (die Template-Datei und die
Stylesheet-Datei) eine asynchrone Aktivität ist. Bevor diese beendet, kann die
Testkomponente nicht erstellt werden. Umgekehrt ist die `async`-Funktion nicht
notwendig, wenn die zu testende Komponente keine externen Abhängigkeiten besitzt
* `TestBed.configureTestingModule`: Nimmt Konfigurationsobjekt für `TestBed`
entgegen und gibt `TestBed` zurück
* `TestBed.compileComponents`: Kompiliert Komponenten, die für `TestBed` definiert
wurden. Instanzen der definierten Komponenten können nun synchron erstellt
werden
* Zweites `beforeEach`: Abschliessende Schritte für das Aufsetzen der
Testumgebungen, werden synchron *nach* dem ersten asynchronene `beforeEach` ausgeführt
* `TestBed.createComponent`: `createComponent` erstellt eine Instanz der
Komponente, die getestet werden soll, und gibt eine ComponentFixture auf die Komponente zurück. Nach dem Aufruf dieser Methode kann das
Testmodul nicht mehr weiter konfiguriert werden
* `componentInstance`: Gibt Instanz der Komponente zurück, die getestet
werden soll
* `detectChanges()`: Ermittelt geplante Änderungen in der DOM und propagiert
diese Änderungen der DOM. Dies geschieht in einer Produktionsumgebung
automatisch, in der Testumgebung muss dies "by design" manuell ausgelöst werden

### Testen der DOM mit DebugElement

### Testen einer Komponente mit einem Service

Test Doubles definieren:
* `providers:    [ {provide: UserService, useValue: userServiceStub } ]`
* `userServiceStub = { isLoggedIn: true, user: { name: 'Test User'} };`

Auf injizierten Service zugreifen
* `userService = fixture.debugElement.injector.get(UserService);`

Wichtig ist, auf das tatsächliche Test Double zuzugreifen, und nicht etwa auf
den Stub.

### Exkurs: Spies, Stubs, Fakes, Mocks und Dummies

Da Unit Testing primär Isolation ist, doch das getestete System (System Under
Test / SUT) vielfach mit
weiteren Systemen (Collaborators, Dependencies) interagiert, ist es oftmals
nötig, für das Testen sogenannte Test Doubles für die Abhängigkeiten zu
erstellen. Dabei wird zwischen verschiedenen Arten von Simulationen
unterschieden:

* *Dummy*: Ein Dummy ist ein Objekt, das übergeben wird, doch niemals wirklich
gebraucht wird. Er nimmt daher die Rolle eines Füllers für ein Parameter oder
einen Rückgabewert ein.
* *Spy*: Ein Spy ist ein Objekt, welches Interaktionen mit anderen Objekten
aufzeichnet. Es ist daher nützlich, wenn nicht nur der Zustand eines Objektes
interessiert, sondern auch die Häufigkeit von Calls, Inhalt von übergebenen
Argumenten und Rückgabewerten etc.
* *Stub*: Ein Stub ist ein Objekt mit vorprogrammierten Verhaltensweisen. Sie
werden typischerweise aus zwei Gründen genutzt:
  * Ersetzung einer externen Schnittstelle (z.B. Datenbank)
  * Als Provider von explizit definierten Daten ins System, um einen bestimmten Codepfad zu
testen
* *Fake*: Ein Fake führt die Idee eines Stubs einen Schritt weiter und verfügt
über eine tatsächliche Implementation, nimmt aber normalerweise Abkürzungen, die
ihn für den Einsatz in einem produktiven System unbrauchbar machen. Bsp.: Eine
In-Memory-Datenbank
* *Mock*: Ein Mock ist ein Objekt, das mit vordefinierten Erwartungen
implementiert wird, die ihrerseits eine Spezifikation der Calls bilden, die
erwartet werden. Sie unterscheiden sich von den anderen Test Doubles in ihrer
Erstellungs- und ihrer Verifizierungsphase.

### Testen einer Komponente mit einem asynchronen Service

async()
* whenStable(): Gibt Promise zurück, welches eingelöst wird, sobald alle offenen Promises des aktuellen Tests aufgelöst werden. Mit einem angehängten `then()` werden anschliessend die nächsten Schritte definiert.

fakeAsync(): fakeAsync() führt analog zu async() den Test in einer **fakeAsync
test zone** aus. Der Vorteil von fakeAsync() gegenüber async ist, dass der Test
synchron abzulaufen scheint. Die `whenStable().then()` Struktur wird durch
tick() ersetzt. Es gibt allerdings Einschränkungen für fakeAsync(). So ist es
beispielsweise nicht möglich, XHR calls auszuführen.

Neben async() und fakeAsync() gibt es auch noch den "traditionellen" Ansatz von
Jasmine zu asynchronem Testen.
https://jasmine.github.io/2.0/introduction.html#section-Asynchronous_Support.
Obwohl aufwendiger als der Ansatz mit async() oder fakeAsync(), ist er ab und an
notwendig (bspw. um auf den `intervalTimer` beim Testen von
`Observable`-Methoden zuzugreifen).

### Testen der Interaktion mit einer Kindkomponente

* Ziel: Testen, ob property und event bindings funktionieren wie erwartet
* Test setzt Input-Variablen und beobachtet entsprechende Outputs
* Zwei Strategien:
	* Testen als stand-alone Komponente
	* Testen, wie Kind-Komponente von der Host-Komponente gebraucht wird (mithilfe
einer test host component)


### Testen einer gerouteten Komponente

* Wenn keine komplexen Routing-Funktionalität in einer Komponente aufgerufen
werden, genügt es oftmals, den Router-Service durch einen Stub zu ersetzen
* `inject`: Funktion, die einen Service in die Testfunktion injiziert und es
ermöglicht, den Service zu ändern oder zu beobachten (spy on). Die Funktion
hat zwei Parameter:
  * Einen Array mit den Services
  * Eine anonyme Testfunktion, deren Parameter exakt der Anzahl und der
Reihenfolge der injizierten Services entsprechen
* Wenn ein Service benötigt wird, der durch den Injektor der Komponente zur
Verfügung gestellt wird, muss der Service durch
`fixture.debugElement.injector.get(<service>)` geholt werden

### Testen einer RouterOutlet-Komponente

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
