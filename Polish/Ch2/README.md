
# C dla Początkującego

## Pierwszy program

Twój pierwszy program w C wyświetli na ekranie tekst "Hello, World!". W polu na kod źródłowy (zależnie od edytora) wpisz następującą treść:

```c

#include <stdio.h>

int main(void) {
    printf("Hello, world!");
    return 0;
}

```

Upewnij się, że tryb twojego kompilatora jest przełączony na C89. Można tego dokonać
poprzez podanie parametru `-std=c89`.

Po zapisaniu i nawigacji za pomocą powłoki do ścieżki zapisu uruchom kompilator:

```sh
gcc plik.c -o plik -std=c89
```

A następnie go uruchom:

```sh
plik
```

Jeśli program wypisał na terminal napis `Hello, World!`, wszystko działa poprawnie.

### Uwagi

W tym podręczniku będę stwierdzał "zalecam robić X, nie zalecam robić Y" - to znaczy, że można zrobić Y i tak się czasami robi, ale użycie X będzie w większości przypadków lepsze. Masz pełną swobodę w wyborze techniki, aczkolwiek używając tych rzadziej spotykanych możesz pogorszyć jakość swojego kodu.

Czasami będę ujawniał przed Tobą "wiedzę tajemną" - bardziej zaawansowane tajniki programowania, których możesz nie rozumieć. Nie ma w tym nic złego.

Na początku, będę pokazywał ci techniki które mogą nie być już w użyciu - jeśli chcesz przygotowywać najlepszy kod - stosuj się do zaleceń w dalszych rozdziałach - 3 i 4 - w których są już przedstawione standardy C99 i C11 (a nie C89), starszy standard został wprowadzony dla maksymalnego uproszczenia.

## Podstawowe pojęcia

Dla właściwego zrozumienia języka C niezbędne jest przyswojenie ogólników.

### Generalne stwierdzenia

Jak każdy język programowania, C sam w sobie jest niezrozumiały dla procesora. Został on stworzony w celu umożliwienia pisania kodu szybciej niż w Assemblerze.

<!-- TODO: Trzeba jakoś zapowiedzieć linker, aktualna definicja jest niezbyt poprawna merytorycznie -->

Program, który zamienia kod C na wykonywalny program to kompilator. Jeśli pracujesz nad projektem, który wymaga kilku plików kodu źródłowego (np. pliki nagłówkowe), wtedy jest uruchamiany kolejny program - linker. Linker służy do połączenia różnych plików i stworzenia jednej aplikacji lub biblioteki. Linker jest uruchamiany nawet wtedy, gdy aplikacja posiada jeden plik źródłowy. Biblioteka jest zestawem procedur, który sam w sobie nie jest wykonywalny ale może być używany przez inne programy. Kompilacja i łączenie plików są ze sobą bardzo ściśle powiązane, stąd są przez wielu traktowane jako jeden proces.

Jedną rzecz warto sobie uświadomić - kompilacja jest praktycznie jednokierunkowa: przekształcenie kodu źródłowego C w kod maszynowy jest bardzo proste, natomiast odwrotnie - nie. Dekompilatory istnieją, ale rzadko tworzą użyteczny kod C.

Najpopularniejszym wolnym (w uproszczeniu darmowym) kompilatorem jest GNU Compiler Collection.

Pewnie zaskoczy Cię to, że C bez bibliotek standardowych nie może zbyt wiele (wyjątkiem jest inline assembly, którego można użyć w połączeniu z przerwaniami, ale przeważnie nie robi się tego; większość funkcji jak np. `printf`, znajduje się w tzw. `libc`, czyli bibliotece standardowej C). Język C w grupie języków programowania wysokiego poziomu jest stosunkowo nisko, dlatego mówi się o nim jako o języku **średniego poziomu**.

Dzięki temu kod napisany w C można dość łatwo przetłumaczyć na kod asemblera. Bardzo łatwo jest też łączyć ze sobą kod napisany w języku asemblera z kodem napisanym w C.

Początkujący programista, czytający kod programu w C może odnieść bardzo nieprzyjemne wrażenie, które można opisać cytatem "ja nigdy tego nie ogarnę". Wszystkie te elementy języka C, które wydają Ci się dziwne i nielogiczne w miarę, jak będziesz nabierał doświadczenia nagle okażą się całkiem przemyślanie dobrane i takie, a nie inne konstrukcje przypadną Ci do gustu. Dalsza lektura tego podręcznika oraz zaznajamianie się z funkcjami z różnych bibliotek ukażą Ci całą gamę możliwości, które daje C doświadczonemu programiście. 

### Paradygmaty programowania

Jeśli miałeś styczność z Pascalem, to pewnie słyszałeś o nim, że jest strukturalnym językiem programowania. W C nie ma tak ścisłej struktury blokowej, mimo to bardzo ważne jest zrozumienie, co oznacza struktura blokowa. Blok jest grupą instrukcji, połączonych w ten sposób, że są traktowane jak jedna całość. W C, blok zawiera się pomiędzy klamrami `{}`. Blok może także zawierać kolejne, zagnieżdżone bloki.

### Bloki instrukcji

Generalnie, blok zawiera ciąg kolejno wykonywanych poleceń. Polecenia zawsze (z nielicznymi wyjątkami) kończą się średnikiem (;). W jednej linii może znajdować się wiele poleceń, choć dla zwiększenia czytelności kodu najczęściej pisze się pojedynczą instrukcję w każdej linii. Jest kilka rodzajów poleceń, np. instrukcje przypisania, warunkowe czy pętli.

Pomiędzy poleceniami są również odstępy - spacje, tabulacje oraz przejścia do następnej linii, przy czym dla kompilatora te trzy rodzaje odstępów mają takie samo znaczenie. Ale uwaga, nie możemy łamać ciągów na kilka linii, tak po prostu!

```c
printf("Hello
world");
return 0;
```

Powyższy kod jest błędny. W C ważne jest samo istnienie odstępu, a nie typ / rozmiar.

Jako ciekawostka która zostanie omówiona później, ciągi można łamać, ale trzeba na końcu linii umieścić znak `\`:

```c
printf("Hello \
world!");
return 0;
```

Zasięg to pojęcie dotyczące zmiennych, które przechowują dane przetwarzane przez program. W większości programów są zarówno zmienne wykorzystywane przez cały czas działania programu oraz takie, które są używane przez pojedynczy blok programu (np. funkcję). Na przykład, w pewnym programie w pewnym momencie jest wykonywane skomplikowane obliczenie, które wymaga zadeklarowania wielu zmiennych do przechowywania pośrednich wyników. Ale przez większą część tego działania te zmienne są niepotrzebne i zajmują tylko miejsce w pamięci - najlepiej gdyby to miejsce zostało zarezerwowane tuż przed wykonaniem wspomnianych obliczeń, a zaraz po ich wykonaniu zwolnione. Dlatego w C istnieją zmienne globalne oraz lokalne. Zmienne globalne mogą być używane w każdym miejscu programu, natomiast lokalne - tylko w określonym bloku czy funkcji (oraz blokach w nim zawartych). Generalnie - zmienna zadeklarowana w danym bloku jest dostępna tylko wewnątrz niego i bloków potomnych.

### Programowanie sturkturalne

Funkcje są ściśle związane ze strukturą blokową, funkcja to po prostu blok instrukcji, który jest potem wywoływany w programie za pomocą pojedynczego polecenia. Zazwyczaj funkcja wykonuje pewne określone zadanie. Najlepiej wykorzystywać funkcje do jednego, względnie prostego i ściśle określonego zadania.

Każda funkcja ma swoją nazwę, za pomocą której jest potem wywoływana w programie, oraz blok wykonywanych poleceń. Funkcja  opcjonalnie pobiera pewne dane, czyli argumenty, czasami także zwraca pewną wartość po zakończeniu wykonywania. Dobrym nawykiem jest dzielenie dużego programu na zestaw mniejszych funkcji aby łatwiej odnaleźć ewentualny błąd w programie oraz łatwiej rozwijać program.

Jeśli chcesz użyć jakiejś funkcji, to powinieneś wiedzieć jakie zadanie wykonuje dana funkcja, jaki jest rodzaj wczytywanych argumentów i do czego są one potrzebne tej funkcji, oraz finalnie, jaki jest rodzaj zwróconych danych i co one oznaczają.

W programach w języku C jedna funkcja ma szczególne znaczenie - jest to `main()`. Funkcję tę, zwaną funkcją główną, musi zawierać każdy program (wyjątek: biblioteka). W niej zawiera się główny kod programu i przekazywane są do niej argumenty, z którymi wywoływany jest program (jako parametry `argc`, `argv` i `argp`). Więcej o funkcji `main()` dowiesz się później.

Język C, w przeciwieństwie do innych języków programowania (np. Fortrana czy Pascala), nie posiada żadnych słów kluczowych, które odpowiedzialne by były za obsługę wejścia i wyjścia. Może się to wydawać dziwne - język, który sam w sobie nie posiada podstawowych funkcji, może być językiem o ograniczonym zastosowaniu. Brak podstawowych funkcji wejścia-wyjścia jest jedną z największych zalet tego języka. Jego składnia opracowana jest tak, by można było bardzo łatwo przełożyć ją na kod maszynowy. To właśnie dzięki temu programy napisane w języku C są takie szybkie.

W 1983 roku, kiedy zapoczątkowano prace nad standaryzacją C, zdecydowano, że powinien istnieć zestaw procedur, stałych i zmiennych globalnych identycznych w każdej implementacji C. Zbiór ten nazwano **biblioteką standardową** (czasem nazywaną `libc`). Zawiera ona podstawowe funkcje, które umożliwiają wykonywanie takich zadań jak wczytywanie i zwracanie danych, modyfikowanie zmiennych łańcuchowych, działania matematyczne, operacje na plikach i wiele innych, jednak nie zawiera funkcji, które mogą być zbyt zależne od systemu operacyjnego czy sprzętu, jak grafika, dźwięk czy obsługa sieci (ale interfejs powłoki jest w niej uwzględniony). W programie "Hello World" użyto funkcji z biblioteki standardowej - `printf`, która wyświetla na ekranie sformatowany tekst.

<!-- TODO: Aktualny review skończył się tutaj. Będzie potrzebne kilka takich na ogarnięcie tej książki ;) -->

### Komentarze

Komentarze to tekst włączony do kodu źródłowego, który jest pomijany przez kompilator i służy jedynie dokumentacji. W języku C, komentarze zaczynają się od `/*` a kończą `*/`. Uwaga: Takich komentarzy nie można zagnieżdzać.

Komentarze mają duże znaczenie dla rozwijania oprogramowania, nie tylko dlatego że inni będą kiedyś potrzebowali przeczytać napisany przez ciebie kod źródłowy ale także możesz chcieć po dłuższym czasie powrócić do swojego programu i możesz zapomnieć, do czego służy dany blok kodu, albo dlaczego akurat użyłeś tego polecenia a nie innego. W chwili pisania programu, to może być dla ciebie oczywiste, ale po dłuższym czasie możesz mieć problemy ze zrozumieniem kodu. Jednak nie należy też wstawiać zbyt dużo komentarzy ponieważ wtedy kod może stać się jeszcze mniej czytelny - najlepiej komentować fragmenty, które nie są oczywiste dla programisty oraz te o szczególnym znaczeniu. Ale tego nauczysz się już w praktyce.

Styl pisania kodu jest o tyle ważny, że powinien on być czytelny i zrozumiały; po to w końcu wymyślono języki programowania wysokiego poziomu (w tym C), aby kod było łatwo zrozumieć. Należy stosować wcięcia dla odróżnienia bloków kolejnego poziomu (zawartych w innym bloku; podrzędnych), nawiasy klamrowe otwierające i zamykające blok powinny mieć takie same wcięcia, staraj się, aby nazwy funkcji i zmiennych kojarzyły się z zadaniem, jakie dana funkcja czy zmienna pełni w programie. W dalszej części książki możesz napotkać więcej porad dotyczących stylu pisania kodu. Staraj się stosować do nich, dzięki temu pisany przez ciebie kod będzie łatwiejszy do czytania i zrozumienia. 

#### Nazewnictwo

Jak już wcześniej było wspomniane, zmiennym i funkcjom powinno się nadawać nazwy, które odpowiadają ich znaczeniu. Zdecydowanie łatwiej jest czytać kod, gdy średnią liczb przechowuje zmienna srednia niż a, a znajdowaniem maksimum w ciągu liczb zajmuje się funkcja max albo znajdz_max niż nazwana f. Często nazwy funkcji to właśnie czasowniki.

W jakim języku należy pisać nazwy? Jeśli chcemy, by nasz kod mogły czytać osoby nieznające polskiego - warto użyć języka angielskiego. Bardzo istotne jest jednak, by nie mieszać języków. Przeplatanie ze sobą dwóch języków robi złe wrażenie.

Warto również zdecydować się na sposób zapisywania nazw składających się z więcej niż jednego słowa. Istnieje kilka możliwości, najważniejsze z nich to oddzielanie podkreśleniem: `int_to_str`,  "konwencja pascalowska", każde słowo dużą literą: `IntToStr`, "konwencja wielbłądzia", pierwsze słowo małą, kolejne dużą literą: `intToStr` lub łączenie słów i stosowanie skrótów: `itos`

Ponownie, najlepiej stosować konsekwentnie jedną z konwencji i nie mieszać ze sobą kilku.

Możesz też napotkać na swojej drodze notację węgierską, którą Ci odradzam - aktualnie, środowiska programistyczne podpowiedzą Ci, jakiego typu jest zmienna.

#### Preprocesor

Nie cały kod będzie zamieniany przez kompilator bezpośrednio na kod wykonywalny programu. W wielu przypadkach będziesz używać tzw. dyrektyw kompilacyjnych. Na początku procesu kompilacji, specjalny podprogram, tzw. preprocesor, wyszukuje wszystkie dyrektywy kompilacyjne i wykonuje odpowiednie akcje - które polegają na edycji kodu źródłowego (np. wstawieniu deklaracji funkcji, zamianie jednego ciągu znaków na inny). Właściwy kompilator, zamieniający kod C na kod wykonywalny, nie napotka już dyrektyw kompilacyjnych, ponieważ zostały one przez preprocesor usunięte, po wykonaniu odpowiednich akcji.

W C dyrektywy kompilacyjne zaczynają się od płotka (#). Przykładem najczęściej używanej dyrektywy, jest `#include`, która jest użyta nawet w tak prostym programie jak "Hello, World!". `#include` nakazuje preprocesorowi włączyć (ang. include) w tym miejscu zawartość podanego pliku, tzw. pliku nagłówkowego; najczęściej to będzie plik zawierający funkcje z którejś biblioteki standardowej (`stdio.h` - STandard Input & Output, rozszerzenie .h oznacza plik nagłówkowy C).

