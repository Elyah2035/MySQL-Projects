---
author: "Martin Schoepf"
lang: "de-AT"
---

# SWPStoff


## ArrayList

Wird benötigt, um Arrays zu erzeugen, welche zur Laufzeit erweitert werden können. Wird auch als dynamischer Array bezeichnet. **Wichtig**: eine ArrayList kann nur Objektdatentypen (keine primitiven Datentypen) speichern. Java stellt für genau diesen Zweck *Wrapperklassen* zur Verfügung. *Integer* beinhaltet einen *int*, oder *Float* beinhaltet einen *float*, usw.

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    // Benutzer gibt beliebig viele Zahlen en
    // werden in Array gespeichert
    ArrayList<Integer> inputsArray = new ArrayList<Integer>();
    int input = 0;
    Scanner scan = new Scanner(System.in);
    while (input != -1) {
      input = scan.nextInt();
      inputsArray.add(input); // erweitern um eine Stelle
    }
    // hiert wird size() anstatt length verwendet
    for (int i = 0; i < inputsArray.size(); i++) {
      System.out.println(inputsArray.get(i));
    }
  }
}
```

weiteres Beispiel:

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Main {
  public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);
    ArrayList<String> al = new ArrayList<String>();
    String vorName = "";
    boolean cont = false;
    do {
      cont = false;
      vorName = scan.nextLine();
      if (!("".equals(vorName))) {
        al.add(vorName);
        cont = true;
      }
    } while ( cont == true );
    System.out.println(al);
    System.out.printf("Die Laenge der Arraylist %s is %d", al, al.size());
  }
}
```

## Vererbung

Wird verwendet um Code besser strukturieren zu können. Öfter verwendete Attribute und Methoden können in einer Klasse zusammengefasst werden, um Fehler zu vermeiden und dem *DRY*-Prinzip zu folgen.
Um von Klassen erben zu können, wird das keyword *extends* verwendet.

### *super*-Keyword

*super* ruft den Konstruktor der vererbten Klasse auf.
Falls eine Methode der Vaterklasse aufgerufen werden soll, obwohl die aktuelle Klasse die Methode überschrieben hat, kann man mit 

```java
super.methodenName(...)
```

die ursprüngliche Methode aufrufen.

```java
\\ in Tier.java
public abstract class Tier {
	
	private String name;
	private String gebTag;
	
	public String getName() {
		return name;
	}
	public String getGebTag() {
		return gebTag;
	}
	
	public Tier(String name, String gebTag) {
		this.name = name;
		this.gebTag = gebTag;
	}

  public String gibLaut(){
    return "tierGeräusch";
  }
}

\\in Hund.java
public class Hund extends Tier {
	
	private String rasse;
	
	public Hund(String name, String gebTag, String rasse) {
		super(name, gebTag); // Aufruf des Konstruktors der Vaterklasse
		this.rasse = rasse;
	}

	public String getRasse() {
		return rasse;
	}

  @Override
  public String gibLaut(){
    return "wuff";
  }

  public String gibLautVonVater(){
    return super.gibLaut();
  }
}

\\ in Main.java
public class Main {
  public static void main(String[] args) {
    Hund hund = new Hund("Hansi","1.1.2010","Pudel");
    hund.gibLaut(); // gibt aus: wuff
    hund.gibLautVonVater(); // gibt aus: tierGeräusch
  }
}

```

### Keyword *abstract*

Markiert die Klasse, dass sie nicht instanziiert werden kann(es kann kein Objekt dieser Klasse erzeugt werden).

### Annotation *@Override*

Steht über einer Funktion, welche überschrieben werden soll. Es kann auch ohne *@Override* überschrieben werden. *@Override* erstellt einen Vertrag, dass genau die Signatur der Funktion entsprechen muss. Ohne diesem Keyword, läuft man Gefahr, dass wenn die Signatur einer Methode der Vaterklasse sich verändert, ich die Methode nur überlade und nicht überschreibe.

