
# Monotone strukture

### Uvodni zadatak
Zadan je broj upita _**Q**_ (**1** ⩽ _**Q**_ ⩽ **10^6**) i prazan niz _**A**_, te svaki upit može biti jedan od sljedeća tri:

1) (_**k**, **x**_)  → Ukoliko je _**k**_ jednak **0** dodaj _**x**_ na početak niza _**A**_, a ako je _**k**_ jedank **1** dodaj _**x**_ na kraj niza _**A**_.

2) (_**k**_) → Ukoliko je _**k**_ jednak **0** obriši element koji se nalazi na početku niza. U suprotnom ako je _**k**_ jednak **1** obriši element koji se nalazi
na kraju niza. 

3) Ispiši najveći broj koji se nalazi u nizu _**A**_.
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
  
Struktura monotonog stroga jest zapravo struktura koja radi točno ono što je opisano - dodaje i izbacuje elemente na kraju niza i u svakome trenutku održava najveći element, pritom sve to radeći u složenosti *O(**Q**)*, odnosno broju upita. 
  
Umjesto da u stog _**S**_ ubacujemo vrijednost elementa, ubacivati ćemo vrijednost jednaku _max(**zadnji_element_stoga**, **novi element**)_, ako je stog prazan možemo samo ubaciti vrijednost novog elementa. Vrijednost najvećeg elementa nalaziti će se na vrhu, a ako trebamo izbaciti zadnji element iz niza dovoljno je izbaciti zadnji element iz stoga. Za lakše razumijevanje strukture važno je primijetiti da je _**zadnji_element_stoga**_ zapravo jednak dosadašnjem maksimumu niza. Također primijetimo da je stog _**S**_ ne-opadajuć odnosno da je svaki element veći ili jednak od svojeg prethodnika.
<p align="center">
  <img src="https://crompetitive.github.io/blog/assets/monotone_stack.png" />
</p>