Nazwy zmiennych, stałych i funkcji mogą składać się z liter (bez polskich znaków), cyfr i znaku podkreślenia z tym, że nazwa taka nie może zaczynać się od cyfry. Nie można używać nazw zarezerwowanych przez język.

Konwencje nazewnictwa w C są dość proste.
Nazwy zmiennych piszemy małymi literami: `idx`, `file`.
Nazwy stałych (zadeklarowanych przy pomocy `#define`, lub zmiennych z modyfikatorem `const`) piszemy wielkimi literami: `SIZE`, `VERSION`.
Nazwy funkcji piszemy małymi literami: `print`, `sum`, `max`.

## Zmienne

### Definicja

Zmienna z definicji to pewien fragment pamięci o ustalonym rozmiarze, który posiada identyfikator oraz przechowuje pewną wartość, zależną od typu zmiennej.

### Deklarowanie zmiennych

Aby móc skorzystać ze zmiennej należy ją przed użyciem zadeklarować, to znaczy poinformować kompilator, jak zmienna będzie się nazywać i jaki typ ma mieć. Przykład:

```c
int a;
```

**Uwaga: Jeśli typ zmiennej to `int`, i jest to zmienna globalna, możemy pominąć `int`.**

Podczas deklaracji możemy też przypisać zmiennej wartość:

```c
int a = 42;
```

**W języku C zmienne deklaruje się na samym początku bloku (czyli przed pierwszą instrukcją).**

Niepoprawnie:

```c
{
   int a = 17;
   printf("%d", a);
   int b; /*Deklaracja po instrukcji, błąd!*/
   b = a;
}
```

Poprawnie:

```c
{
   int a = 17, b;
   printf("%d", a);
   b = a;
}
```

Zmienne tego samego typu można deklarować po przecinku. `int x, y;` == `int x; int y;`
**W C, nie są inicjalizowane zmienne lokalne. Oznacza to, że w nowo zadeklarowanej zmiennej znajdują się śmieci - to, co wcześniej zawierał przydzielony zmiennej fragment pamięci. Aby uniknąć błędów, dobrze jest inicjalizować wszystkie zmienne w momencie zadeklarowania.**

### Konwencje nazewnictwa zmiennych

Zasady nazywania zmiennych:
 * Pierwszy znak : litera lub _
 * Zakaz używania słów kluczowych (już są użyte). Po zmianie wielkości liter jednak będą dopuszczalne
 * Wielkość liter odróżnia nazwy, `odleglosc` i `Odleglosc` to dwie różne zmienne
 * Długość nazwy - niektóre kompilatory pozwalają na użycie do 247 znaków.
 
Przykłady niedopuszczalnych nazw:
```c
1x  //zaczyna się od cyfry
x+y //znak specjalny
char //słowo kluczowe
dodaj dwie liczby //spacje
```

Zmienne mogą być dostępne dla wszystkich funkcji programu - nazywamy je wtedy zmiennymi globalnymi. **Jeśli funkcja zwraca wartość int, możemy ominąć 'int'!**

```c
a,b;

void f(void) {
    a=3;
}
 
main(void) {
    b=3;
    a=2;
    return 0;
}
```

Uwaga: Zmienne globalne są domyślnie inicjalizowane zerem.

Zmienne, które funkcja deklaruje w swoim bloku nazywamy zmiennymi lokalnymi. Czy będzie błędem nazwanie tą samą nazwą zmiennej globalnej i lokalnej?. Nie. W danej funkcji da się używać tylko jej zmiennej lokalnej. Tej konstrukcji należy z wiadomych względów unikać.

```c
a = 1; 

main(void) {
    int a=2;
    printf("%d", a); /* =2 */
}
```

Czas życia zmiennych ilustruje poniższy przykład:

```c
main() {
 int a = 10;
 {
   int b = 10;
   printf("%d %d", a, b);
 }

 printf("%d %d", a, b);       /* Błąd, b już nie istnieje! */
}
```

Stała różni się od zmiennej tylko tym, że nie można jej przypisać innej wartości w trakcie działania programu. Wartość stałej ustala się w kodzie programu i nigdy ona nie ulega zmianie. Stałą deklaruje się z użyciem słowa kluczowego const w sposób następujący:

```
const typ nazwa_stałej=wartość;
```

Dobrze jest używać stałych w programie, ponieważ unikniemy wtedy pomyłek a kompilator może często zoptymalizować ich użycie (np. od razu podstawiając ich wartość do kodu).

```
const int START=5;
int i=START;
START=4;  /* Błąd. */
int j=START;
```

Dla komputera każdy obszar w pamięci jest taki sam - to zlepek bajtów, w takiej postaci zupełnie nieprzydatny dla programisty i użytkownika. Podczas pisania programu musimy wskazać, w jaki sposób ten ciąg ma być interpretowany.

Typ zmiennej wskazuje właśnie sposób, w jaki pamięć, w której znajduje się zmienna będzie wykorzystywana. Określając go przekazuje się kompilatorowi informację, ile pamięci trzeba zarezerwować dla zmiennej, a także w jaki sposób wykonywać na niej operacje.

Każda zmienna musi mieć określony swój typ w miejscu deklaracji i tego typu nie może już zmienić. Lecz co jeśli mamy zmienną jednego typu, ale potrzebujemy w pewnym miejscu programu innego typu danych? W takim wypadku stosujemy konwersję (rzutowanie) jednej zmiennej na inną zmienną. Rzutowanie zostanie opisane później, w następnym podrozdziale.

W języku C wyróżniamy następujące typy zmiennych (wg rozmiaru):

 * podstawowe:
   * char - jednobajtowe liczby całkowite;
   * int - typ całkowity o długości domyślnej dla danej architektury komputera (przeważnie 4 bajty);
   * float - typ zmiennoprzecinkowy, reprezentujący liczby rzeczywiste (4 bajty); Są dokładnie opisane w IEEE 754.
   * double - typ zmiennopozycyjny podwójnej precyzji (8 bajtów);
 * inne: typy złożone


Wg lokalizacji definicji typy dzielimy na wbudowane, które zna kompilator; oraz zdefiniowane przez użytkownika typy danych, które należy kompilatorowi opisać.

* int - typ przeznaczony jest do liczb całkowitych. Liczby te możemy zapisać na kilka sposobów, dziesiętnie: `12,42,15`, oktalnie: `044, 031` i heksadecymalnie: `0xFF, 0x1A, 0x80`.

* float - przeznaczony jest do liczb zmiennoprzecinkowych. Istnieją dwa sposoby zapisu, dziesiętny: `3.14, 1.72` i wykładniczy: `8e3, 9.2e-4`

* double ("podwójny") - przeznaczony dla liczb zmiennoprzecinkowych podwójnej precyzji. Oznacza to, że liczba zajmuje zazwyczaj w dwa razy więcej pamięci niż float, ale ma też dwa razy lepszą dokładność. Domyślnie ułamki wpisane w kodzie są typu double. Możemy to zmienić dodając na końcu literę "f":

```c
1.5f /* <-- float */
1.5  /* <-- double */
```

