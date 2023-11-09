
# Monotone strukture

### Uvodni zadatak
Zadan je broj upita _**Q**_ (**1** ⩽ _**Q**_ ⩽ **10^6**) i prazan niz _**A**_, te svaki upit može biti jedan od sljedećih 5:

1) (_**x**_)  → Dodaj _**x**_ na početak niza _**A**_.

2) (_**x**_)  → Obriši element na početku niza _**A**_.

3) (_**x**_)  → Dodaj _**x**_ na kraj niza _**A**_.

4) (_**x**_)  → Obriši element na kraju niza _**A**_.

5) Ispiši najveći broj koji se nalazi u nizu _**A**_.
<p align="center">
  <img src="https://crompetitive.github.io/blog/assets/mono_uvod_skica.png" />
</p>
<p align="center"> <i> U nekom trenutku niz je [9, 4, 1, 2], te je najveći element <b>9</b>. Zatim dodajemo <b>5</b> na desni kraj, te je najveći i dalje <b>9</b>. No nakon dodavanja
brišemo element na početku (<b>9</b>) te je sada najveći element <b>5</b>. Nakon ovih promijena niz je [4, 1, 2, 5]. </i> </p>

---
### Potrebno znanje
Za daljnje razumijevanje teksta potrebno je znati strukture: stog (eng. _stack_), red (eng. _queue_) i dek (eng. _deque_).

---
### Spora rješenja 
Možemo raditi doslovno što se od nas traži u zadatku, odnosno održavati jedan dek i u svakome trenutku znati vrijednost najvećeg elementa koji se nalazi u njemu. Primijetimo kako takvo rješenje možemo raditi naivno u složenosti *O(**Q** * **Q**)*, tako da za svaki upit treće vrste prođemo po cijelome deku, te pronađemo i ispišemo najveći element (_**Q**_ puta prolazimo po cijelom nizu, koji je maksimalne duljine _**Q**_). Također moguće je i znatno bolje rješenje s turnirskim stablom u složenosti *O(**Q** * log2|**Q**|)*, no moguće je postići rješenje složenosti *O(**Q**)* koje, uz to što je i brže, je znatno lakše za koristiti i implementirati. 
  
---
### Monotoni stog 

Zamislimo pojednostavljenu verziju zadataka u kojoj su promjene (dodavanje i brisanje elementa) niza moguće samo s njegove jedne strane, na primjer s kraja (ako su promjene moguće samo na početku niza algoritam je "simetričan"). 
  
Struktura monotonog stroga jest zapravo struktura koja radi točno ono što je opisano - dodaje i izbacuje elemente na kraju niza i u svakome trenutku održava najveći element, pritom sve to radeći u složenosti *O(**Q**)*, odnosno broju upita (svaki upit imati će složenost *O(**1**)*). 
  
Umjesto da u stog _**S**_ ubacujemo vrijednost novog elementa, ubacivati ćemo vrijednost jednaku _max(**zadnji_element_stoga**, **novi element**)_, ako je stog prazan možemo samo ubaciti vrijednost novog elementa. Vrijednost najvećeg elementa nalaziti će se na vrhu, a ako trebamo izbaciti zadnji element iz niza dovoljno je izbaciti zadnji element iz stoga. Za lakše razumijevanje strukture važno je primijetiti da je _**zadnji_element_stoga**_ zapravo jednak dosadašnjem maksimumu niza. Također primijetimo da je stog _**S**_ ne-opadajuć odnosno da je svaki element veći ili jednak od svojeg prethodnika.
<p align="center">
  <img src="https://crompetitive.github.io/blog/assets/monotone_stack.png" />
</p>

---
### Monotoni red

Zamislimo sljedeću pojednostavljenu verziju zadataka u kojoj možemo dodavati elemente samo na kraj i brisati s početka niza. to jest raditi operacije reda (eng. _queue_), te naravno ispisivati najveći element u nizu. Također monotoni red na engleskom se naziva _sliding window_ metoda jer naš red možemo zamisliti kao segment koji se pomiće u desno po nizu, ponekad se sužava, a ponekad širi, te uvijek moramo znati najveću vrijednost koja se nalazi u segmentu.

Za ovu metodu trebati ćemo koristiti jedan dek (eng. _deque_) koji će odgovarati na upite za najveći element, kojeg ćemo nazvati _**S**_, te jedan red (eng. _queue_) koji predstavlja pravi niz, nazvati ćemo ga _**A**_. Red _**A**_ predstavlja "pravo" stanje našeg niza, tj. nakon operacije dodavanja (na kraj) doista dodajemo element na kraj, te slično nakon brisanja elementa (s početka) doista izbacujemo element iz _**A**_. Za razliku od reda _**A**_, dek _**S**_ raditi će slično kao i stog _**S**_ u monotonom stogu. 

Pri dodavanju novog elementa na kraj niza događa se sljedeće. Na kraj reda _**A**_ dodajemo novi element, a s kraja deka _**S**_ brišemo sve elemente koji su **manji** od novog elementa, te nakon tih brisanja tek dodajemo novi element na kraj. Motivacija iza ove radnje, jest ta da ukoliko imamo neki element _**a**_ koji je dodan prije nekog **većeg** elementa _**b**_, zapravo uopće nije vrijedan daljnjeg čuvanja, jer nikada neće moći biti najveći element (dokle god se _**a**_ nalazi u nizu, nalaziti će se i sigurno _**b**_). 

Ukoliko trebamo izbrisati element na početku, trebamo provjeriti je li prvi element reda _**A**_ jednak prvom elementu deka _**S**_ - ukoliko jest, izbacimo prvi element iz deka _**S**_. Nakon te provjere možemo slobodno izbaciti element s početka reda _**A**_. 

Primjetimo kako se najveći element uvijek nalazi na početku deka _**S**_, te da je  dek _**S**_ ne-rastuć.

Također primjetimo kako složenost monotonog reda jest **amortizirano** *O(**Q**)*. Složenost je amortizirana jer makar operacija dodavanja nije nužno složenosti *O(**1**)*, brisanje s kraja reda dogoditi će se najviše *O(**Q**)* puta (dok operacija je brisanja uvijek *O(**1**)*).
<p align="center">
  <img src="https://crompetitive.github.io/blog/assets/monotone_queue_image.png" />
</p>

Za bolje razumijevanje proučite kod:
```c++
#include <cstdio>
#include <queue>
#include <deque>

using namespace std;

// implementirati ćemo monotoni red kao strukturu
struct monotone_queue {
  // u našoj strukturi deklarirajmo pomoćne strukture (dek S i red A)
  deque<int> S;
  queue<int> A;

  // funkcija dodavanja elementa
  void push(int x) {
    // najprije dodajemo element u red A
    A.push(x);
    // zatim brišemo sve elemente koji su manji od x s kraja deka
    while(!S.empty() && S.back() < x) { S.pop_back(); }
    // te na kraja dodajemo x u dek
    S.push_back(x);
  }

  // funkcija brisanja elementa (s početka)
  void pop() {
    // ukoliko su prvi elementi reda A i deka S jednaki obrisi prvi element iz S
    if(S.front() == A.front()) { S.pop_front(); }
    // obrisi prvi element iz A
    A.pop();
  }

  // funkcija koja vraća najveći element u nizu
  int max() {
    // najveći element uvijek se nalazi na početku deka S
    return S.front();
  }
};
```
