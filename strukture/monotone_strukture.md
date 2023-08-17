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
### Spora rješenja i početna opažanja 
Probajmo raditi doslovno što se od nas traži u zadatku. Imajmo neki niz _**A**_ (duljine _**Q**_ jer tolik je najveći mogući broj elemenata koji će se nalaziti u nizu) i dvije pomoćne 
varijable _**l**_ i _**d**_ u kojima su spremljene pozicije prvog lijevog (početnog) i prvog desnog (krajnjeg) mjesta u nizu koja još nisu zauzeta. Na samom početku njihove 
vrijednosti iznose **0** i **1**. Također zamislimo da je taj niz cikličan, odnosno ako u nekom trenutku bilo koja od varijabli _**l**_ i _**d**_ pređe granice niza možemo ih 
slobodno postaviti da pokazuju na suprotan kraj niza.
  
Akciju prve vrste rješavamo na sljedeći način, ako je _**k**_ jednak **0** tada _**A[l]**_ postaje _**x**_ i smanjimo _**l**_ za jedan. Ako je _**k**_ jedak **1** tada _**A[d]**_ 
postaje _**x**_ i povećamo _**d**_ za jedan. Akciju druge vrste rješavamo samo povećavanjem/smanjivanjem varijabli _**l**_/_**d**_ ako je _**k**_ jednak **0**/**1** (razlog zašto ne trebamo
"brisati" te elemente vidjeti ćemo u objašnjenu treće akcije). Složenost oba postupka je _O(**1**)_. Akciju treće vrste rješavamo jednostavnim prolaskom niza od pozicije _**l**_ do pozicije 
_**r**_ i pamćenjem najvećeg elementa. Složenost ovog potupka je _O(_|_**A**_|_)_ odnosno _O(**Q**)_. 

Ukupna složenost ovog algoritma jest _O(_|_**A**_| * _**Q**)_, što je presporo. Primjetimo jedno isprava glupo opažanje. Ako bi postojao dodatan uvijet u zadatku da je _**x**_ ≥ trenutno 
najvećeg elementa u nizu _**A**_ tada bi složenost traženja najvećeg elementa bila _O(**1**)_ jer umjesto da tražimo najveći element u cijelome nizu mi znamo da se on nalazi ili na početku 
ili na kraju niza, pa bi ukupna složenost bila _O(**Q**)_. 