* char - typ znakowy umożliwiający zapis znaków w ASCII. Może być traktowany jako liczba z zakresu 0 do 255. Znaki zapisujemy w pojedynczych cudzysłowach, by odróżnić je od łańcuchów tekstowych. Przykład:   `'x'. '#'. ']'`. Pojedynczy cudzysłów zapisujemy jako '\'' a null (czyli zero, które kończy napisy) jako '\0' lub 0.

* void - można go używać tam, gdzie oczekiwana jest nazwa typu. void nie jest właściwym typem, bo nie można utworzyć zmiennej takiego typu; jest to "pusty" typ (ang. void = "pusty"). Typ void przydaje się do zaznaczania, że funkcja nie zwraca żadnej wartości lub że nie przyjmuje żadnych parametrów (więcej o tym później). Można też tworzyć zmienne typu "wskaźnik na void" 

* size_t - to typ zdefiniowany w nagłówku `stddef.h`, jako alias do liczby całkowitej bez znaku. Użycie size_t może poprawić przenośność i czytelność kodu. Czym dokładnie jest size_t? Sprawdźmy to:

```sh
$ echo | gcc -E -xc -include 'stddef.h' - | grep size_t #=> typedef unsigned int size_t;
```

Specyfikatory to takie słowa kluczowe, które zmieniają znaczenie typu danych.

Jak komputer może przechować liczbę ujemną. W przypadku przechowywania liczb ujemnych musimy w zmiennej przechować jeszcze jej znak. Jak wiadomo, zmienna składa się z szeregu bitów. W przypadku użycia zmiennej pierwszy bit z lewej strony (nazywany także bitem najbardziej znaczącym) przechowuje znak liczby. Efektem tego jest spadek "pojemności" zmiennej, czyli zmniejszenie największej wartości, którą możemy przechować w zmiennej.

Signed oznacza liczbę ze znakiem, unsigned - bez znaku (nieujemną). Mogą być zastosowane do typów: char i int i łączone ze specyfikatorami short i long (gdy ma to sens).

```c
signed char a;      /* -128 do 127 */
unsigned char b;    /* 0 do 255    */
```

Jeśli przy signed lub unsigned nie podamy typu, kompilator przyjmie wartość domyślną - int. 
Jeżeli nie podamy żadnego ze specyfikatora wtedy liczba jest domyślnie przyjmowana jako signed (uwaga: nie dotyczy `char`!).
Liczby bez znaku pozwalają nam zapisać większe liczby przy tej samej wielkości zmiennej - ale trzeba uważać, by nie zejść z nimi poniżej zera - wtedy "przewijają" się na sam koniec zakresu, co może powodować błędy w programach. 

Short i long są wskazówkami dla kompilatora, by zarezerwował dla danego typu mniej (lub więcej) pamięci. Mogą być zastosowane do dwóch typów: int i double (tu tylko long), mając różne znaczenie.
Jeśli przy short lub long nie napiszemy, o jaki typ nam chodzi, kompilator przyjmie domyślny typ - int.
Należy pamiętać, że to jedynie życzenie wobec kompilatora - w niektórych kompilatorach typy int i long int mają ten sam rozmiar. Standard języka C nakłada jedynie na kompilatory następujące ograniczenia:

int:
    > 16 bitów
    >= short
    < long;
    
short int: > 16 bitów;
long int: > 32 bity;

`volatile` (ang. ulotny) - kompilator wyłączy dla takiej zmiennej różne optymalizacje, za to wygeneruje kod, który będzie odwoływał się zawsze do komórek pamięci danego obiektu. Zapobiegnie to błędowi, gdy obiekt zostaje zmieniony przez część programu, która nie ma zauważalnego dla kompilatora związku z danym fragmentem kodu lub nawet przez zupełnie inny proces. Modyfikator `volatile` jest rzadko stosowany i przydaje się w wąskich zastosowaniach, jak współbieżność, współdzielenie zasobów i przerwania systemowe.

Zmienna, której będziemy używać w swoim programie bardzo często, może mieć nadany modyfikator `register`. Kompilator może wtedy umieści zmienną w rejestrze, do którego ma szybki dostęp, co przyśpieszy odwołania do tej zmiennej. Nie mamy żadnej gwarancji, że zmienna tak zadeklarowana rzeczywiście się tam znajdzie, chociaż dostęp do niej może zostać przyspieszony w inny sposób.

`static` pozwala na zdefiniowanie zmiennej statycznej. Statyczność polega na zachowaniu wartości pomiędzy kolejnymi definicjami tej samej zmiennej. Jest to przydatne w funkcjach. Gdy zdefiniujemy zmienną w ciele funkcji, to zmienna ta będzie od nowa definiowana wraz z domyślną wartością (jeżeli taką podano). W wypadku zmiennej określonej jako statyczna, jej wartość się nie zmieni przy ponownym wywołaniu funkcji.

`extern` oznacza zmienne globalne zadeklarowane w innych jednostkach - informujemy kompilator, żeby nie szukał jej w aktualnym pliku.

`typedef` to słowo kluczowe, służące do definiowania typów pochodnych np.:

```
typedef stary_typ nowy_typ;
typedef int myint;
```

od tej pory można używać typu myint zamiast int.
Często używa się typedef w jednej instrukcji razem z definicją typu .

## Operatory

W języku C wyróżniamy następujące operatory:
 * **Przypisania**, przypisuje wartośc prawego argumentu lewemu. Np: `int a = 42, b; b = a;`. Tego operatora można użyć kaskadowo, np: `int a, b, c; a = b = c = 84;`
 * **a #= b (#: `+, -, *, /, %, &, |, ^, <<, >>` => `a = a # (b)`)**, to skrócony zapis różnych operatorów. Przykład:
 
```c
int a = 1; a += 9;     /* a = a + 9; */
a *= a + 6; /* a = a * (a + 6); */
a %= 2;     /* a = a % 2; */
```

 * **Rzutowanie niejawne**, polega na zamianie jednego typu na inny bez wyraźnej deklaracji w kodzie. Przykład:
 
```c
int i = 42.7;            /* double -> int */
float f = i;             /* int -> float */
double d = f;            /* float -> double */
unsigned u = i;          /* int -> unsigned int */
f = 4.2;                 /* double -> float */
i = d;                   /* double -> int */
char *str = "foo";       /* const char* -> char* (const x -> x nie jest dopuszczalne, ale ciągi stanowią wyjątek) */
const char *cstr = str;  /* char* -> const char* */
void *ptr = str;         /* char* -> void* */
```

* **Rzutowanie jawne**, to zamiana z jednego typu na inny z wyraźną deklaracją w kodzie. Przykład:

```c
float d = 3.14; /* rzutowanie niejawne, double -> float */
int pi = (int)d; /* rzutowanie jawne, float -> int */
pi = (unsigned)pi * 2; /* rzutowanie jawne, signed -> unsigned */
```
 
 * **Artmetyczne**. **Uwaga**: W arytmetyce komputerowej nie działa prawo łączności oraz rozdzielności. Wynika to z ograniczonego rozmiaru zmiennych, które przechowują wartości. C definiuje następujące operatory arytmetyczne:
  * dodawanie ("`+`"),
  * odejmowanie ("`-`"),
  * mnożenie ("`*`"),
  * dzielenie ("`/`"),
  * modulo ("`%`") określone tylko dla liczb całkowitych.
  
  Przykłady:
```c
int a=7, b=2;
printf ("%d\n", a % b); /* => 1 */

float a = 7 / 2;
printf("%f\n", a); /* => 3, Wynik operacji jest takiego typu, jak największy z argumentów. Oznacza to, że operacja wykonana na dwóch liczbach całkowitych nadal ma typ całkowity nawet jeżeli wynik przypiszemy do zmiennej rzeczywistej.*/
```
  
 * **Operatory dodawania i odejmowania** są określone również, gdy jednym z argumentów jest wskaźnik, a drugim liczba całkowita. Ten drugi jest także określony, gdy oba argumenty są wskaźnikami. Aby skrócić zapis wprowadzono dodatkowe operatory: inkrementacji ("++") i dekrementacji ("--"), które dodatkowo mogą być pre- lub postfiksowe:
 
 ```c
 int a, b, c;
 a = 3;
 b = a--; /* b=3 a=2 */
 c = --b; /* b=2 c=2 */
 c = b++; /* b=3 c=2 */
 a = ++b; /* b=4 c=4 */
 ```
 
Czasami użycie operatorów postfiksowych jest nieco mniej efektywne (kompilator musi stworzyć zmienną tymczasową).
 * Oprócz podstawowych operacji matematycznych, język C został wyposażony także w **operatory bitowe**, zdefiniowane dla liczb całkowitych. Są to:

  * negacja bitowa (NOT)("~"),
  * koniunkcja bitowa (AND)("&"),
  * alternatywa bitowa (OR)("|") i
  * alternatywa rozłączna (XOR) ("^").

Działają one na poszczególnych bitach przez co mogą być szybsze od innych operacji. Działanie tych operatorów można zdefiniować za pomocą poniższych tabel:

```
 "~" | a     "&" | a | b     "|" | a | b     "^" | a | b
-----+---   -----+---+---   -----+---+---   -----+---+---
  0  | 1      0  | 0 | 0      0  | 0 | 0      0  | 0 | 0
  1  | 0      0  | 1 | 0      1  | 1 | 0      1  | 1 | 0
              0  | 0 | 1      1  | 0 | 1      1  | 0 | 1
              1  | 1 | 1      1  | 1 | 1      0  | 1 | 1
```
 * **Przesunięcie bitowe** operatory te przesuwają one w danym kierunku bity lewego argumentu o liczbę pozycji podaną jako prawy argument:
 
```c
   a  | a<<1 | a<<2 | a>>1 | a>>2
------+------+------+------+------
 0001 | 0010 | 0100 | 0000 | 0000
 0011 | 0110 | 1100 | 0001 | 0000
 0101 | 1010 | 0100 | 0010 | 0001
 1000 | 0000 | 0000 | 1100 | 1110
 1111 | 1110 | 1100 | 1111 | 1111
 1001 | 0010 | 0100 | 1100 | 1110
```

```c
#include <stdio.h>

main(void) {
    printf("6<<2=%d\n", 6<<2);  /* => 24 */
    printf("6>>2=%d\n", 6>>2);  /* => 1 */
    return 0;
}
```

 * **Operatory porównania**. W C występują następujące operatory porównania:
    * równe ("=="),
    * różne ("!="),
    * mniejsze ("<"),
    * większe (">"),
    * mniejsze lub równe ("<="),
    * większe lub równe (">=").

Wykonują one odpowiednie porównanie swoich argumentów i zwracają jedynkę jeżeli warunek jest spełniony lub zero jeżeli nie jest.

```c
#include <stdio.h>
int main(void) {
    if (2 == 3)
        printf("Rowne\n");
    else
        printf("Nie rowne\n");
    return 0;
}
```

 * **Operatory logiczne**. Analogicznie do części operatorów bitowych, definiuje się operatory logiczne takie jak:
   * negację: `!`
   * koniunkcję: `&&`
   * alternatywę: `||`

