# Težak-Lagan Rušenje (TLR) stabla (eng. _Heavy-Light Decomposition of tree_) 

### Primjerni zadatak
Zadano je težinsko stablo ***G*** i ***Q*** pitanja (**1** <= ***Q***, ***|G|*** <= **10^5**). Postoje dva tipa pitanja:

1) (***u, v***) → ispisi sumu težina bridova na putu od čvora ***u*** do ***v***.
  
2) (***e, w***) → promjeni težinu brida ***e*** na ***w***.

---
### Spora i djelomično točna rješenja
Započnimo nekim očito sporim rješenjima. Na primjer mogli bismo svaki za svako pitanje 1. vrste proći pretragom u dubinu (eng. ***DFS***) iz čvora ***u*** i za svaki čvor 
pamtiti sumu od čvora ***u*** do tog nekog čvora, a za svako pitanje 2. vrste samo promijeniti vrijednost tog nekog brida. Primijetimo kako za svako pitanje 1. vrste prolazimo 
cijelim stablom, pa je složenost *O(**Q** * *|***G***|*)*.

Primijetimo kako kada bi stablo bilo lanac tada bismo mogli izgraditi tournament i 1. pitanje bi odgovaralo upitu sume nad nekim intervalom (bez smanjenja općenitosti možemo
pretpostaviti da je prvi čvor lanca 1, drugi 2, treći 3 ... zadnji |***G***|), odnosno sumu intervala ***[u,v]***, dok bi 2. pitanje bila promjena vrijednosti ***u***-tog elementa
(ako je brid ***e*** brid između čvorova ***u*** i ***v*** tako da ***u < v***). Tada složenost postaje *O(**Q** * log2|**G**|)*, no samo kada je stablo lanac. Ono što bi voljeli
napraviti jest "srušiti" stablo tako da postane jedan (ili vise) niz nad kojim možemo raditi operacije tournamenta kao i što smo ih radili na lancu.

---
### Teški i lagani bridovi
Označimo neke bridove koji spajaju čvor ***u*** s njegovom djecom laganima, a neke teškima (primijetimo da smo ukorijenili stablo). Preciznije, ako je veličina podstabla djeteta ***v*** 
najveća među veličinama sve ostale djece označimo brid (***u, v***) teškim, a u suprotnom (sve ostale bridove) laganim. 

Primijetimo sljedeće, ako je brid koji spaja dijete ***v*** s njegovim roditeljem ***u*** lagan tada je veličina podstabla djeteta barem upola manja od veličine podstabla roditelja. 
Nadalje to znači da na putu od bilo kojeg čvora do korijena stabla postoji najviše *log2(|**G**|)* laganih bridova. Još primijetimo da svi uzastopni teški bridovi čine lance, a ne stabla 
jer iz svakog čvora izlazi maksimalno jedan težak brid. 
<p align="center">
  <img src="https://github.com/crompetative/blog/assets/tezak-lagan_rusenje_skica.png" />
</p>
<p align="center"> <i> Lagani bridovi obojani su plavo, a teški crveno. </i> </p>

---
### Konačno rješenje i TLR
Na putu od svakog čvora do korijena stabla nalazi se maksimalno *log2(|**G**|)* lanaca teških bridova i *log2(|**G**|)* laganih bridova. Zaključujemo, na svakom jednostavnom putu unutar tog
stabla nalazi se maksimalno 2 * *log2(|**G**|)* lanaca teških bridova i 2 * *log2(|**G**|)* laganih bridova. Zatim što možemo napraviti jest izgraditi tournament stablo nad svakim lancem teških
bridova (u samoj implementaciji bit će dovoljno održavati samo jedno tournament stablo) i riješili smo zadatak!

Kako bismo riješili pitanja 1. vrste pronalazimo najniži zajednički predak čvorova (eng. ***LCA***) iz upita i penjemo se od oba čvora prema tom pretku. Ako je brid iz trenutačnog čvora
u njegovog roditelja lagan ručno dodajemo težinu tog brida na rješenje, u suprotnom dodajemo sumu intervala bridova iz lanca teških bridova i preskačemo cijeli taj lanac do njegovog vrha, tj. 
laganog brida. Treba biti pažljiv kada je zajednički predak unutar lanca teških bridova, jer tada ne uzimamo vrijednost cijelog lanaca nego samo nekog intervala.

Slično kada trebamo promijeniti težinu brida, ako je on lagan ručno ga mijenjamo, a u suprotnom mijenjamo pojedinačnu vrijednost u tournament stablu pripadajućeg teškog lanca.

Analizirajmo složenost, pošto na svakom pitanju 1. vrste prolazimo kroz 2 * *log2(\|**G**\|)* lanaca i složenost upita na svakom pripadajućem lancu jest *log2(\|**duljina_lanca**\|)* krajnja
složenost jest *O(**Q** * log2(\|**G**\|)^2)*.

---
### Implementacija i savjeti
Što se tiče samog zadatke moguće ga je riješiti u složenosti *O(**Q** * log2(\|**G**\|))* bez korištenja TLR-a, ali ovo je dobar uvodan zadatak za ovaj algoritam i u mnogim zadacima on je glavno
riješenje.

Kao što je već spomenuto nije potrebno raditi odvojena tournament stabla za svaki lanac teških bridova već ih sve možemo spojiti u jedan veliki niz, pritom trebamo za svaki lanac znati gdje
započinje i gdje završava u tom velikom nizu, tj. za svaki brid (čvor) trebamo znati koji je njihov indeks.

Također pri odabiru teškog brida dovoljno je odabrati onaj brid koji vodi do djeteta s najvećim podstablom, složenost ostaje ista.

---
### Dodatno
Ako nas zanima vrijednost upita u tournamentu, a tako da upit pokriva cijeli tournament zapravo taj upit samo vraća vrijednost koja se nalazi u korijenu tournamenta. Tu mogućnost smo izgubili 
sa spajanjem tournamenta u jedan, odnosno, sada upit koji pokriva cijeli lanac nas ne košta *O(1)* već *O(log2)*. Sada bi nam moglo pasti na pamet da ipak razdvojimo te tournamente, ali to 
također ima svoje probleme - mogli bismo prijeći memorijsko ograničenje jer tournament mora pokrivati duljinu potencije 2, a da i ne spominjemo _ruznocu_ takve implementacije. To možemo izvesti 
konstrukcijom tournamenta koji zauzima točno *O(2 * **N**)* memorije (ako je ***N*** duljina niza listova). Objašnjenje tog algoritma ovdje neće biti prezentirano. 
