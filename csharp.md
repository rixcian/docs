# C# Dokumentace

***

Výpisky k předmětu PAP (Počítačové aplikace)



### Konvence

- Každá část kódu náleží do jmenného prostoru (namespace ConsoleApplication { ... })
- Nepoužívat stejný název pro jmenný prostor a pro třídu!
- Při psaní jména rozhraní použít konvenci *INazevRozhrani*

### Komentáře a bloky

```csharp
// Jednořádkový komentář

/*
 Víceřádkový komentář
*/

#region deklarace
  char a = 'a';
  char b = '\x0041'; // hexa
  char c = (char)65; // A
#endregion
```

### Řídící struktura - Switch

```csharp
int caseSwitch = 1;
switch (caseSwitch) {
  case 1:
    Console.Writeline("Case 1");
    break;
  case 2:
    Console.Writeline("Case 2");
    break;
  default:
    Console.Writeline("Default Case");
    break;
}
```

### Modifikátory "out" a "ref"

- Tyto dvě klíčová slova používáme při předávání parametru nějaké funkci. Obě dvě klíčová slova nám říkají, že parametr předáváme **referencí**, ne hodnotou, jako běžně. To znamená, že všechny změny provedené parametru metody budou při vrácení hodnoty funkcí, reflektovány zpět do proměnné.

```csharp
  class Zakaznik {
    public int Spocti(out int par1) {
      par1 += 2;
      return par1;
    }
  }

  z = new Zakaznik();
  int a = 15;
  int vysledek = z.Spocti(out a);
  // vysledek = 17
  // a = 17
```

### Přetěžování metod

- V jedné třídě jsou metody stejně pojmenované, ale s jinou signaturou

- Jiná signatura je dána některou z těchto možností:
  
  - Jiný počet parametrů
  
  - Jiný typ parametrů

```csharp
    // Stejná signatura, kompilátor vyhodí error
    class Test {
      public void SampleMethod(out int i) { ... }
      public void SampleMethod(ref int i) { ... }
    }

    // Rozdílná signatura
    class Test {
      public void SampleMethod(int i) { ... }
      public void SampleMethod(ref int i) { ... }
    }
```

### Modifikátory dostupnosti

- **Public** - dostupné odkudkoliv
- **Protected** - dostupné pouze v dané třídě a v jejich dědicích
- **Private** - dostupné pouze v daném typu (třídě nebo struktuře)
- **Internal** - dostupné pouze ze stejné assembly, nikoliv však z jiné
- **Protected Internal** - dostupné pouze z dědiců v dané assembly

| **Člen**  | Výchozí | Povolené                                                 |
|:---------:|:-------:|:--------------------------------------------------------:|
| enum      | public  | None                                                     |
| class     | private | public, protected, internal, private, protected internal |
| interface | public  | None                                                     |
| struct    | private | public, internal, private                                |

### Další modifikátory

- **Static** - při použití u třídy musí být všechny prvky v dané třídě statické
- **Sealed** - při použití u třídy brání ostatním třídám z dané třídy dědit
- **Abstract** - indikuje, že prvek, který modifikuje, nemá kompletní implementaci. Při použití u třídy se z dané třídy může pouze dědit, ne vytvářet objekty
- **Virtual** - povoluje tzv. "přepsat" prvek v dědičné třídě
- **Partial** - umožňuje rozdělit kód třídy na více částí, které jsou v různých souborech cs

### Accessors

```csharp
public int Operand1 {
    get;
    set;
}

private int operand2;
public int Operand2 {
    private set {
        ...
    }
    get {
        return operand2;
    }
}
```

### Dědičnost

- .NET neumožňuje násobnou dědičnost

```csharp
  public class Student {
      protected int delkaStudia = 5;

      public double SpoctiStipendium() {
          return 5000 * delkaStudia;
      }
  }

  public class ZahranicniStudent : Student {
      private double koef = 0.9;

      public new double SpoctiStipendium() {
          return 5000 * koef * delkaStudia;
      }
  }

  Student s = new ZahranicniStudent();
  Console.WriteLine(s.SpoctiStipendium());
```

### Polymorfismus

- virtuální metody pomocí *virtual* a *override*

```csharp
  public class Student {
      protected int delkaStudia = 5;
        public virtual double SpoctiStipendium() {
        return 5000 * delkaStudia;
      }
  }

  public class ZahranicniStudent : Student {
      private double koef = 0.9;
    public override double SpoctiStipendium() {
          return 5000 * koef * delkaStudia;
      }
  }

  Student s = new ZahranicniStudent();
  Console.WriteLine(s.SpoctiStipendium());
```

- volání implementace předchůdce pomocí *base*
- *base* nemusí být vázáno pouze na virtuální metody