Działają one bardzo podobnie do operatorów bitowych, jednak zamiast operować na poszczególnych bitach, biorą pod uwagę wartość logiczną argumentów.

W przypadku operatorów logicznych nie używamy specjalnego typu danych do operacji logicznych. Można je stosować do liczb (np. typu int), tak samo jak operatory bitowe albo arytmetyczne. Operatory logiczne w wyniku dają zawsze albo 0 albo 1. 

 * **Operator wyrażenia warunkowego** to operator ?:. Jest to jedyny operator w tym języku przyjmujący trzy argumenty.

```c
a ? b : c
```

Jego działanie wygląda następująco: najpierw oceniana jest wartość logiczna wyrażenia a; jeśli jest ono prawdziwe, to zwracana jest wartość b, jeśli natomiast wyrażenie a jest nieprawdziwe, zwracana jest wartość c.

Praktyczne zastosowanie - znajdowanie większej z dwóch liczb:

```c
a = (b>=c) ? b : c;     /* Jeśli b jest większe bądź równe c, to zwróć b. 
                           W przeciwnym wypadku zwróć c. */
```

Wartości wyrażeń są przy tym operatorze obliczane tylko jeżeli zachodzi taka potrzeba, np. w wyrażeniu `1 ? 1 : f()` funkcja f() nie zostanie wywołana. 

 * **Operator przecinka** powoduje on obliczanie wartości wyrażeń od lewej do prawej po czym zwrócenie wartości ostatniego wyrażenia. W zasadzie, w normalnym kodzie programu ma on niewielkie zastosowanie, gdyż zamiast niego lepiej rozdzielać instrukcje zwykłymi średnikami. Używa się go jednak w instrukcji sterującej for. 

 * **Operator sizeof** zwraca rozmiar w bajtach podanego typu lub podanego wyrażenia. Ma on dwa rodzaje: `sizeof(typ)` i `sizeof wyrazenie`. Przykładowo:
 
 ```c
 printf("sizeof(int) = %d", sizeof(int));
 ```
 
 Operator ten jest często wykorzystywany przy dynamicznej alokacji pamięci, co zostanie opisane w rozdziale poświęconym wskaźnikom.

Pomimo, że w swej budowie sizeof przypomina funkcję, to jednak nią nie jest. Wynika to z trudności w implementacji takowej funkcji - jej specyfika musiałaby odnosić się bezpośrednio do kompilatora. Ponadto jej argumentem musiałby być typ, a nie zmiennq. W C nie jest możliwe przekazywanie typu jako argumentu. Często zdarza się, że rozmiar zmiennej musi być wiadomy jeszcze w czasie kompilacji - to ewidentnie wyklucza implementację sizeof() jako funkcji. Wynikiem operatora sizeof jest zmienna typu size_t.

 * **Inne operatory**:
  * operator `[]` - później opisany przy tablicach;
  * jednoargumentowe operatory `*` i `&` później opisane przy wskaźnikach;
  * operatory `.` i `->` później opisane przy strukturach;
  * operator `()` - wywołania funkcji,
  * operator `()` grupujący wyrażenia.

Priorytet operatorów:

```
1. nawiasy
2. jednoargumentowe przyrostkowe: [] . -> wywołanie funkcji postinkrementacja postdekrementacja
3. jednoargumentowe przedrostkowe: ! ~ + - * & sizeof preinkrementacja predekrementacja rzutowanie
4. * / %
5. + -
6. << >>
7. < <= > >=
8. == !=
9. &
10. ^
11. |
12. &&
13. ||
14. ?:
15. operatory przypisania
16. ,
```

## Sterowanie przebiegiem programu

C jest językiem imperatywnym - oznacza to, że instrukcje wykonują się jedna po drugiej w takiej kolejności w jakiej są napisane. Aby móc zmienić kolejność wykonywania instrukcji potrzebne są instrukcje sterujące. 

Instrukcja sterująca `if` wygląda tak:

```c
if (statement) {
   /* blok wykonany, jeśli statement jest prawdziwe */
}
/* ... */
```

Istnieje możliwość reakcji na nieprawdziwość wyrażenia - należy wtedy użyć słowa kluczowego else:

```c
if (x) {
   /* blok wykonany, jeśli x jest prawdziwe */
} else {
   /* blok wykonany, jeśli x nie jest prawdziwe */
}
```

Popatrzmy teraz na bardziej praktyczne zastosowanie tej konstrukcji:

```c
#include <stdio.h>
 
int main (void) {
   int a, b;
   a = 4;
   b = 6;
   if (a == b) {
     printf ("a = b\n");
   } else {
     printf ("a =/= b\n");
   }
   return 0;
}
```

Stosowany jest też krótszy zapis warunków logicznych, korzystający z tego, jak C rozumie prawdę i fałsz, liczba całkowita różna od zera oznacza prawdę a liczba całkowita równa zero oznacza fałsz. Przykład:

Jeśli zmienna a jest typu int, zamiast:

```c
if (a == 1) puts("Tak!");
```

Można to zrobić tak:

```c
if(a) puts("Tak!");
```

Czasami można użyć operatora wyrażenia warunkowego zamiast konstrukcji `if`:

```c
if (a)
   b = 1/a;
else
   b = 0;
```

=>

```c
b = a ? 1/a : 0;
```

Aby ograniczyć wielokrotne stosowanie instrukcji if możemy użyć switch.

```c
switch (statement) {
   case wartosc1: /* instrukcje, jeśli statement == wartosc1 */
     break;
   case wartosc2: /* instrukcje, jeśli statement == wartosc2 */
     break;
   default: /* instrukcje, jeśli żaden z wcześniejszych warunków nie został spełniony */
     break;
}
```

Należy pamiętać o użyciu instrukcji break po zakończeniu listy instrukcji następujących po case. Jeśli tego nie zrobimy, program przejdzie do wykonywania instrukcji z następnego case. Przykład błędu spowodowanego tym:

```
#include <stdio.h>

a,b;

main (void) {
    printf ("A=");
    scanf ("%d", &a);
    printf ("B=");
    scanf ("%d", &b);
    switch (b) {
        case  0: printf ("Nie mozna dzielic przez zero.\n"); /* !!! break; */
        default: printf ("a/b=%d\n", a/b); /* Potencjalny błąd, dzielenie przez zero! */
    }
    return 0;
}
```

Czasami jednak, nie-dodawanie break jest zamierzone (fall-through):

```
#include <stdio.h>

main (void) {
    int a = 4;
    switch (a%2) {
        case 0:
            printf ("Liczba %d dzieli się przez 2\n", a);
            break;
        case -1:
        case 1:
            printf ("Liczba %d nie dzieli się przez 2\n", a);
            break;
    }
    return 0;
}
```

Czasami zdarza się że program musi wykonać wielokrotnie jakiś określony fragment kodu.
Do tego możemy użyć tzw. pętli. Pętla while wykonuje się jeśli warunek jest prawdziwy.

```
main(void) {
    while(warunek) {
        /* ... */
    }
}
```

Działający przykład pętli while:

```
#include <stdio.h>

main (void) {
    int a = 1;
    while (a <= 5) { /* zapętlaj dopóki a nie przekracza 5 */
        printf ("%d^2=%d\n", a, a*a); /* wypisz a^2 */
        a++; /* inkremenacja a */
    }
    return 0;
}
```

Od instrukcji while czasami wygodniejsza jest instrukcja for. Umożliwia ona wpisanie ustawiania zmiennej, sprawdzania warunku i inkrementowania zmiennej w jednej linijce co często zwiększa czytelność kodu. Szkielet instrukcji for jest następujący:

```
for (expr1; expr2; expr3) {
    expr4;
}
expr5;
```

Zasada działania tej pętli jest podobna w językach takich jak Java lub C#, więc jeśli znasz już te języki, możesz pominąć mały opis poniżej.
expr1 to wyrażenie wykonywane przed pierwszym przebiegiem pętli. Przeważnie jest inicjalizacją zmiennej, lub po prostu go nie ma. Uwaga: w środku expr1 **nie można** deklarować zmiennych, tak samo jak w środku jakiegokolwiek bloku.
expr2 to wyrażenie pełniące taką samą funkcję jak warunek pętli while.
expr3 to instrukcja wykonywana **po każdym** przejściu pętli.
expr4 to kod który będzie znajdował się w pętli.
expr5 to kod który może wykonać się tuż po expr3.

Jeśli w pętli for nie ma instrukcji continue, możemy ją (ale nie musimy, a nawet nie powinniśmy) zastąpić ją pętlą while w taki sposób:

```
{
   expr1;
   while (expr2) {
     expr4;
     expr3;
   }
   expr5;
 }
```

Ważną rzeczą jest tutaj to, żeby zrozumieć i zapamiętać jak tak naprawdę działa for. Początkującym programistom nieznajomość tego faktu może sprawić wiele problemów.
W pierwszej kolejności w pętli for wykonuje się expr1. Wykonuje się ono zawsze, nawet jeżeli warunek przebiegu pętli jest od samego początku fałszywy.
Po wykonaniu expe1, for sprawdza warunek zawarty w expr2, jeżeli jest on prawdziwy (!= 0), to wykonywane jest expr4, czyli najczęściej to co znajduje się między klamrami, lub gdy ich nie ma, następna **pojedyncza** instrukcja. W szczególności musimy pamiętać, że sam średnik też jest instrukcją (którą można porównać do `NOP`-no operation/tł. brak operacji, w Assemblerze).
Gdy expr4 zostanie już wykonane, expr3 zostaje wykonane. Należy zapamiętać, że expr3 zostanie wykonane, nawet jeżeli była to już ostatnia iteracja pętli pętli. Poniższe 3 przykłady pętli for w rezultacie dadzą ten sam wynik. Wypiszą na ekran liczby od 5 do 10.

```
int i;

/* #1 - nawiasy klamrowe nie są potrzebne, aczkolwiek mogą się tu znajdować */
for(i = 5; i <= 10; ++i) {
    printf("%d ", i);
}

/* #2 - wersja bez nawiasów klamrowych której używam w większości przypadków */
for(i = 5; i <= 10; ++i) 
    printf("%d ", i);

/* #3 - przykład bardziej ilustrujący użycie expr3 do wykonania działań pętli, nie zalecam używania go. */
for(i = 5; i <= 10; printf("%d ", i++));
```