```java
// Fahrzeug.java
// Erstellen einer Klasse welche vererbt werden soll. - abstract
public abstract class Fahrzeug {

	private int ps;
	private int baujahr;
	private double currentVelocity;
	
	public Fahrzeug(int ps, int baujahr) {
		this.ps = ps;
		this.baujahr = baujahr;
		this.currentVelocity = 0.0;
	}
	
	public void accelerate() {
		this.currentVelocity += this.ps/10.0;
	}

	public int getPs() {
		return ps;
	}

	public int getBaujahr() {
		return baujahr;
	}

	public double getCurrentVelocity() {
		return currentVelocity;
	}
	
	protected void setCurrentVelocity(double v) {
		this.currentVelocity = v;
	}
}

// Auto.java
// Subklasse (Klasse die von Vaterklasse erbt)
public class Auto extends Fahrzeug{
	
	private String marke;

	public Auto(int ps, int baujahr, String marke) {
		super(ps, baujahr);
		this.marke = marke;
	}

	public String getMarke() {
		return marke;
	}
}

// Traktor.java
// erbt ebenfalls von Fahrzeug
public class Traktor extends Fahrzeug {
	private double zugKraft;
	
	public Traktor(int ps, int baujahr, double zugKraft) {
		super(ps, baujahr);
		this.zugKraft = zugKraft;
	}

	public double getZugKraft() {
		return zugKraft;
	}
	
	@Override
	public void accelerate() {
		this.setCurrentVelocity( this.getCurrentVelocity() + this.getPs()/40.0);
//		this.currentVelocity += this.getPs()/40.0;
	}

}

```

### instanceof - überprüfen der Klasse bzw. der Vaterklasse

Wird verwendet, um während der Laufzeit zu überprüfen ob, das Objekt in eine bestimmte Klasse ***gecastet*** werden kann, also ob die Klasse des Objekts dem gecasteten entspricht bzw. in der Vererbungshirarchie anzutreffen ist.

```java
// in Tier.java
public abstract class Tier{
  protected String name;
  protected String geburtsDatum;

  public getGeburtsDatum() { 
    return this.geburtsDatum; 
  }

  public String getName(){
    return this.name;
  }

  public Tier(String name, String geburtsDatum){
    this.name = name;
    this.geburtsDatum = geburtsDatum;
  }
}

// in Hund.java
public class Hund extends Tier{
  private String rasse;

  public String getRasse(){
    return this.rasse;
  }

  public Hund(String name, String geburtsDatum, String rasse){
    super(name, geburtsDatum);
    this.rasse = rasse;
  }
}

// in Katze.java
public class Katze extends Tier{

  public Katze(String name, String geburtsDatum){
    super(name, geburtsDatum);
  }
}

// in Klasse Main.java
import java.util.ArrayList;

public class Main {

	public static void printHundeRasse(ArrayList<Tier> tiere) {
		for (Tier t : tiere) { // forEach loop
			if (t instanceof Hund) {
				Hund h = (Hund) t; // cast (umwandlung) zu Hund
				System.out.println(h.getRasse());
			} else {
				System.out.println("leider keine Rasse vorhanden (kein Hund)");
			}
		}

		// alternative 
		for (int i = 0; i < tiere.size(); i++) {
			Tier t = tiere.get(i);
			if (t instanceof Hund) {
				Hund h = (Hund) t; // cast (umwandlung) zu Hund
				System.out.println(h.getRasse());
			} else {
				System.out.println("leider keine Rasse vorhanden (kein Hund)");
			}
		}
	}

	public static void main(String[] args) {
		ArrayList<Katze> katzen = new ArrayList<Katze>();
		Katze schmusi = new Katze("Schmusi", "10.10.2010");
		katzen.add(schmusi);

		ArrayList<Hund> hunde = new ArrayList<Hund>();
		Hund kratzi = new Hund("Kratzi", "10.10.2010", "Rotweiler");
		hunde.add(kratzi);

		ArrayList<Tier> tiere = new ArrayList<Tier>();

		tiere.add(schmusi);
		tiere.add(kratzi);

		System.out.println(tiere);

		// geht nicht = abstract
//		Tier t = new Tier("tiere=i", "1.1.1111");

		printHundeRasse(tiere);

		kratzi.getRasse();

	}

}
```



# Zufallszahlen (Random)

Können von System generiert werden, es gibt mehrere Möglichkeiten, diese zu erzeugen.

```java
import java.util.Random; // wichtig

public class Main {
  public static void main(String[] args) {
    // Erstellen eines int
    Random rand = new Random();
    int randomZahl = rand.nextInt(10); // Zahl zwischen 0 und 10 exclusive
    
    Random rand = new Random();
    int bound = 1000; // wie weit - exclusive dieser Zahl
    int randomZahl = rand.nextInt(bound) + 10;
    System.out.println(randomZahl); // Zahl zwischen 10 - 1010 exclusive
    
    double randDouble = rand.nextDouble();
    System.out.println(randDouble); // Zahl zwischen 0.0 und 1.0 exclusive 
    
    // Ziel ist es einen Double von 10.0 - 20.0 zu machen
    int randomZahl2 = rand.nextInt(9) + 10;
    double randDouble2 = rand.nextDouble();
    double res = randDouble2 + randomZahl2;
    System.out.println(res);
    
    // Alternative
    double randDouble3 = rand.nextDouble()*10.0 + 10.0;
    System.out.println(randDouble3);
  }
}

```
# git basics