```csharp
  public class Student {
      protected int delkaStudia = 5;
      public virtual double SpoctiStipendium() {
          return 5000 * delkaStudia;
      }
  }

  public class ZahranicniStudent : Student {
      private double koef = 0.9;    
      public override double SpoctiStipendium() {
          return base.SpoctiStipendium() * koef;
      }
  }

  Student s = new ZahranicniStudent();
  Console.WriteLine(s.SpoctiStipendium());
```

### Abstraktní třídy a metody

- abstraktní metoda se označuje slovem *abstract*

- nemá žádný kód

- třída obsahující abstraktní metodu musí být označena jako abstraktní

- nelze vytvářet instance abstraktních tříd

- v dědici musí být abstraktní metoda označena jako *override* a napsána její implementace

```csharp
public abstract class Student {
    protected int delkaStudia = 5;
    public abstract double SpoctiStipendium();    
}

public class ZahranicniStudent : Student {
    private double koef = 0.9;

    public override double SpoctiStipendium() {
        return 5000 * koef * delkaStudia;
    }
}
```

### Statické třídy, metody, accessory

**Statické třídy**

- jsou označeny jako *static*

- musí obsahovat pouze statické vlastnosti a metody

- není možné vytvářet instance těchto tříd

- jsou automaticky sealed, tj. nelze z nich dědit

- zároveň nemohou dědit z jiné řídy než Object

**Statické metody**

- nemohou využívat vlastností třídy, které nejsou statické

- umožňují overloading (přetěžování) nikoliv overriding (překrývání)

**Statické vlastnosti**

- je na ně přistupováno přes jméno třídy

- neexistují lokální statické proměnné

- jsou inicializovány ještě před statickým konstruktorem

- mohou být i private

### Konstruktory

- konstruktor má stejný název jako třída

- není-li ve třídě napsán konstruktor, pak je automaticky vygenerován výchozí

```csharp
// Volání konstruktorů v jedné třídě
public class Student {
    private string jmeno;
    private int rokNarozeni;
    private byte delkaStudia;

    public Student(string jmeno, int rokNarozeni) {
        this.jmeno = jmeno;
        this.rokNarozeni = rokNarozeni;
    }

    public Student(string jmenoStud, int rokNarozeniStud, byte delka)
        : this(jmenoStud, rokNarozeniStud) 
    {
        delkaStudia = delka;
    }
}

Student stud1 = new Student("Adam", 1990);
Student stud2 = new Student("Adam", 1990, 5);
```

**Statické konstruktory**

- nemají modifikátory přístupu (jsou veřejné)

- nemají parametry

- nemohou být volány přímo na vyžádání

- jsou volány automaticky před tím, než je vytvořena instance třídy nebo přistoupeno ke statické vlastnosti

```csharp
// Statické metody určité třídy pracují nad nějakým datovým zdrojem, který musí být nejprve inicializován/načten/vytvořen
public class Transform {
    private static DateTime initialSample;

    static Transfrom() {
        initialSample = DateTime.Now;
    }
    
    public static double GetDifference() {
        return (DateTime.Now - Transfrom.initialSample).TotalSeconds;
    }
}
```

### Object()

- kažadá třída dědí ze třídy Object

- proto je možné překrýt virtuální metody: bool **Equals**(Object obj), void **Finalize**(), int **GetHashCode**(), string **ToString**()

- dále je možné využít nevirtuální metody: 
  
  - Type **GetType**() - vrací Type aktuální instance
  
  - Object **MemberwiseClone**() - vytváří mělkou kopii (shallow copy) aktuálního objektu (např. potřebuji vytvořit kopii třídy Student)

### Finalizace v .NET

- *protected virtual void Finalize()*
  
  - v každé třídě lze překrýt tuto metodu

- **Garbage Collector**
  
  - označuje k finalizaci pouze objekty s překrytou metodou *Finalize*

### Destruktory

- metoda označená **~**

- stejného jména jako třída

- destruktory lze použít pouze u tříd

- jedna třída může mít pouze jeden destruktor

- není možný override ani overload

- nemají žádné modifikátory

- nemohou být volány
  
  - *Vynucení: GC.Collect(Object obj)*

```csharp
public sealed class NameChecker {
    ~Namechecker {
        // ... Destruktor
    }
}
```

### Rozhraní

- definuje, co musí třída nebo struktura umět

- nahrazuje násobnou dědičnost

- klíčové slovo *interface*

- konvence jména *INazevRozhrani*

- může určovat signatury: metod, vlastností, indexerů, událostí

- vše je a musí být public

- třída může splňovat vícero rozhraní (ale musí je samozřejmě všechny také implementovat)

```csharp
// Add -> New Item -> Interface -> IScholarShip.cs
namespace Studenti {
    interface IScholarShip {
        byte LengthOfStudy { get; set; }
        double CountScholarShip(int annualFee);
    }
}
```