Jeśli chodzi o expr3 w pętli, jest z nią związana najważniejsza rzecz którą wyjaśnię.
Sam często robię to w **zły** sposób, bo jest mi tak wygodnie.
Moim pierwszym językiem był C++, z którego pozostało mi kilka zwyczajów jeśli chodzi o kod źródłowy.
Jednym z takich zwyczajów było używanie `i++` - postinkrementacji. (Uwaga: to wcale nie jest tak, że w C++ cały czas używamy postinkrementacji - kurs z którego się uczyłem, zalecał używanie `i++`)
**Nie powinno się tak robić**. Post-inkrementacja powoduje, że tworzony jest obiekt tymczasowy, który jest zwracany jako wynik operacji (choć wynik ten nie jest nigdzie czytany). Jedno kopiowanie liczby do zmiennej tymczasowej nie jest drogie, ale w pętli takie kopiowanie odbywa się po każdym przebiegu pętli. Dodatkowo, w C++ podobną konstrukcję stosuje się do obiektów - kopiowanie obiektu może być już czasochłonną czynnością. Dlatego w pętli for teoretycznie należy stosować wyłącznie ++i.
Pisząc absolutnie wszystko w tym repozytorium, miałem w głowie jedną ważną myśl - żeby dać mojemu czytelnikowi wybór.
Jeśli gdziekolwiek - na forum, pod postem na grupie, ktokolwiek używa postinkrementacji w pętli - **to nie znaczy że kod jest nieprawidłowy**. Po prostu będzie mniej lub bardziej niewydajny. Większość kodu (na nieszczęście) pisze się wyłącznie dla wygody programisty, zamiast pod wydajność w mniej lub bardziej krytycznych miejscach.
Jeśli myślisz że zamienienie i++ i ++i nie ma prawie żadnych konsekwencji - mylisz się, i często trzeba przystosowywać warunek i treść pętli do używanego typu inkrementacji lub dekrementacji. (Uwaga: jeśli będziesz używać konstrukcji typu ++i+i++, nie licz na to że będziesz mieć kolegów w świecie programowania)

Większość pętli ma to do siebie, że może nie wykonać się ani razu. Jak temu zapobiec? Użycie for/while ze zmienionym warunkiem aby pętla wykonała się conajmniej raz nie jest dobrym pomysłem. Osobiście, rzadko spotykam pętle do..while, aczkolwiek może ona się przydać Tobie.

Składnia jest bardzo intuicyjna:

```
do {
    expr2;
} while(expr1);
expr3;
```

W tym przykładzie, expr1 jest warunkiem pętli, expr2 jest kodem zawartym w środku pętli, a expr3 jest kodem który wykona się na końcu pętli.

Przykład pętli do..while, oblicza kwadrat liczb od 1 do 10:

```
int a;
do printf ("%d ", a*a); while (++a <= 10);
```

Instrukcja break pozwala opuścić każdą pętlę w dowolnym momencie:

```
int a;
for (a = 1; a != 9; ++a) {
    if (a == 5) break;
    printf ("%d\n", a);
}
```

I tutaj pojawia się problem. Jak wyjść z takiej konstrukcji?

```
while(...) {
    for(...) {
        if(i == 49) /*Tu należy przerwać pętlę*/
    }
}
```

Najprościej takich konstrukcji jest po prostu, unikać.
Jeśli jednak taka się zdarzy - sam nie wiem, co może się stać jeśli użyję break. Jeśli już taka sytuacja się przydarzy, trzeba użyć instrukcji goto. Instrukcji goto używa się następująco:

```
etykieta:
/* ... */
goto etykieta; /* Skoczy do kodu w miejscu `etykieta`, i rozpocznie jego wykonywanie */
```

Instrukcje goto mogą komplikować program i generować różne bliżej nie określone błędy, więc NIE zalecam ci ich używania w kontekście innym niż ten powyżej. Poprawiony program wygląda tak:

```
while(...) {
    for(...) {
        if(i == 49) goto label;
    }
}
label:
```

Kompilator GCC w wersji >=4.0 jest uczulony na etykiety zamieszczone przed nawiasem klamrowym, zamykającym blok instrukcji. Niedopuszczalne jest umieszczanie etykiety zaraz przed klamrą, która kończy blok instrukcji, zawartych np. w pętli for. Można natomiast stosować etykietę przed klamrą kończącą daną funkcję. 

Czasami spotykamy się z pętlami które nie mają wyraźnego końca. Przykład:

```
while(1) {}
for(;;) {}
```

Takie pętle są używane stosunkowo często w programowaniu. Uwaga:

```
int a = 0;
for(;;) {
   if(a <= 10) break;
   a++;
}
```

Taki kod absolutnie **nie ma** prawa istnienia i używanie tej "techniki" może być ruinujące jakość kodu.

W przeciwieństwie do break, która przerywa wykonywanie pętli instrukcja continue powoduje przejście do następnej iteracji, o ile tylko warunek pętli jest spełniony. Pętla poniżej dla każdej wartości większej niż 5 nie wyświetli "A" i wykona 10 pełnych iteracji:

```
int i;
for (i = 0 ; i < 10 ; ++i) {
    printf("A");
    if (i > 5) continue ;
    printf("B");
}
```

## Instrukcje wejścia-wyjścia

W C do komunikacji z użytkownikiem służą odpowiednie funkcje. Używając funkcji, nie musimy wiedzieć, w jaki sposób komputer wykonuje jakieś zadanie, interesuje nas tylko to, co ta funkcja robi. Funkcje niejako "wykonują za nas część pracy", ponieważ nie musimy pisać być może setek lub tysięcy linijek kodu, żeby np. wypisać tekst na ekranie (wbrew pozorom - kod funkcji wyświetlającej tekst na ekranie jest dość skomplikowany). Jeszcze taka uwaga - gdy piszemy o jakiejś funkcji, zazwyczaj lecz nie zawsze podając jej nazwę dopisujemy na końcu nawias. 

W pierwszym przykładzie została użyta jedna z dostępnych funkcji wyjścia, printf(). Z punktu widzenia swoich możliwości jest to jedna z bardziej skomplikowanych funkcji, a jednocześnie jest jedną z częściej używanych jeśli chodzi o wyjście. Przyjrzyjmy się ponownie kodowi pierwszego programu. 

```
#include <stdio.h>
 
main(void) {
    printf("Hello World!");
}
```

Prototyp funkcji printf() wygląda tak:

```
printf(const char * format, ...);
```

`...` oznacza, że po parametrze format możemy przekazać jeszcze inne argumenty, których typ nie jest do końca określony.
printf() korzysta z tych argumentów, aby wyświetlać dane na terminal które przeplatają się np z liczbami lub innymi ciągami wewnątrz. Przykład:

```
int i = 2;
printf("Liczbami całkowitymi są m.in. %d i %d.\n", 1, i);
```

Format to napis ujęty w cudzysłowy, który określa ogólny kształt tego, co ma być wyświetlone. Format jest drukowany tak, jak go napiszemy, jednak niektóre znaki zostaną w nim zmienione na co innego. Przykładowo, ciąg `\n` (będący technicznie jednym znakiem) jest zamieniany na znak nowej linii (patrz niżej). Natomiast procent jest zmieniany na jeden z argumentów znajdujących się po ciągu formatu. Po procencie następuje specyfikator, instruujący printf(), jak wyświetlić dany argument. W tym przykładzie %d (od decimal) oznacza, że argument ma być wyświetlony jak liczba całkowita. W związku z tym, że \ i % mają specjalne znaczenie, aby wydrukować je, należy użyć ich podwójnie (%%, \\).

Uwaga: Jeśli jesteś już doświadczonym programistą, prawdopodobnie wiesz że w sensie technicznym to kompilator podmienia sobie '\n' na znak nowej linii, ale dla zachowania prostoty mojego wywodu, porównuję % i \.

Czasami możemy się pomylić:

```
printf("%i %s %i", 21, 37, "blablabla"); /* właściwie: "%i %i %s" */
```

Zachowanie printf() w tym momencie można uznać za niezdefiniowane.
Przy włączeniu ostrzeżeń (opcja -Wall lub -Wformat w GCC) kompilator powinien nas ostrzec, gdy format nie odpowiada podanym elementom. Kompilatory aktualnie są na tyle mądre że potrafią czasami "optymalizować" printf().

Funkcja printf() nie jest wyjątkową konstrukcją języka i łańcuch formatujący może być podany do funkcji jako zmienna. W związku z tym możliwa jest np. taka konstrukcja:

```
#include <stdio.h>

main(void) {
     char buf[10];
     scanf("%9s", buf); /* wczytaj ciąg znaków od użytkownika do zmiennej buf, będący długości conajwyżej 9 znaków. */
     printf(buf);
}
```

Jednak ten kod ma jeden bardzo poważny mankament. Jak już pamiętasz, jeśli podamy niewłaściwy format, np użytkownik wpisze %s kiedy taki ciąg nie został podany, może stać się coś niezbyt ciekawego. Aby temu zapobiec, musimy poprawić nasz kod:

```
#include <stdio.h>

main(void) {
     char buf[10];
     scanf("%9s", buf); /* wczytaj ciąg znaków od użytkownika do zmiennej buf, będący długości conajwyżej 9 znaków. */
     printf("%s", buf);
}
```

Najpopularniejsze formaty:

```
printf("%d", d); -> int
printf("%f", f); -> float/double
printf("%c", c); -> char
printf("%s", s); -> char * (lub char[])
```

Funkcja puts() przyjmuje jako swój argument ciąg znaków, który następnie bezmyślnie wypisuje na ekran kończąc go znakiem przejścia do nowej linii. W ten sposób, "Hello World" można zrobić tak:

```
#include <stdio.h>
 
main(void) {
    puts("Hello World!");
}
```

Możemy bardzo prosto zaimplementować swoją własną wersję puts:

```
int myputs(char * s) {
    for(;*s;s++)
        putchar(*s);
    putchar('\n');
}
```

Opisując funkcję fputs() przenosimy się do przyszłości (a konkretnie do rozdziału operacji na plikach), ale warto o niej wspomnieć teraz, gdyż umożliwia ona wypisanie argumentu bez wypisania na końcu znaku przejścia do nowej linii.

```
#include <stdio.h>
 
main(void) {
    fputs("Hello World!", stdount); /* stdout -> standard out; standardowe wyjście */
}
```

Jeszcze inna funkcja służąca do wypisywania danych na ekranie to putchar(). Ta funkcja powinna być już Ci znana, działa podobnie jak `printf("%c",x);`. Przykład jej użycia; wypisuje "w tabelce" liczby od 0 do 49:

```
#include <stdio.h>
main(void) {
   int i;
   for (i = 0; i < 50; ++i) {
       if (i % 10) {
           putchar(' ');
       }
       printf("%2d", i);
       if ((i % 10)==9)  {
         putchar('\n');
       }
   }
   return 0;
}
```
=>
```
 0  1  2  3  4  5  6  7  8  9
10 11 12 13 14 15 16 17 18 19
20 21 22 23 24 25 26 27 28 29
30 31 32 33 34 35 36 37 38 39
40 41 42 43 44 45 46 47 48 49
```

Znasz już podstawowe funkcje wyjścia, czas na funkcje wejścia.
Najprostszą funkcją będzie funkcja main() która może pobierać parametry, ale o tym później!

Rozpoczniemy od funkcji scanf(), która działa podobnie do printf(), ale odwrotnie. Przykład poniżej pobiera liczbę od użytkownika, i wyświetla jej kwadrat na ekranie:

```
#include <stdio.h>

main(void) {
    int a = 0;
    printf ("A=");
    scanf ("%d", &a);
    printf ("%d^2=%d\n", a*a); 
}
```

Jedyna linijka która może być problematyczna to ta:

```
scanf ("%d", &a);
```

Wykonujemy tutaj funkcję scanf. Pojawia się tu operator &.

Przy wykonaniu funkcji scanf jeśli podajemy jej parametr który nie jest typem wskaźnikowym (tzn. nie ma gwiazdki jak w `char *`) ani tablicą (tzn. nie ma nawiasów kwadratowych jak w `char[]`), dodajemy & na początku. Potem wyjaśnię Ci, czemu jest to niezbędne.

Brak & jest częstym błędem wśród początkujących programistów. Ponieważ funkcja scanf() akceptuje zmienną liczbę argumentów to nawet kompilator może mieć kłopoty z wychwyceniem takich błędów (standard nie wymaga od kompilatora wykrywania takich pomyłek), choć kompilator GCC radzi sobie z tym jeżeli podamy mu argument -Wformat który był wspomniany przy okazji printf().

Należy bardzo uważać podczas wczytywania ciągów. Na poniższym przykładzie przedstawię wrażliwy kod:

```
#include <stdio.h>
main(void) {
    char ciag[100];     /* 1 */
    scanf("%s", ciag);  /* 2 */
}
```

W linijce oznaczonej jako 1, deklarujemy nowy ciąg znaków o maksymalnej długości 99 (+tzw. null terminator, który kończy ciąg).
Druga linijka wczytuje natomiast nieokreśloną ilość znaków - to znaczy, że może wczytać ich 100. Potencjalne bum? Spróbuj sam.
Uruchom kod i wpisz tam trochę ponad 100 znaków, albo zmniejsz rozmiar tabalicy do 5 (`char ciag[5];`) zamiast odliczać znaki.

Poprawna wersja kodu powyżej wygląda następująco:

```
#include <stdio.h>
 
main(void) {
    char ciag[100];
    scanf("%99s", ciag);
    return 0;
}
```

Tutaj ustalamy, że wczytane może być tylko 99 znaków (rezerwujemy jeden na wspomniany wcześniej null terminator).

Funkcja scanf() zwraca również liczbę poprawnie wczytanych zmiennych lub EOF, jeżeli nie ma już danych w strumieniu lub nastąpił błąd. Załóżmy dla przykładu, że chcemy stworzyć program, który odczytuje po kolei liczby i wypisuje ich kwadraty. W pewnym momencie liczby się kończą lub jest wprowadzana liczba w niepoprawnym formacie, wtedy program powinien zakończyć działanie. Aby to zrobić, należy sprawdzać wartość zwracaną przez funkcję scanf() w warunku pętli. Przykład:

```
#include <stdio.h>

main(void) {
    int n;
    while (scanf("%d", &n) == 1)
        printf("%d\n", n*n);
}
```

Rozpatrzmy teraz trochę bardziej skomplikowany przykład. Otóż, ponownie jak poprzednio nasz program będzie wypisywał trzecią potęgę podanej liczby, ale tym razem musi ignorować błędne dane (tzn. pomijać ciągi znaków, które nie są liczbami) i kończyć działanie tylko w momencie, gdy nastąpi błąd odczytu lub koniec pliku (jeśli jakikolwiek został przekierowany do standardowego wejścia). Przykład zawiera kod funkcji main():

```
int result, n;
do  {
    result = scanf("%d", &n);
    if (result)
        printf("%d\n", n*n*n);
    else
        result = scanf("%*s");
} while (result != EOF);
```

Kolejna funkcja to gets(). Wczytuje ona jedną linię. Aczkolwiek jej używanie jest wysoce niezalecane, ponieważ może się wydarzyć to samo co podczas wykonania `scanf("%s", string);` - bufor może się przepełnić.

Bezpieczniejszy odpowiednik gets() to fgets(). Wspominałem wcześniej o fputs() - ta funkcja jest z tego samego zakresu.
Jej wykonanie wygląda następująco:

```
fgets(bufor, dlugosc, stdin);
```

Jedyna różnica to stały (ale tylko chwilowo, jak zabrniesz dalej z nauką C to zrozumiesz jego znacznie) parametr stdin, oraz zmienna określająca długość bufora, na której mam zamiar się skupić. Funkcja czyta tekst aż do napotkania znaku przejścia do nowej linii, który także zapisuje w wynikowej tablicy. Jeżeli brakuje miejsca w tablicy to funkcja przerywa czytanie, w ten sposób, aby sprawdzić czy została wczytana cała linia, czy tylko jej część należy sprawdzić czy ostatnim znakiem jest znak przejścia do nowej linii. Jeżeli nastąpił jakiś błąd lub na wejściu nie ma już danych funkcja zwraca wartość NULL (o niej dowiesz się więcej w rozdziale o wskaźnikach).

Przykład użycaia fgets():

```
char buf[128];
fputs("Jak sie nazywasz? > ", stdout);
fgets(buf, 126, stdin);
printf("Witaj %s!", buf);
```

## Funkcje

W matematyce pod pojęciem funkcji rozumiemy twór, który pobiera pewną liczbę argumentów i zwraca wynik (uwaga, nie chcę urazić potencjalnych matematyków więc pokojowo stwierdzę że nie znam się na tym). Jeśli dla przykładu weźmiemy funkcję cos(x) to x będzie zmienną rzeczywistą, która określa kąt, a w rezultacie otrzymamy inną liczbę rzeczywistą - cosinus tego kąta. 

W C funkcja (czasami nazywana głównie przez programistów +60 podprogramem) to wydzielona część programu, która przetwarza argumenty i ewentualnie zwraca wartość, która następnie może być wykorzystana jako argument w innych wyrażeniach lub funkcjach. Funkcja może posiadać własne zmienne lokalne.

Po lekturze poprzednich części tego dokumentu mógłbyś podać kilka funkcji, z których korzystałeś - printf, scanf, puts, fgets ...

```
for(i=1; i <= 10; ++i) {
  printf("%d ", i*i);
}
for(i=1; i <= 20; ++i) {
  printf("%d ", i*i);
} 
for(i=1; i <= 10; ++i) {
  printf("%d ", i*i);
}
```

Wyobraź sobie taki kod. Mógłbyś zmniejszyć jego rozmiar używając goto, zmiennych tymczasowych, etc...; jednak jest to WYSOCE niezalecane, i przez WYSOCE mam na myśli **niedopuszczalne**. Więc trzeba wymyślić inny sposób. I w tym momencie, przychodzą na pomoc twórcy ogólnej teorii programowania i programowania proceduralnego.

Zamiast powtarzać się w kodzie, zawsze lepiej jest zredukować jego rozmiar dzięki funkcjom. Innym, niemniej ważnym powodem używania funkcji jest rozbicie programu na fragmenty wg. ich funkcjonalności. Oznacza to, że jeden duży program dzieli się na mniejsze funkcje, które są "wyspecjalizowane" w wykonywaniu określonych czynności. Dzięki temu łatwiej jest zlokalizować błąd. Ponadto takie funkcje można potem przenieść do innych programów i ewentualnie przygotować bibliotekę, ale o tym później.

"Dobrze jest uczyć się na przykładach". To od początku mojej kariery w programowaniu było moim mottem - 

Rozważmy następujący kod:

```
suma (x, y) int x; int y; {
    int wynik;
    wynik = x + y;
    return wynik;
}
```

`int iloczyn (x,y) int x; int y;` to nagłówek funkcji, który opisuje, jakie argumenty przyjmuje funkcja i jaką wartość zwraca (funkcja może przyjmować wiele argumentów, lecz może zwracać tylko jedną wartość; można to bardziej sprecyzować, ale ta wiedza powinna wystarczyć Ci na początek). Na początku podajemy typ zwracanej wartości - u nas int; jeśli zwracanym typem ma być int, możemy go pominąć. Następnie mamy nazwę funkcji i w nawiasach listę argumentów po nazwach. Po nawiasach robimy "deklarację" tych parametrów używając typu, nazwy, i średnika. Jeśli funkcja nie przyjmuje parametrów, deklarujemy ją następująco: `funkcja(void)`.

Ciało funkcji (czyli wszystkie wykonywane w niej operacje) umieszczamy w nawiasach klamrowych. Pierwszą instrukcją w tej funkcji jest deklaracja zmiennej - jest to zmienna lokalna, czyli niewidoczna poza funkcją. Dalej przeprowadzamy odpowiednie działania i zwracamy rezultat za pomocą instrukcji return. 

Czasami możemy chcieć przed napisaniem funkcji poinformować kompilator, że dana funkcja istnieje. Kompilator nie wie, jakie funkcje są deklarowane w dalszej części pliku. Dlatego, musimy stworzyć jej prototyp. Prototypy przeważnie umieszcza się w plikach .h (stdio.h, stdlib.h ...), jednak w chwili obecnej będziemy chcieli zadeklarować funkcję w pliku .c:

```
int f2(int p);

f1(void) {
    return f2(6);
}

f2(p) int p; {
    return p==6?1:f1();
}
 
main(void) {
  return f2(1);
}
```

Jak możesz zauważyć, deklaracja prototypu wygląda trochę inaczej niż właściwa implementacja funkcji niżej. Taka deklaracja jest częściej spotykana, ponieważ pochodzi z nowszego standardu. Od tej chwili, zalecam Ci deklarować funkcje w taki właśnie sposób - to nie znaczy że musisz, masz wybór.

```
int f2(int p);
int f1(void) {
    return f2(6);
}
int f2(int p) {
    return p==6?1:f1();
}
int main(void) {
    return f2(1);
}
```

Jeszcze inną "nowością" jest usunięcie reguły implicit int. To znaczy, że w nowszych standardach musimy dodawać przedrostek int tam, gdzie go dotychczas nie dodawaliśmy. Przy zmiennych globalnych, które kiedyś deklarowaliśmy tak: `a,b;` będziemy musieli dodać przedrostek int: `int a,b;`. Tak samo w przypadku funkcji; `main(void)` teraz jest `int main(void)`.

Od tego momentu, zaczniemy stosować się do C99.

Innym bardzo częstym zwyczajem jest umieszczenie funkcji main() na samej górze pliku (w przeciwieństwie do ogólnie przyjętej konwencji umieszczania jej na dole aby zaoszczędzić konieczność deklaracji prototypów na samej górze pliku). Jeśli zechcielibyśmy umieścić main() na samej górze, kod wyglądałby tak:

```
int f1(void);
int f2(int p);

int main(void) {
  return f2(0);
}

int f1(void) {
  return f2(6);
}

int f2(int p) {
  return p==6?1:f1();
}
```

Osobiście nie mam "ulubionego stylu" i czasami używam pierwszego, ale dość rzadko drugiego.
Najważniejszą rzeczą jest to, żeby nie mieszać styli i konwencji.