git ist ein Versionsverwaltungssystem, welches uns erlaubt verschiedene Versionen nachvollziehbar zu speichern und es erlaubt uns einfaches Zusammenarbeiten mit anderen Mitarbeitern.

Es existiert (normalerweise) ein `remote` repository auf zb. Github und ein `lokales` auf unserer Festplatte.

Wenn wir ein Repository angelegt haben (zb. auf Github) können wir es mit:

```git clone https://github.com/username/repositoryName``` 

auf unser System clonen.

**Wir müssen uns im git Verzeichnis befinden um folgende Befehle ausführen zu können.**

mit `git add dateiname.endung` können wir Dateien zu den "beobachtenden" Dateien hinzufügen.

mit `git commit -m"nachricht welche in de Logs erscheint"` erzeuge ich einen neuen *Busstop* welcher wie eine Version der Codebasis angesehen werden kann.

Durch `git push` werden die Änderungen meines Repostitories auf das `remote` Repository übertragen.

Um Änderungen von `remote` Repository in mein System zu holen, wir der Befehl `git pull` verwendet.

Jederzeit kann ```git status``` verwendet werden um den aktuellen Status des Repositories abzufragen.

### Ablaufdiagram eines typischen Workflows

![ablauf diagramm](diagrams/git.png)

### Ignorieren von Dateien

um nicht benötigte Dateien (zB. Dateien, welche beim compilieren erzeugt werden) zu ignorieren können wir im Hauptverzeichnis die Datei *.gitignore* erzeugen/editieren.

```
**.class
```

ignoriert alle *.class* Dateien.

# Exceptions

Exception entsprechen Ausnahmen, auf die der Benutzer reagieren kann.
Viele kennen eine der folgenden: *ArrayOutOfBoundsException* oder *NullPointerException*.
Diese Ausnahmen können entweder mit:

- try/catch
- throws 

behandelt werden. Mit try/catch werden die Programmzeilen, welche
eine Exception werden können in einen Block eingeschlossen.

```java
FileWriter writer = null;
try {
	writer = new FileWriter("myFile.txt");
	writer.write(myString);
	writer.close();
	System.out.println("Successfully wrote to the file.");
} catch (IOException e) {
	System.out.println("Da ist etwas schief gelaufen.");
}
```

hier kann auf den Fehler direkt reagiert werden.
Falls die Aufgabe des Fehlerbehandelns abgegeben werden soll, kann man 
die Exception an die aufrufende Funktion weiterreichen --> *throws*

```java
private static void writeToFile(String what) throws IOException {
	FileWriter writer = null;
	writer = new FileWriter("myFile.txt");
	writer.write(what);
	writer.close();
	System.out.println("huhu");
}
```

Exceptions erben von *Throwable*, welche eine Vaterklasse von *Error* und
*Exception* ist. *Error* definiert Fehler, auf die nicht reagiert werden kann/soll.

Man kann Exception einfach als Klasse definieren:

```java
// in class ToExpensiveException
public class ToExpensiveException extends Exception {

}
```

diese kann dann wie folgt verwendet werden:

```java
public static void throwMeMaybeAnException(int price) throws ToExpensiveException {
		if (price > 10) {
			throw new ToExpensiveException(); // hier wird die Exception erzeugt und geworfen
		}
	}
```

## Ein ganzes Beispiel

```java
// in ToExpensiveException.java
public class ToExpensiveException extends Exception {

}

```

```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
	
	public static void throwMeMaybeAnException(int price) throws ToExpensiveException {
		if (price > 10) {
			throw new ToExpensiveException();
		}
	}
	
	private static void writeToFile(String what) throws IOException {
		FileWriter writer = null;
		writer = new FileWriter("myFile.txt");
		writer.write(what);
		writer.close();
		System.out.println("huhu");
	}
	
	public static void main(String[] args) throws IOException {
		String myString = "Hello, world!";
		// Variante try catch
		FileWriter writer = null;
		try {
			writer = new FileWriter("myFile.txt");
			writer.write(myString);
			writer.close();
			System.out.println("Successfully wrote to the file.");
		} catch (IOException e) {
			System.out.println("Da ist etwas schief gelaufen.");
		}
		
		// Behandlung des Fehlers
		try {
			throwMeMaybeAnException(11);
			System.out.println("Yeah I have something to eat");
		} catch (ToExpensiveException e) {
			System.out.println("Sorry inflation!");
		}
		
		try {
			writeToFile("Servas");
		} catch (IOException e) {
			System.out.println("sorry my friend");
		}
		
	}

}
```