##### Vybraná rozhraní z knihovny tříd

- **IComparable**
  
  - slouží pro porovnávání dvou objetů (resp. hodnotových typů)
  
  - stanovuje metodu *int CompareTo(Object obj)*

- **ICloneable**
  
  - slouží jako podpora klonování, umožňuje nahradit výchozí klonování vlastním specifickým způsobem
  
  - stanovuje metodu *Object Clone()*

- **IDisposeable**
  
  - poskytuje mechanismus pro uvolňování unmanaged zdrojů
  
  - stanovuje metodu *void Dispose()*

- **IEquatable<T>**
  
  - obecný mechanismus pro určení shody dvou instancí a jedná se o generický interface
  
  - stanovuje metodu *bool Equals(T other)*

### Genericita

- generické mohou být třídy nebo metody

- znamená to využití typu jako parametru

- o jaký typ se jedná se určí až v okamžiku vytváření instance resp. volání metody

- tzn. kód třídy pracuje s obecným typem

- **Motivace:**
  
  - větší šance na znovupoužití kódu
  
  - řeší typovou bezpečnost
  
  - u generických tříd lze určit omezující požadavky na generický typ

```csharp
// Definice třídy
class Node<T> {
    private T data;
    private int id;

    public Node(T newData) {
        data = newData;
    }
}

// Použití třídy
Node<int> prvni = new Node<int>(42);

Student pom = new Student();
Node<Student> druhy = new Node<Student>(pom);
```

##### Generická rozhraní

- můžeme navrhnout vlastní generické rozhraní

- můžeme využít stávající, např. IEquatable

```csharp
public class Student : IComparable, IEquatable<Student>
{
    public bool Equals(Student other) {
        return (id == other.id) && (jmeno == other.jmeno);
    }
}
```

### Výčty

- potřebujeme omezený počet hodnot

```csharp
enum RocniObdobi { jaro, leto, podzim, zima };
enum DenniObdobi { rano = 5, poledne = rano + 2, vecer };

RocniObdobi ro = RocniObdobi.jaro;
```

### Struktury

- můžou obsahovat: konstruktory, konstanty, pole, metody, vlastnosti, indexery, operátory, události a vnořené třídy

- **nelze přeložit když:**
  
  - inicializuji vlastnost přímo v těle struktury
  
  - navrhuji vlastní bezparametrický konstruktor
  
  - parametrický konstruktor neinicializuje všechny vlastnosti

- **dědičnost:**
  
  - každá dědí pouze ze třídy Object, nelze dědit z jakékoliv jiné třídy
  
  - je sealed (nelze ze struktury dále dědit)

```csharp
struct Point {
    public int x;
    public int y;
    // public int y = 2; NELZE

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

Point p;
p.x = 5; p.y = 10;

Point p2 = new Point(5, 10);
```

### Indexovače

- mechanismus, který umožňuje k objektu přistupovat jako k poli pomocí []

```csharp
enum language { en, cz };

public class Lokalizator {
    private string[] cz = new string[] { "soubor", "projekt", "sestaveni" };
    private string[] en = new string[] { "file", "project", "build" };

    public string this[language lang, int poradi] {
        get {
            if (lang == language.cz) { return cz[poradi]; }
            return en[poradi];
        }
    }
}

Lokalizator lk = new Lokalizator();
Console.WriteLine( lk[ language.cz, 0 ] ); // soubor
Console.WriteLine( lk[ language.en, 0 ] ); // file
```



### Soubory

- při práci se soubory musíme přidat namespace *using System.IO* 

- nejzajímavější třídy System.IO: Path, Directory, File



### Výjimky

```csharp
// Ukázka vyvolání vyjímky
private bool isMature(Student stud) {
    if (stud == null) {
        // Vyvolání vyjímky
        throw new ArgumentNullException("stud", "Parameter stud cannot be null.");
    }
    return (stud.Age > 18);
}

// Ukázka ošetření vyjímky
private void button_Click(object sender, EventArgs e) {
    try {
        button.Text = isMature(s).ToString();
    }
    catch (ArgumentNullException ex) {
        labelError.Content = ex.Message;
    }
    finally {
        button.Text = "";
    }
}
```



### Delegáti

- delegát může volat i více metod najednou

- v .NET existuje mnoho metod vyžadující jako parametr nějaký typ delegáta
  
  - List<T>.Find()
  
  - List<T>.Average()

```csharp
public delegate void Zpracuj(string message);

public static void MojeMetoda(string message) {
    Console.WriteLine(message);
}

static void Main(string[] args) {
    // Vytvoření instance delegáta
    Zpracuj mm = Program.MojeMetoda;
    mm("Volání instance delegáta!");
}
```



### Události

- **event** - akce, na kterou je možné odpovědět/zachytit

- vyvolání události pomocí *event raise*

- Více na: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/events/