Zauważyłeś zapewne, że używając funkcji printf() lub scanf() po argumencie zawierającym tekst z odpowiednimi modyfikatorami mogłeś podać praktycznie nieograniczoną liczbę argumentów. Zapewne deklaracja obu funkcji zadziwi Cię jeszcze bardziej:

```
/* Gdzieś w STDIO.H */
int printf(const char *format, ...);
int scanf(const char *format, ...);
```

Jak widzisz w deklaracji zostały użyte trzy kropki. Otóż język C ma możliwość przekazywania teoretycznie nieograniczonej liczby argumentów do funkcji (jedynym ograniczeniem jest rozmiar stosu programu).

Ważne jest to, aby umieć dostać się do odpowiedniego argumentu oraz poznać jego typ (używając funkcji printf, mogliśmy wpisać jako argument dowolny typ danych). Do tego celu możemy użyć wszystkich ciekawostek, zawartych w pliku nagłówkowym stdarg.h.

Załóżmy, że chcemy napisać prostą funkcję, która dajmy na to, mnoży wszystkie swoje argumenty (zakładamy, że argumenty są typu int). Przyjmujemy przy tym, że ostatni argument kończący mnożenie będzie równy 0. Będzie ona wyglądała tak: 

```
#include <stdarg.h>
 
int multiply(int a, ...) {
    va_list args;
    int result = 1, t;
    va_start(args, a);
    for (t = a; t; t = va_arg(args, int)) {
        result *= t;
    } 
    va_end(args);
    return result;
}
```

va_list to specjalny typ danych, w którym przechowywane będą argumenty przekazane do funkcji. va_start inicjuje args do dalszego użytku przez va_arg. Jako drugi parametr musimy podać nazwę ostatniego znanego argumentu funkcji (tj. tego przed kropkami). va_arg odczytuje kolejne argumenty i przekształca je na odpowiedni typ danych. Na zakończenie używane jest va_end - **wywołanie tego jest obowiązkowe**.

Wywołanie funkcji jest banalnie proste i powinieneś już je znać. Ogólny schemat wygląda tak:

```
nazwa (arg1, arg2, argn);
```

Jeśli chcemy przypisać zmiennej wartośc jaką zwraca funkcja, będzie to wyglądać następująco:

```
var = nazwa(arg1, arg2, argn);
```

Teraz, wyobraź sobie taką funkcję:

```
void message() {
  puts("Hello, World!");
}
```

I jej wywołanie:

```
message; /* źle */
message(); /* dobrze */
```

Funkcja również jest zmienną (do tego globalną), więc nie możemy jej "wywoływać" tak jak zmiennej:

```
int num = 42;
printf("%d", num);
```

I musimy dodać nawiasy aby wykonać funkcję:

```
int a = 2, b = 2;
printf("%d", add(a, b));
```

Z tego wynika, że możemy też wywoływać zmienne, ale najpierw musimy je rzutować na liczbę. Aboslutnie nie jest to poprawne, ale jest możliwe; aktualnie potraktuj to jako ciekawostkę:

```
int main(void) {
    int x = 0;
    ((void (*)(void))x)();
    return 0;
}
```

Próba wykonania tego programu zakończy się błędem "segmentation fault"; aczkolwiek na systemach bez ochrony pamięci możliwe byłoby kontynuowanie działania programu.
Jest szansa, że znajdziemy praktyczny użytek tej konstrukcji w dziale o programowaniu systemów operacyjnych.

Przykładowa funkcja wraz z wywołaniem wygląda tak:

```
#include <stdio.h>

int suma(int a, int b) {
    return a + b;
}
 
int main(void) {
    int m = suma (4, 5);
    printf ("4 + 5 = %d\n", m);
}
```

return to słowo kluczowe języka C, w przypadku funkcji służy ono do przerwania funkcji (i przejścia do następnej instrukcji w funkcji wywołującej) lub zwrócenia wartości. W przypadku procedur powoduje przerwania procedury bez zwracania wartości. Użycie tej instrukcji jest bardzo proste i wygląda tak:

```
return zwracana_wartość;
```

lub dla procedur (funkcji zwracających void; nic):

```
return;
```

Możliwe jest użycie kilku instrukcji return w obrębie jednej funkcji. Wielu programistów uważa jednak, że lepsze jest użycie jednej instrukcji return na końcu funkcji, gdyż ułatwia to śledzenie przebiegu programu.
Zwracana wartość

W C zwykle przyjmuje się, że 0 oznacza poprawne zakończenie funkcji:

```
return 0;
```

a wszelkie inne wartości oznaczają niepoprawne zakończenie:

```
return 1;
```

Ta wartość może być oczywiście wykorzystana przez inne instrukcje, np. if:

```

int funkcja(x) {
    return x - 1;
}

int main() {
    if(funkcja(1)) return 1;
    else return 0;
}

```

Możesz, oczywiście, zwracać więcej danych niż 1, aczkolwiek znacznie wybiega to za materiał który znasz (i poznasz niedługo):

```
#include <stdio.h>

typedef struct {
    int r1;
    float r2;
    char r3;
    char r4[100];
    double r5;
} rtype;

rtype f(void) {
    rtype ret = {9, 8.7, 'O', "Hello World", 65.432};
    return ret;
}

int main(void) {
    rtype data;
    data = f();
    printf("{%d, %f, %c, %s, %f}\n", data.r1, data.r2, data.r3, data.r4, data.r5);
}
```

Jeśli nie rozumiesz kodu powyżej - to nic złego. Do struktur przejdziemy nieco później i na chwilę obecną możesz potraktować to również jako ciekawostkę.

Kolejna metoda którą możemy zastosować to użycie wskaźników:

```
#include <stdio.h>

/* Niezbyt wydajna metoda */
void swap (int * a, int * b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main(void) {
    int x=4, y=2;
    printf("x=%d ; y= %d\n", x,y); 
    swap(&x, &y);
    printf("x=%d ; y= %d\n", x,y);
}
```

Uwaga: Wskaźniki, tablice, struktury - masz prawo nie wiedzieć co to. Dowiesz się tego w następnych rozdziałach. Jeśli będziesz znał już zasadę działania tablic, wskaźników i struktur - możesz wrócić tu spowrotem.

Tablice mogą być parametrami funkcji. Można je podawać jako wskaźniki, tablice z nieokreślonym rozmiarem i tablice z określonym rozmiarem. Przykłady kolejno wymienionych metod:

```
void funkcja(int *tab)    // Jako wskaźnik
void funkcja(int tab[5])  // Jako tablica z określonym rozmiarem
void funkcja(int tab[])   // Tablica z nieokreślonym rozmiarem.
```

Ponieważ tablica jako symbol jest wskaźnikiem do jej pierwszego elementu, możemy korzystać z tablic w sposób następujący:

```
#include<stdio.h>

void rarray(int tab[], int amount) {
    int idx;
    for(idx=0;idx<amount;idx++)
        scanf("%d",&tab[idx]);
    fflush(stdin);
}

void warray(int tab[], int amount) {
    int idx;
    for(idx=0;idx<amount;idx++)
        printf("%d, ",tab[idx]);
    printf("\n");
}

int main(void) {
    int tab[5];
    printf("Wprowadź 5 liczb: \n");
    rarray(tab,5);
    printf("Wprowadzone liczby to: \n");
    warray(tab,5);
}
```

Funkcje mają dostęp do wczytywania i zapisywania danych do tablicy.

Do tej pory we wszystkich programach istniała funkcja main(). Po co tak właściwie ona jest? Otóż jest to funkcja, która zostaje wywołana przez fragment kodu inicjującego pracę programu. Kod ten tworzony jest przez kompilator i nie mamy na niego wpływu. Istotne jest, że każdy program w języku C który w założeniu ma być wykonywalny musi zawierać funkcję main(). Istnieją trzy możliwe prototypy tej funkcji:

```
int main(void);
int main(int argc, char * argv[]);
int main(int argc, char * argv[], char * argp[]);
```

Argument argc jest liczbą nieujemną określającą, ile ciągów znaków przechowywanych jest w tablicy argv. Wyrażenie `argv[argc]` ma zawsze wartość NULL. Pierwszym elementem tablicy argv, czyli `argv[0]` jest nazwa programu (!) czy komenda, którą program został uruchomiony. Pozostałe przechowują argumenty podane przy uruchamianiu programu. 

Pewien fragment zaznaczyłem wykrzyknikiem - czy `argv[0]` ***jest*** nazwą programu?

Zgadywanki są fajne, ale musisz zajrzeć do standardu, aby się upewnić. Standard twierdzi:

    If the value of argc is greater than zero, the string pointed to by argv[0] represents the program name; argv[0][0] shall be the null character if the program name is not available from the host environment.

