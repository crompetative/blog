# Monotone strukture

### Uvodni zadatak
Zadan je broj akcija _**Q**_ (**1** ⩽ _**Q**_ ⩽ **10^6**) i prazan niz _**A**_, te svaka akcija može biti jedna od sljedeće dvije vrste:

1) (_**k**, **x**_) → Ukoliko je _**k**_ jednak **0** dodaj _**x**_ na početak niza _**A**_, a ako je _**k**_ jedank **1** dodaj _**x**_ na kraj niza _**A**_.

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
### Spora rješenja i početna opažanja 
Možemo raditi doslovno što se od nas traži u zadatku, odnosno trebamo održavati jedan dek i u svakome trenutku znati vrijednost najvećeg elementa koji se nalazi u njemu.