(tł. Jeśli wartość argc jest większa od zera, ciąg wskazywany przez `argv[0]` **reprezentuje** nazwę programu; `argv[0][0]` powinno być tzw. null-terminatorem, jeśli nazwa programu nie jest dostępna ze środowiska hosta programu.

`argv[0] jest tylko nazwą programu`, "reprezentuje" jego nazwę, więc niekoniecznie nią jest. Sekcja wcześniej twierdzi:

    If the value of argc is greater than zero, the array members argv[0] through argv[argc-1] inclusive shall contain pointers to strings, which are given implementation-defined values by the host environment prior to program startup.

(tł. Jeśli wartość argc jest większa od zera, elementy tablicy `argv[0]` aż do `argv[argc-1]` włącznie powinny być wskaźnikami
do ciągów, które są zależne od tych zdefiniowanych w implementacji przez środowisko hosta przed uruchomieniem programu)

Ten fragment jest dosłownie przekopiowany z standardu C99.

To znaczy, że nazwa programu może być pusta, jeśli system operacyjny takiej nie przyznał, i być czymkolwiek innym jeśli host coś dał. Właśnie, czymś innym. Jako programista systemu operacyjnego chętnie przygotowałbym easteregga który sprawia że `argv[0]` jest np. tłumaczone na Somalijski, zaszyfrowane, podane w odwrotnej kolejności bajtów...

Więc `argv[0]` może być nazwą programu lub komendą za pomocą której program został wywołany. Nie ufaj temu!

Jeśli program zostanie wywołany poleceniem `./a.out param1 param2`, argc będzie równe 3 (2 argumenty + `argv[0]`). Jak sprawdzić to na własne oczy? Bardzo prosto:

```
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char * argv[]) {
    int i;
    printf("argc=%d\b", argc);
    for (i = 0; i<argc; ++i) {
        printf("#%d: %s\n", i, argv[i]);
    }
    return EXIT_SUCCESS; /* !! */
}
```

Zwróć uwagę, że z funkcji main zwracamy wartość (jeśli nie zwrócimy, domyślnie zwracane jest 0). Spróbuj uruchomić aplikację z różnymi parametrami i zaobserwować rezultat! Uwaga: EXIT_SUCCESS i EXIT_FAILTURE to dwie zmienne które powinny być zwracane przez main() zależnie od tego, jak zakończyło się wykonanie programu.

main() jeśli chodzi o wykonywanie, niczym nie różni się od innych funkcji i też możemy ją wykonywać, jednak jest to niezalecane.

Poniżej przekażę Ci kilka bardziej zaawansowanych informacji o funkcjach w C, jeśli nie masz ochoty zagłębiać się w szczegóły, możesz spokojnie pominąć tę część i wrócić tu później. 

Język C ma możliwość tworzenia tzw. funkcji rekurencyjnych. Jest to funkcja, która w swojej własnej definicji (ciele) wywołuje samą siebie. Najbardziej klasycznym przykładem może tu być silnia. Funkcja rekurencyjna która oblicza silnię wygląda tak:

```
int fac(int n) {
    if (n<0) return 0;
    if (n==0||n==1) return 1;
    return n*fac(n-1);
}
```

Musimy być ostrożni przy funkcjach rekurencyjnych, gdyż łatwo za ich pomocą utworzyć funkcję, która będzie wywoływała sama siebie w nieskończoność, a co za tym idzie będzie zawieszać program. Tutaj pierwszymi instrukcjami if ustalamy "warunki stopu", gdzie kończy się wywoływanie funkcji przez samą siebie, a następnie określamy, jak funkcja będzie wywoływać samą siebie (odjęcie jedynki od argumentu, co do którego wiemy, że jest dodatni, gwarantuje, że dojdziemy do warunku stopu w skończonej liczbie iteracji).

Warto też zauważyć, że funkcje rekurencyjne czasami mogą być znacznie wolniejsze niż podejście nierekurencyjne (iteracyjne, przy użyciu pętli). Flagowym przykładem może tu być funkcja obliczająca wyrazy ciągu Fibonacciego.

```
int count;

unsigned fibr(unsigned n) {
    ++count;
    return n<2 ? n : (fibr(n-2) + fibr(n-1));
}

unsigned fibi (unsigned n) {
    unsigned a = 0, b = 0, c = 1;
    ++count;
    if (!n) return 0;
    while (--n) {
        ++count;
        a = b;
        b = c;
        c = a + b;
    }
    return c;
}
```

W języku C nie można tworzyć zagnieżdżonych funkcji (funkcji wewnątrz innych funkcji). 

Jeśli nie podamy żadnych parametrów funkcji, to funkcja będzie używała zmiennej liczby parametrów. Aby wymusić pustą listę argumentów, należy użyć `int f(void)` (tylko prototypy / deklaracje).

Jeśli nie użyjemy w funkcji instrukcji return, wartość zwracana będzie przypadkowa.

Nie jest możliwe przekazywanie typu jako argumentu.

## Obsługa plików

Wcześniej wspominaliśmy funkcje f* (np. fgets(), fputs() ...). Zawsze podawaliśmy stdout lub stdin. Jest też stderr, pozwalające zapisywać dane do strumienia błędu (tam raportujemy błędy). Teraz nauczę Ciebie korzystania z plików w C.

Każdy z nas, korzystając na co dzień z komputera przyzwyczaił się do tego, że plik ma określoną nazwę. Jednak, w pisaniu programu, posługiwanie się całą nazwą niosłoby ze sobą co najmniej dwa problemy, duże zużycie pamięci - przechowywanie całej nazwy pliku zajmuje niepotrzebnie pamięć i ryzyko błędów (ale o tym później).

Programiści korzystają z identyfikatora pliku, który jest pojedynczą liczbą całkowitą. Dzięki temu kod programu jest czytelniejszy i nie trzeba korzystać ciągle z pełnej nazwy pliku. Jednak sam plik nadal jest identyfikowany po swojej nazwie. Aby "przetworzyć" nazwę pliku na odpowiednią liczbę korzystamy z funkcji open lub fopen. Różnica wyjaśniona została poniżej.  Istnieją dwie metody obsługi czytania i pisania do plików, wysokopoziomowa i niskopoziomowa (tylko \*nix, Windows oferuje WinAPI). Nazwy funkcji z pierwszej grupy zaczynają się od litery "f" (np. fopen(), fread(), fwrite()), a identyfikatorem pliku jest wskaźnik na strukturę typu FILE. Owa struktura to pewna grupa zmiennych, która przechowuje dane o pliku. Szczegółami nie musisz się przejmować, funkcje biblioteki standardowej same zajmują się wykorzystaniem struktury FILE. Programista może więc zapomnieć, czym tak naprawdę jest struktura FILE i traktować taką zmienną jako identyfikator pliku. Druga grupa to funkcje typu read(), open(), write() i close(). Podstawowym identyfikatorem pliku jest liczba całkowita, która identyfikuje dany plik w systemie operacyjnym. Liczba ta w systemach typu UNIX jest nazywana deskryptorem pliku. Należy pamiętać, że nie wolno używać funkcji z obu tych grup jednocześnie w stosunku do jednego pliku, tzn. nie można najpierw otworzyć pliku za pomocą fopen(), a następnie odczytywać danych z tego samego pliku za pomocą read().

Czym różnią się oba podejścia do obsługi plików? Metoda wysokopoziomowa ma swój własny bufor, w którym znajdują się dane po odczytaniu z dysku a przed wysłaniem ich do programu użytkownika. W przypadku funkcji niskopoziomowych dane kopiowane są bezpośrednio z pliku do pamięci programu. W praktyce używanie funkcji wysokopoziomowych jest prostsze a przy czytaniu danych małymi porcjami również często szybsze i właśnie ten model zostanie tutaj zaprezentowany. 

Skupimy się teraz na najprostszym z możliwych zagadnień - zapisie i odczycie pojedynczych znaków oraz całych ciągów.
Napiszmy zatem nasz pierwszy program, który stworzy plik "abc.txt" i umieści w nim tekst "Hello world!": 

```
#include <stdio.h>
#include <stdlib.h>

int main (void) {
    FILE * f = fopen("abc.txt", "w"); /* Uwaga! Gwiazdka */
    char * tekst = "Hello world!";
    if (!f) {
        printf("Couldn't open abc.txt!\n");
        exit(1);
    }
    fprintf(f, "%s", tekst); /* Zapisywanie ciągu */
    fclose(f); /* Zamykanie pliku */
}
```

Teraz omówimy najważniejsze elementy programu. Jak już było wspomniane wyżej, do identyfikacji pliku który jest otwarty używa się wskaźnika na strukturę FILE (czyli `FILE *`). Funkcja fopen zwraca ów wskaźnik w przypadku poprawnego otwarcia pliku, bądź też NULL, gdy plik nie może zostać otwarty. Pierwszy argument funkcji to nazwa pliku, natomiast drugi to tryb dostępu - w oznacza "write" (zapisywanie). Zwrócony uchwyt do pliku będzie mógł być wykorzystany jedynie w funkcjach zapisujących dane. I odwrotnie, gdy otworzymy plik podając tryb r ("read", wczytywanie), będzie można z niego jedynie czytać dane. Funkcja fopen została dokładniej opisana w odpowiedniej części rozdziału o bibliotece standardowej. 

Po zakończeniu korzystania z pliku należy plik zamknąć. Robi się to za pomocą funkcji fclose. Jeśli zapomnimy o zamknięciu pliku, wszystkie dokonane w nim zmiany nie zostaną zapisane. (Dla doświadczonych programistów: jeśli nie flushowałeś bufora, dane znikną i nie zostaną zapisane, natomiast jeśli bufor został zapisany do pliku, taki problem nie ma miejsca)

Można zauważyć, że do zapisu do pliku używamy funkcji fprintf, która wygląda bardzo podobnie do printf - jedyną różnicą jest to, że w fprintf musimy jako pierwszy argument podać identyfikator pliku. Nie jest to przypadek - obie funkcje tak naprawdę robią to samo. Używana do wczytywania danych z klawiatury funkcja scanf też ma swój odpowiednik wśród funkcji operujących na plikach, nosi ona nazwę fscanf. W rzeczywistości język C traktuje tak samo klawiaturę i plik - są to źródła danych, podobnie jak ekran i plik, do których można dane kierować. Jest to myślenie typowe dla systemów typu UNIX, jednak dla użytkowników przyzwyczajonych do systemu Windows albo języków typu Pascal może być to co najmniej dziwne. Nie da się ukryć, że między klawiaturą i plikiem na dysku zachodzą podstawowe różnice i dostęp do nich odbywa się inaczej - jednak funkcje języka C pozwalają nam o tym zapomnieć i same zajmują się szczegółami technicznymi. Z punktu widzenia programisty, urządzenia te sprowadzają się do nadanego im identyfikatora. Uogólnione pliki nazywa się w C strumieniami. Każdy program w momencie uruchomienia "otrzymuje" od razu trzy otwarte strumienie,  stdin (wejście) do odczytywania danych wpisywanych przez użytkownika, stdout (wyjście) do wyprowadzania informacji dla użytkownika i stderr (wyjście błędów) do powiadamiania o błędach. Aby korzystać ze standardowych strumieni, musimy dołączyć plik nagłówkowy stdio.h. Nie musimy otwierać ani zamykać strumieni standardowych (tak jak w przypadku niestandardowych plików za pomocą fopen i fclose). Warto tutaj zauważyć, że konstrukcja: `fprintf (stdout, "Hello World!");` jest równoważna konstrukcji `printf ("Hello World!");`. Podobnie jest z funkcją scanf(), `fscanf (stdin, "%d", &var);` działa tak samo jak `scanf("%d", &var);`.

Jeśli nastąpił błąd, możemy się dowiedzieć o jego przyczynie na podstawie zmiennej errno zadeklarowanej w pliku nagłówkowym errno.h. Możliwe jest też wydrukowanie komunikatu o błędzie za pomocą funkcji perror. Na przykład używając:

```
FILE * f;
f = fopen ("/jakaś/fikcyjna/ściezka", "r");
if(!fp) {
   perror("Blad");
   exit(EXIT_FAILURE);
}
```

Komunikat błędu będzie wyglądał tak: `Blad: No such file or directory`.
Inny sposób z użyciem errno wygląda tak:

```
/* Na początku pliku */
#include <errno.h>

/* Gdzieś w funkcji main() */
FILE * f;
errno = 0;
f = fopen("/jakaś/fikcyjna/ścieżka", "r");
printf("Kod bledu: %d", errno);
```

## Ćwiczenia

 * Napisz program, który wyświetli twoje imię.
 * Napisz program, który doda dwie liczby wpisane przez użytkownika
 * Napisz program, który podzieli dwie liczby wpisane przez użytkownika. Uwaga: Obsłuż przypadek, w którym użytkownik wpisuje 0!
 * Napisz program, który utworzy plik i zapisze w nim imię użytkownika, o które go poprosisz.
 * Napisz program, który generuje tabliczkę mnożenia. Uwaga: Spraw, żeby wyglądała jak tabelka (tzn, nie rozjechana)
 * Napisz bardzo prosty kalkulator.
 * Napisz program, który wczyta z pliku dwie liczby i wyświetli ich iloczyn.

**[Powrót do spisu treści](..)**
