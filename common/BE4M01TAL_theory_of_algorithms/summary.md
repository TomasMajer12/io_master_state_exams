# BE4M01TAL — Teorie algoritmů (TAL) — Souhrn ke státnici

**Vyučující:** prof. RNDr. Marie Demlová, CSc. (katedra matematiky 13101, FEL ČVUT)
**Jazyk:** česky (přednášky i učebnice), anglické termíny v závorkách pro orientaci
**Zdroje:**
- `materials/tal-doh.pdf` — komplexní česká učebnice Demlové (číslované definice ve tvaru X.Y.Z, citováno jako *doh §X.Y.Z*)
- `materials/tals*w.pdf` — týdenní přednáškové slidy (citováno jako *přednáška Tk*)
- Oficiální stránka kurzu: <https://math.fel.cvut.cz/en/people/demlova/tal/>
- Sylabus: <https://fel.cvut.cz/cz/education/bk/predmety/48/63/p4863106.html>
- Doporučená literatura: Kozen *The Design and Analysis of Algorithms* (Springer 1991), Harel *Algorithmics* (Addison-Wesley 2002), Talbot & Welsh *Complexity and Cryptography* (Cambridge 2006)

**Struktura souhrnu** zrcadlí 6 oficiálních okruhů státnice (viz `../../00_subjects_overview.md`):

1. [Asymptotický růst, časová a paměťová složitost, správnost algoritmů](#1-asymptotický-růst-funkcí-časová-a-paměťová-složitost-správnost-algoritmů)
2. [Turingovy stroje — deterministické, vícepáskové, nedeterministické](#2-turingovy-stroje)
3. [Rozhodovací úlohy a jazyky. Třídy P, NP, co-NP. Redukce, NPC, Cookova věta. Heuristiky a aproximace](#3-rozhodovací-úlohy-a-jazyky-třídy-p-np-co-np-redukce-npc-cookova-věta-heuristiky-a-aproximace)
4. [Třídy paměťové složitosti PSPACE a NPSPACE. Savitchova věta](#4-třídy-paměťové-složitosti-pspace-a-npspace-savitchova-věta)
5. [Pravděpodobnostní algoritmy. Randomizované Turingovy stroje. Třídy RP, ZPP, co-RP](#5-pravděpodobnostní-algoritmy-randomizované-tm-třídy-rp-zpp-co-rp)
6. [Rozhodnutelnost a nerozhodnutelnost. Rekursivní a rekursivně spočetné jazyky. Diagonální jazyk. Univerzální jazyk a univerzální TM](#6-rozhodnutelnost-a-nerozhodnutelnost-rekursivní-a-rekursivně-spočetné-jazyky)

⭐ = u zkoušky nejčastěji vyžadovaná témata (podle archivu `../../frequently_asked_questions/oi_wiki_committee_archive.md`).

---

## 1. Asymptotický růst funkcí, časová a paměťová složitost, správnost algoritmů

### 1.1 Algoritmus, úloha, instance (*doh §1.1*)

**Algoritmus** (algorithm) = dobře definovaná posloupnost výpočetních kroků, která ze vstupu (zadání) vytváří výstup (řešení).

**Úloha / problém** (problem) = obecná specifikace vztahu zadání ↔ řešení. **Instance** úlohy je konkrétní zadání všech parametrů úlohy.

Algoritmus A **řeší úlohu** U ⇔ pro každou instanci U vydá správné řešení. *Důsledek:* algoritmus, který se na nějakém vstupu nezastaví, **nemůže** žádnou úlohu řešit.

### 1.2 Měření časové složitosti (*doh §1.1.4*)

Dvě hlavní míry:

1. **Nejhorší případ** (worst case) — asymptotický odhad $T(n)$ doby pro každou instanci velikosti $n$.
2. **Průměrná složitost** (average case) — asymptotický odhad $T_{\text{aver}}(n)$ s váhou pravděpodobnosti instancí.

Pro posloupnost stejných operací: **amortizovaná složitost** (amortized complexity) = průměrná složitost nejhoršího případu pro posloupnost $n$ operací.

### 1.3 ⭐ Asymptotický růst funkcí (*doh §1.2*)

Nechť $g(n)$ je nezáporná funkce. Pro nezápornou $f(n)$ definujeme:

| Symbol | Definice | Význam |
|---|---|---|
| $f \in O(g)$ | $\exists c>0,\ n_0:\ f(n) \le c\,g(n)$ pro $n \ge n_0$ | "neroste více než $g$" (horní odhad) |
| $f \in \Omega(g)$ | $\exists c>0,\ n_0:\ f(n) \ge c\,g(n)$ pro $n \ge n_0$ | "roste alespoň jako $g$" (dolní odhad) |
| $f \in \Theta(g)$ | $\exists c_1, c_2 > 0,\ n_0:\ c_1 g(n) \le f(n) \le c_2 g(n)$ | "stejný asymptotický řád" |
| $f \in o(g)$ | $\forall c>0\ \exists n_0:\ f(n) < c\,g(n)$ pro $n \ge n_0$ | "roste striktně méně" |
| $f \in \omega(g)$ | $\forall c>0\ \exists n_0:\ f(n) > c\,g(n)$ pro $n \ge n_0$ | "roste striktně více" |

**Vztahy a vlastnosti**
- $f \in \Omega(g) \Leftrightarrow g \in O(f)$
- $f \in \Theta(g) \Leftrightarrow f \in O(g) \land f \in \Omega(g)$
- $f \in \Theta(g) \Leftrightarrow g \in \Theta(f)$ (symetrie)
- O, Ω, Θ jsou **tranzitivní a reflexivní**.

**Limitní kritérium** (*doh §1.2.11*):
$$\lim_{n\to\infty} \frac{f(n)}{g(n)} = \begin{cases} 0 & \Rightarrow f \in o(g) \\ \infty & \Rightarrow f \in \omega(g) \\ a \ne 0,\ a \in \mathbb{R} & \Rightarrow f \in \Theta(g) \end{cases}$$

**Notační poznámka.** Symboly značíme `f(n) ∈ O(g(n))`. Anglická literatura píše `f(n) = O(g(n))`, ale jde o jednosměrný "rovnostní" symbol bez obvyklých vlastností rovnosti.

**Příklady** (*doh §1.2.15*)
- $\log_a n \in \Theta(\log_b n)$ pro $a, b > 1$ (změna základu = konstantní násobek).
- $\lg(n!) \in \Theta(n \lg n)$ — důsledek Gaussovy věty:
  $$n^{n/2} \le n! \le \left(\tfrac{n+1}{2}\right)^n.$$

> **Varování (přednáška T2 — "špatný indukční důkaz").** Tvrzení $\sum_{i=1}^n i \in O(n)$ je **chybné**. Důkaz indukcí, kde předpokládáme $\sum_{i=1}^n i \in O(n)$ a v kroku přičteme $(n+1) \in O(n)$, je vadný: konstanty v O-zápisu se nesmí "akumulovat" lineárně přes indukci. Správná hodnota je $\Theta(n^2)$.

### 1.4 Řešení rekursivních vztahů (*doh §1.3*)

#### 1.4.1 Přímá metoda — odhad + indukce

**Příklad.** $T(n) = 2T(n/2) + n$, $T(1) = 1$. Odhad $T(n) \le c \cdot n \lg n$.
- Báze: $T(2) = 4 \le 2c$ pro $c \ge 2$.
- Krok: $T(n) \le 2 \cdot c(n/2) \lg(n/2) + n = cn\lg n - cn + n \le cn \lg n$ pro $c \ge 2$.
- $\Rightarrow T(n) \in \Theta(n \lg n)$.

#### 1.4.2 Strom rekurze (*doh §1.3.2–1.3.4*)

**Příklad.** $T(n) = 3T(n/4) + n^2$. Hladina $i$ má $3^i$ uzlů, každý práce $(n/4^i)^2$, příspěvek $(3/16)^i n^2$. Suma geometrické řady $\le \tfrac{16}{13} n^2$. Listy přidávají $\Theta(n^{\log_4 3}) = o(n^2)$. Celkem $T(n) \in \Theta(n^2)$.

#### 1.4.3 ⭐ Master Theorem (*doh §1.3.5*)

Nechť $a \ge 1$, $b > 1$ konstanty, $T(n) = a\,T(n/b) + f(n)$. Pak:

| Případ | Podmínka na $f$ | Výsledek |
|---|---|---|
| 1 | $f(n) \in O(n^{\log_b a - \varepsilon})$ pro nějaké $\varepsilon > 0$ | $T(n) \in \Theta(n^{\log_b a})$ |
| 2 | $f(n) \in \Theta(n^{\log_b a})$ | $T(n) \in \Theta(n^{\log_b a} \lg n)$ |
| 3 | $f(n) \in \Omega(n^{\log_b a + \varepsilon})$ a regularita $a\,f(n/b) \le c\,f(n),\ c<1$ | $T(n) \in \Theta(f(n))$ |

**Zobecnění.** $f(n) \in \Theta(n^{\log_b a} \lg^k n) \Rightarrow T(n) \in \Theta(n^{\log_b a} \lg^{k+1} n)$.

**Hanojská věž** (přednáška T2, klasický příklad rekurze) — $T(n) = 2T(n-1) + 1$, $T(1) = 1 \Rightarrow T(n) = 2^n - 1 \in \Theta(2^n)$. Master Theorem zde nelze použít (vzorec není $aT(n/b)$).

### 1.5 Amortizovaná složitost — tři metody (*doh §1.3.9*)

| Metoda | Princip | Užití |
|---|---|---|
| **Agregační** | spočítáš celkovou složitost $T(n)$ posloupnosti $n$ operací; amortizovaná = $T(n)/n$ | jednoduché posloupnosti |
| **Účetní** (accounting) | každé operaci přiřadíš "kreditní" cenu $\hat{c}_i$; podmínka $\sum \hat{c}_i \ge \sum c_i$ pro každý prefix | nestejně drahé operace, kredit kompenzuje |
| **Potenciálová** | potenciálová funkce $\Phi(D_i) \ge \Phi(D_0)$; $\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$ | nejflexibilnější |

**Příklad.** Binární čítač s operací `Increment`. Sekvenci $n$ inkrementů provedeme v čase $\Theta(n)$ (nikoliv $\Theta(n \lg n)$, jak by naznačovala nejhorší jediná operace), takže amortizovaná složitost = $O(1)$. Důkaz potenciálovou metodou: $\Phi(D_i)$ = počet jedničkových bitů.

### 1.6 Správnost algoritmů — variant a invariant (*doh §2.2*) ⭐

Pro důkaz správnosti ověřujeme **dvě věci**:

1. Algoritmus se na **každém vstupu zastaví** (totální správnost, total correctness).
2. Po zastavení dává **správný výstup** (partial correctness, podmíněná správnost).

**Variant** (variant) = výraz s hodnotou v $\mathbb{N}$ (nebo dobře uspořádané množině), který se s každou iterací **striktně zmenšuje** k nejmenší možné hodnotě. Zaručuje konečnost cyklu.

**Invariant** (loop invariant) = tvrzení, které:
- platí **před** prvním cyklem,
- je **zachováno** každou iterací,
- z platnosti při ukončení plyne správnost výstupu.

#### 1.6.1 Příklad: Bublinkové třídění (*doh §2.2.2*)
```
for k = n step -1 to 2 do
  for j = 1 step 1 to k-1 do
    if a[j] > a[j+1] then swap a[j], a[j+1]
```
- **Variant vnějšího cyklu:** $k$ klesá z $n$ k 2 ⇒ konečnost.
- **Invariant (po $i$-tém průchodu, tj. $k = n-i$):**
  a) $a[n-i+1], \dots, a[n]$ obsahuje $i$ největších prvků;
  b) $a[n-i+1] \le \dots \le a[n]$.
- Indukcí podle $i$.

#### 1.6.2 Příklad: Euklidův algoritmus (*doh §2.1*)
```
Euklid(a, b):
  if b = 0: return a
  else: return Euklid(b, a mod b)
```
**Variant:** druhý argument $b$ — striktně klesající přirozené číslo.

**Invariant:** dvojice $(r, t)$ a $(t, t \bmod r)$ mají **stejnou množinu společných dělitelů** ⇒ NSD se zachovává, výstup je NSD$(a,b)$.

**Časová složitost.** $y_{k+2} < y_k/2$ (druhý argument se každé 2 kroky alespoň zpolovi) ⇒ počet volání $\in O(\lg b) = \Theta(\lg b)$. Dolní mez ukazují Fibonacciho čísla: $\text{Euklid}(F_{k+2}, F_{k+1})$ dělá $k+1$ volání, $F_{n+2} \ge (3/2)^n$ (indukcí).

#### 1.6.3 Příklad: Minimální kostra — obecné schéma (*doh §2.2.10–2.2.13*)
```
K := ∅; S := {{v} | v ∈ V}
while |S| > 1:
  vyber hranu e ∈ E\K mezi C₁, C₂ ∈ S,
    nejlevnější aspoň pro jednu z nich
  K := K ∪ {e}; S := (S \ {C₁,C₂}) ∪ {C₁ ∪ C₂}
```
- **Variant:** $|S|$ klesá o 1 ⇒ končí po $n-1$ krocích.
- **Invariant:** $K \subseteq T_{\min}$ (pro nějakou min. kostru). Klíčový důkaz výměnnou argumentací: kdyby vybraná hrana $e$ nepatřila do žádné min. kostry, lze ji vyměnit za nějakou hranu $e_1$ na cestě v $T_{\min}$ se stejnou nebo nižší cenou.

**Důsledek:** Kruskalův i Primův algoritmus jsou speciální případy tohoto schématu (liší se volbou hrany).

### 1.7 Nejkratší cesty (*doh §2.3*) — používáme později při NPC redukcích

**Bellmanův princip optimality:** $u(x, y) = \min_{z \ne y}(u(x, z) + a(z, y))$.

**Trojúhelníková nerovnost** (bez záporných cyklů): $u(x, y) \le u(x, z) + u(z, y)$.

**Algoritmy:** Bellman-Ford $O(mn)$, fronta verze $O(mn)$, Floyd-Warshall $O(n^3)$.

---

## 2. Turingovy stroje

### 2.1 Deterministický TM (DTM) — formální definice (*doh §3.1.2*) ⭐

$$M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$$

- $Q$ — konečná množina **stavů**
- $\Sigma$ — vstupní abeceda (input alphabet)
- $\Gamma$ — pásková abeceda, $\Sigma \subset \Gamma$
- $B \in \Gamma \setminus \Sigma$ — **prázdný symbol** (blank)
- $\delta: (Q \setminus F) \times \Gamma \to Q \times \Gamma \times \{L, R\}$ — parciální **přechodová funkce** (transition function)
- $q_0 \in Q$ — počáteční stav (initial state)
- $F \subseteq Q$ — koncové (přijímající) stavy (accepting states)

**Páska je nekonečná na obě strany**, na začátku obsahuje vstup obklopený blanky.

### 2.2 Situace (konfigurace, ID) a krok výpočtu (*doh §3.1.3–3.1.6*)

**Situace** (instantaneous description, ID): $X_1 X_2 \dots X_{i-1}\,q\,X_i X_{i+1} \dots X_k$ — stav vlevo od symbolu pod hlavou.

**Krok výpočtu** $\vdash$:
- $\delta(q, X_i) = (p, Y, R)$: $\dots X_{i-1}\,q\,X_i \dots \vdash \dots X_{i-1}\,Y\,p\,X_{i+1} \dots$
- $\delta(q, X_i) = (p, Y, L)$: $\dots X_{i-1}\,q\,X_i \dots \vdash \dots p\,X_{i-1}\,Y\,X_{i+1} \dots$
- $\delta(q, X_i)$ nedefinováno ⇒ **TM se zastaví**.

**Výpočet** = $\vdash^*$ (reflexivní a tranzitivní uzávěr).
- **Úspěšné zastavení:** koncový stav $q' \in F$.
- **Neúspěšné zastavení:** $\delta$ nedef. v nekoncovém stavu.

### 2.3 Jazyk přijímaný / rozhodovaný TM ⭐ (*doh §3.1.7, 3.1.12*)

- $L(M) = \{ w \in \Sigma^* \mid M \text{ se na } w \text{ úspěšně zastaví} \}$ — **jazyk přijímaný** $M$.

| Pojem | Význam |
|---|---|
| $L$ je **přijímán** (recognized) TM | $\exists M:\ L = L(M)$ |
| $L$ je **rozhodován** (decided) TM | $\exists M:\ L = L(M)$ **a** $M$ se na všech vstupech zastaví |

**Důsledek:** rozhodovaný ⇒ přijímán; opačně **neplatí**.

**Časová složitost** $T(n)$ = max počet kroků k zastavení přes vstupy délky $n$; jinak nedefinováno.

**Paměťová složitost** $S(n)$ = největší rozdíl pořadových čísel polí, která TM použil.

### 2.4 Techniky návrhu TM (*doh §3.1.14–3.1.15*, přednáška T6)

1. **Informace pamatovaná stavem** — stav $(q, 0)$ kóduje "stav $q$ + naposledy přečtená 0".
2. **Více stop** (multi-track) — páskový symbol = $k$-tice; jedna stopa značí "navštívené" symboly.
3. **Podprogramy** — pojmenované sady stavů spravující konkrétní úkol.

### 2.5 Vícepáskový TM (*doh §3.1.16*)

TM s $k$ páskami: $\delta: (Q \setminus F) \times \Gamma^k \to Q \times \Gamma^k \times \{L, R\}^k$. Každá hlava se pohybuje nezávisle.

**Věta o ekvivalenci** (*doh §3.1.20*) ⭐
> K vícepáskovému TM $M_1$ s časovou složitostí $T(n)$ existuje 1-páskový TM $M_2$ se stejným chováním a složitostí $O(T(n)^2)$, tj. **kvadratické zpomalení**.

**Myšlenka simulace:** $M_2$ má $2k$ stop — pro každou pásku $M_1$ jednu stopu s obsahem a druhou stopu se značkou polohy hlavy. Jeden krok $M_1$ = projetí pásky $M_2$ + aktualizace polí.

### 2.6 Nedeterministický TM (NTM) ⭐ (*doh §3.1.21–3.1.23*)

$$\delta: (Q \setminus F) \times \Gamma \to \mathcal{P}_f(Q \times \Gamma \times \{L, R\})$$

V dané situaci NTM **vybere libovolný krok** z konečné množiny možností.

**Jazyk přijímaný NTM:** $L(M) = \{w \mid \exists \text{ posloupnost kroků vedoucí do } q_f\}$.

NTM **rozhoduje** $L$, pokud **každý** výpočet (každá nedeterministická volba) se zastaví.

**Věta o determinizaci** (*doh §3.1.23*)
> Ke každému NTM přijímajícímu/rozhodujícímu $L$ existuje **deterministický 1-páskový** TM se stejným chováním (s exponenciálním zpomalením v nejhorším případě).

### 2.7 Vztah TM a RAM (*doh §3.2*)

**RAM** (random access machine) = počítač s libovolným přístupem:
- programová jednotka (program + programový čítač)
- aritmetická jednotka (+, −, ×, /)
- paměť (neomezený počet buněk s celými čísly)
- vstupní/výstupní jednotka

Buňka adresy 0 = pracovní registr, adresa 1 = indexový registr. Instrukce: `LOAD, STORE, ADD, SUB, MUL, DIV, READ, WRITE, JUMP, JZERO, JGE, STOP, ACCEPT, REJECT`. Operandy: konstanta `=j`, přímá adresace `j`, nepřímá `*j` (přes indexový registr).

**Vzájemné simulace:**
- TM → RAM (*doh §3.2.16*): $n$ kroků TM ⇒ $O(n^2)$ kroků RAM.
- RAM → vícepáskový TM s 5 páskami (*doh §3.2.17*); při omezení na bitový růst velikosti slov simuluje TM $n$ kroků RAM v $O(n^3)$ (*doh §3.2.18*).
- 1-páskový TM simuluje RAM v $O(n^6)$.

**Churchova-Turingova teze** (přednáška T5): cokoliv intuitivně vypočitatelné = vypočitatelné TM = vypočitatelné RAM. Polynomiální vztahy mezi modely jsou jádrem **robustního pojmu polynomiální složitosti** ⇒ definice tříd P, NP nezávisí na konkrétním modelu.

---

## 3. Rozhodovací úlohy a jazyky. Třídy P, NP, co-NP. Redukce, NPC, Cookova věta. Heuristiky a aproximace

> **Pozn.:** Toto je **nejčastěji zkoušený okruh státnice** — v archivu OI-Wiki tvoří přes 30 % všech TAL otázek. Klíčové jsou definice tříd, polynomiální redukce, Cookova věta a alespoň jedna konkrétní NPC redukce (např. 3-SAT → 3-barevnost nebo 3-SAT → klika).

### 3.1 Rozhodovací úlohy a tři verze úlohy (*doh §4.1*)

**Rozhodovací úloha** (decision problem) — odpověď ANO / NE.

**Úloha SAT — splnitelnost booleovských formulí** (*doh §4.1.2*) ⭐ — *seed celého řetězce NPC redukcí (Cookova věta), proto ji definujeme hned a formálně.*

- **Literál** (literal) = proměnná $x$, nebo její negace $\lnot x$.
- **Klauzule** (clause) = disjunkce literálů $C = l_1 \lor l_2 \lor \dots \lor l_k$.
- **CNF** (konjunktivní normální forma, conjunctive normal form) = konjunkce klauzulí $\varphi = C_1 \land C_2 \land \dots \land C_m$.
- Formule je **splnitelná** (satisfiable) ⇔ existuje ohodnocení proměnných hodnotami TRUE/FALSE, při kterém je $\varphi$ pravdivá.

**SAT.** Vstup: výroková formule $\varphi$ v CNF. Otázka: je $\varphi$ splnitelná? (Neptáme se *jakým* ohodnocením — jen zda nějaké existuje.)

**$k$-SAT** = SAT, kde každá klauzule má nejvýše (resp. přesně) $k$ literálů. Pozor na hranici: **3-SAT je NPC** (§3.7.1), ale **2-SAT je v P** (§3.7.16).

**Optimalizační úloha** (optimization problem) má **tři verze** (přednáška T7):

| Verze | Otázka |
|---|---|
| **Rozhodovací** | ∃ řešení s hodnotou ≤ $K$ (resp. ≥ $K$)? |
| **Vyhodnocovací** | Jaká je hodnota optima? |
| **Optimalizační** | Najdi optimální řešení. |

**Tvrzení (*doh §4.1.8*):** polynomiální řešitelnost jedné verze ⇒ všech tří. Důkaz pro TSP: rozhodovací → vyhodnocovací binárním vyhledáváním ($O(\lg)$), → optimalizační fixováním hran ($O(n^2)$).

### 3.2 Instance jako slovo, úloha jako jazyk (*doh §4.2.1–4.2.2*) ⭐

Každá instance je zakódována jako slovo nad abecedou $\Sigma$. **Jazyk úlohy** $U$:
$$L_U = \{w \in \Sigma^* \mid w \text{ je kód ANO-instance}\}.$$

Doplněk $L_U^c = \Sigma^* \setminus L_U$ obsahuje kódy NE-instancí (a slova mimo formát).

### 3.3 ⭐ Třídy P a NP (*doh §4.2*)

$$\boxed{\; U \in \mathbf{P} \;\;\Longleftrightarrow\;\; \exists\,\text{deterministický TM rozhodující } L_U \text{ v polynomiálním čase}.\;}$$

$$\boxed{\; U \in \mathbf{NP} \;\;\Longleftrightarrow\;\; \exists\,\text{NTM rozhodující } L_U \text{ v polynomiálním čase}.\;}$$

**Místo TM lze brát RAM** — robustnost díky polynomiálním simulacím.

**Příklady P (*doh §4.2.4*)** — minimální kostra, nejkratší cesty (Dijkstra/Floyd/Bellman-Ford), maximální tok v síti, minimální řez, 2-SAT, prvočíselnost (AKS test, 2002).

**Příklady NP (*doh §4.2.9*)** — kliky, $k$-barevnost ($k \ge 3$), batoh, problém obchodního cestujícího, hamiltonovský cyklus.

#### Ekvivalentní charakterizace NP (přednáška T7): nedeterministický algoritmus jako dvoufázový proces

Úloha $U \in $ NP ⇔ ∃ polynomiální algoritmus, který má dvě fáze:
1. **Generování kandidáta** $s$ (certifikát, svědek; angl. *certificate, witness*) — nedeterministicky náhodný řetězec.
2. **Verifikace** — deterministický polynomiální algoritmus na vstupu $(I, s)$ vydá ANO / NEVÍM.

**Korektnost:** $\forall$ ANO-instance $I$ ∃ $s$: verifikace = ANO; $\forall$ NE-instance ∄ takové $s$.

Velikost certifikátu $|s|$ je nutně polynomiální v $|I|$ (přihlédnutí k polynomiálnímu času verifikace).

**Důsledek (zřejmá inkluze):** $\mathbf{P} \subseteq \mathbf{NP}$.

### 3.4 ⭐⭐ Polynomiální redukce úloh (*doh §4.3.1–4.3.2*)

**Redukce** $U \propto V$ ("$U$ se redukuje na $V$"): existuje algoritmus $M$ převádějící instance $U$ na instance $V$ se zachováním odpovědi:
$$I \text{ je ANO-instance } U \;\;\Longleftrightarrow\;\; M(I) \text{ je ANO-instance } V.$$

**Polynomiální redukce** $U \propto_p V$: navíc $M$ pracuje v polynomiálním čase. Význam: **"$U$ není těžší než $V$"** — kdykoliv umíme rozhodovat $V$, umíme rozhodovat i $U$.

**Tranzitivita (*doh §4.3.2*):** $U \propto_p V \land V \propto_p W \Rightarrow U \propto_p W$.

**Důležitá tvrzení (*doh §4.3.4–4.3.5*).** Pro $U \propto_p V$:
1. $V \in \mathbf{P} \Rightarrow U \in \mathbf{P}$ (snadné se přenáší dolů).
2. $U \in \mathbf{NPC} \Rightarrow V \in \mathbf{NPC}$ (těžkost se přenáší nahoru).

A klíčová implikace pro otevřený problém:
$$\mathbf{NPC} \cap \mathbf{P} \ne \emptyset \;\;\Longrightarrow\;\; \mathbf{P} = \mathbf{NP}.$$

Tj. nalezení polynomiálního algoritmu pro **jedinou** NPC úlohu by vyřešilo otázku P vs NP.

### 3.5 ⭐ Třída NPC a NP-těžké úlohy (*doh §4.3.3, 4.3.6*)

$$\boxed{\; U \in \mathbf{NPC} \;\;\Longleftrightarrow\;\; U \in \mathbf{NP}\ \land\ \forall V \in \mathbf{NP}: V \propto_p U.\;}$$

NPC úlohy jsou "nejtěžší" v NP — kdyby byla jedna v P, byla by celá NP v P.

**NP-těžká** (NP-hard): existuje NPC úloha $V$ s $V \propto_p U$. NP-těžká úloha **nemusí** být v NP (může být ještě těžší, např. v PSPACE).

### 3.6 ⭐⭐ Cookova věta (Cook 1971) (*doh §4.3.7*)

$$\boxed{\;\mathbf{SAT \in NPC.}\;}$$

Tj. SAT (splnitelnost CNF formule) je NP-úplná.

**Klíčový postup důkazu** (zkouší se nejčastěji — viz oi-wiki questions):

1. **SAT ∈ NP:** zřejmé — certifikát je splňující ohodnocení proměnných, verifikace v lineárním čase.

2. **Univerzální polynomiální redukce.** Pro libovolnou úlohu $U \in $ NP existuje NTM $M$ s polynomiálním stop-časem $p(n)$, který rozhoduje $L_U$. Konstruujeme polynomiální převod $w \mapsto \varphi_{M, w}$ (CNF formule) takový, že
   $$w \in L(M) \;\;\Longleftrightarrow\;\; \varphi_{M, w} \text{ je splnitelná}.$$

3. **Logické proměnné kódují konfiguraci výpočtu** v čase $0 \le i \le p(n)$:
   - $h_{i,j}$ — hlava je v čase $i$ na poli $j$
   - $s^q_i$ — TM je v čase $i$ ve stavu $q$
   - $t^A_{i,j}$ — v poli $j$ v čase $i$ je symbol $A$

4. **Osm skupin klauzulí** $\varphi_1, \dots, \varphi_8$:

| | Tvrzení |
|---|---|
| $\varphi_1$ | V každém čase je TM právě v jednom stavu. |
| $\varphi_2$ | V každém čase je hlava právě na jednom poli. |
| $\varphi_3$ | V každém čase je v každém poli právě jeden symbol. |
| $\varphi_4$ | Počáteční konfigurace odpovídá vstupu $w$. |
| $\varphi_5$ | Krok podle $\delta$: $(s^q_i \land h_{i,j} \land t^A_{i,j}) \Rightarrow \bigvee_{(p, C, D) \in \delta(q, A)} (s^p_{i+1} \land t^C_{i+1, j} \land h_{i+1, j+D})$. |
| $\varphi_6$ | Nečtená pole se nemění: $(\lnot h_{i,j} \land t^A_{i,j}) \Rightarrow t^A_{i+1, j}$. |
| $\varphi_7$ | V koncovém stavu zůstává. |
| $\varphi_8$ | V čase $p(n)$ je TM v přijímajícím stavu $q_f$. |

5. **Sestavení:** $\varphi_{M, w} = \varphi_1 \land \dots \land \varphi_8$. Velikost je polynomiální v $p(n)$ (počet proměnných i klauzulí). Převod do CNF zachovává polynomiální velikost.

Splnění odpovídá konzistentní posloupnosti konfigurací končící v $q_f$ ⇔ akceptace $M$ na $w$.

### 3.7 ⭐⭐ Polynomiální redukce — kompletní katalog NPC úloh (*doh §4.4*)

> **Co je nutné umět u státnice.** Komise se obvykle ptá: (1) **definuj** úlohu, (2) **ukaž, že je v NP** (certifikát + verifikace), (3) **proveď redukci z některé známé NPC** úlohy — a očekává, že kandidát zvládne aspoň jeden řetězec do hloubky. Nejpoužívanější otázky podle archivu OI-Wiki: TSP, vrcholové pokrytí, 3-barevnost, klika a ILP přes SAT. Níže jsou **všechny redukce, které Demlová v učebnici §4.4 dokládá**.

**Metoda (*doh §4.4.1*).** Aby byla $V$ NPC, stačí ověřit:
1. $V \in $ NP (najít polynomiální verifikaci certifikátu).
2. Najít $U \in $ NPC s $U \propto_p V$ (polynomiální redukce **z** $U$ **na** $V$).

```
              SAT (Cook 1971)
                │
        SAT → 3-CNF-SAT
                │
                ├──→ 3-barevnost ──→ ILP (0-1)
                │           │
                │           └──→ problém rozkladu ──→ SubsetSum ──→ batoh, dělení kořisti
                │
                └──→ klika ──→ nezávislé množiny ──→ vrcholové pokrytí
                                                            │
                                                            ├──→ ham. cyklus (orient.) ──→ ham. cesta ──→ nejdelší cesty ──→ nejkratší cesty v orient. grafu (se zápornými cykly)
                                                            │
                                                            └──→ ham. kružnice (neorient.) ──→ TSP
```

#### 3.7.1 SAT → 3-CNF-SAT (*doh §4.4.2–4.4.5*) ⭐

**3-CNF-SAT.** Vstup: formule $\varphi$ v CNF, kde každá klauzule má **přesně 3 literály**. Otázka: je $\varphi$ splnitelná?

**Převod.** Pro každou klauzuli $C = l_1 \lor l_2 \lor \dots \lor l_s$ s $s > 3$ literály zavedeme $s-3$ nových proměnných $x_1, \dots, x_{s-3}$ a nahradíme ji řetězcem klauzulí:
$$\psi_C = (l_1 \lor l_2 \lor x_1) \land (\lnot x_1 \lor l_3 \lor x_2) \land (\lnot x_2 \lor l_4 \lor x_3) \land \dots \land (\lnot x_{s-3} \lor l_{s-1} \lor l_s).$$

**Správnost:** $\psi_C$ je splnitelné ⇔ $C$ je splnitelné. Důkaz: pokud $\psi_C$ splnitelné a žádný z $l_1, \dots, l_s$ není pravdivý, pak řetězec implikací nutí $x_1 = \dots = x_{s-3} = $ TRUE a poslední klauzule $(\lnot x_{s-3} \lor l_{s-1} \lor l_s)$ je nesplněna — spor. Naopak pokud $l_k$ je pravdivý, zvolíme $x_1 = \dots = x_{k-2} = $ TRUE, ostatní FALSE.

**Složitost:** přidáme max $(s-3)k$ nových proměnných a prodloužíme formuli o max $2(s-3)k$ literálů ⇒ polynomiální.

#### 3.7.2 3-CNF-SAT → 3-barevnost (*doh §4.4.6–4.4.10*) ⭐

**$k$-barevnost grafu.** Obarvení vrcholů = funkce $b: V \to B$ (množina barev, $|B| = k$) tak, že $\{u, v\} \in E \Rightarrow b(u) \ne b(v)$. Otázka: je daný graf $G$ $k$-barevný?

**Pomocný graf $G_1$.** 5 vrcholů $\{a, b, c, d, e\}$ a 5 hran, takový že:
- pokud $a, b$ mají **stejnou** barvu, pak $e$ musí mít tutéž barvu;
- pokud jeden z $a, b$ má barvu $z$, lze $G_1$ obarvit tak, aby $e$ měl barvu $z$.

**Konstrukce hlavního grafu $G$ z formule $\varphi$:**

Označme $x_1, \dots, x_n$ všechny proměnné. Vrcholy $G$:
- pro každou proměnnou $x_i$: literály $x_i, \lnot x_i$,
- tři kalibrační vrcholy $R, G, B$ (červená/zelená/modrá).

Hrany $G$:
- **trojúhelník $\{R, G, B\}$** — nutí tyto tři vrcholy mít 3 různé barvy.
- pro každé $i$: **trojúhelník $\{B, x_i, \lnot x_i\}$** — vynucuje, aby $x_i$ a $\lnot x_i$ měly barvy z $\{R, G\}$ a navíc různé.
- pro každou klauzuli $C = l_1 \lor l_2 \lor l_3$: dvě kopie $G_1$. V první kopii odpovídají vrcholy $l_2, l_3$ vrcholům $a, b$, vrcholy $l_1$ a $e$ druhé kopie pak $a, b$ druhé kopie, a vrchol $R$ je spojen s $e$ druhé kopie.

**Korespondence:** Obarvení $b(R) = $ červená, $b(G) = $ zelená, $b(B) = $ modrá. Literál → zelená ⇔ je pravdivý (jinak červená). Vlastnost $G_1$ zajistí, že **klauzule je splněna** (alespoň jeden literál zelený) ⇔ **graf je 3-barevný**.

**Důsledek:** $\varphi$ splnitelná ⇔ $G$ je 3-barevný. Velikost $G$: polynomiální v délce $\varphi$.

> **Poznámka — obecná $k$-barevnost.** Pro každé pevné $k \ge 3$ je $k$-barevnost NPC. **2-barevnost** je v P (test bipartitnosti pomocí BFS — $O(n+m)$).

#### 3.7.3 3-barevnost → ILP (0-1) (*doh §4.4.11–4.4.13*)

**Celočíselné lineární programování** (ILP, *integer linear programming*). Otázka: má daný systém lineárních (ne)rovnic s celočíselnými proměnnými přípustné řešení?

**Převod.** Pro graf $G = (V, E)$ a 3 barvy $\{c, m, z\}$ definujeme **0-1 ILP** instanci:

**Proměnné** (pro každý vrchol $v$): $x^c_v, x^m_v, x^z_v \in \{0, 1\}$ (1 ⇔ vrchol $v$ má danou barvu).

**Podmínky:**
- Pro každý vrchol $v$: $\;x^c_v + x^m_v + x^z_v = 1\;$ (právě jedna barva).
- Pro každou hranu $e = \{u, v\}$ a každou barvu $b \in \{c, m, z\}$: $\;x^b_u + x^b_v \le 1\;$ (sousedi nesmějí mít stejnou barvu).

**Velikost instance:** $3|V|$ proměnných, $|V| + 3|E|$ podmínek $= O(n + m)$. Polynomiální.

**Korespondence:** ILP má přípustné řešení ⇔ $G$ je 3-barevný.

#### 3.7.4 3-barevnost → problém rozkladu (*doh §4.4.14–4.4.17*)

**Problém rozkladu** (*exact cover*, *X3C*). Vstup: konečná množina $X$ a systém podmnožin $S \subseteq 2^X$. Otázka: existuje $A \subseteq S$ takové, že $A$ je rozklad $X$ (disjunktní pokrytí)?

**Převod z 3-barevnosti $G = (V, E)$:**

**Prvky množiny $X$ (celkem $4|V| + 6|E|$):**
- pro každý vrchol $v$: čtyři prvky $v, p^c_v, p^m_v, p^z_v$,
- pro každou hranu $e = \{u, v\}$: šest prvků $q^c_{uv}, q^m_{uv}, q^z_{uv}, q^c_{vu}, q^m_{vu}, q^z_{vu}$.

**Podmnožiny v $S$:**

1. **„Volba barvy vrcholu"** — pro každý $v$: $\{v, p^c_v\}, \{v, p^m_v\}, \{v, p^z_v\}$. Výběrem rozkladu se vybere právě jedna z těchto trojic ⇒ určuje se barva $v$.

2. **„Posedlost zbylých barev"** — pro každý $v$ a každou barvu $b$ označme $N(v) = \{u : \{u,v\} \in E\}$. Definujeme $S^b_v = \{p^b_v\} \cup \{q^b_{vu} : u \in N(v)\}$. Tj. množina obsahuje "zbytkový" $p^b_v$ a všechny hraniční $q^b_{vu}$ pro hrany incidentní s $v$.

3. **„Konsistence barvy na hraně"** — pro každou hranu $e = \{u, v\}$ šest dvouprvkových množin pro všechny páry **různých** barev: $\{q^c_{uv}, q^m_{vu}\}, \{q^c_{uv}, q^z_{vu}\}, \{q^m_{uv}, q^c_{vu}\}, \{q^m_{uv}, q^z_{vu}\}, \{q^z_{uv}, q^c_{vu}\}, \{q^z_{uv}, q^m_{vu}\}$. Pouze tyto, **nikoli** páry stejné barvy — to zaručuje, že barvy sousedních vrcholů jsou různé.

**Velikost:** $3|V| + 3|V| + 6|E|$ množin $\Rightarrow$ polynomiální.

**Korespondence:** Pokud je $G$ 3-barevný s barvou $b$, vybereme do rozkladu: $\{v, p^{b(v)}_v\}$, dvě množiny $S^{b_1}_v, S^{b_2}_v$ pro nezvolené barvy, a $\{q^{b(u)}_{uv}, q^{b(v)}_{vu}\}$ pro každou hranu (existuje v $S$ právě proto, že $b(u) \ne b(v)$). Naopak rozklad dává obarvení podle výběru z 1).

#### 3.7.5 Problém rozkladu → SubsetSum (*doh §4.4.18–4.4.21*)

**SubsetSum.** Vstup: kladná čísla $a_1, \dots, a_n$ a cíl $K$. Otázka: existuje $J \subseteq \{1, \dots, n\}$ s $\sum_{i \in J} a_i = K$?

**Převod.** Přejmenujeme prvky $X = \{0, 1, \dots, n-1\}$, $S = \{S_1, \dots, S_r\}$. Zvolíme základ $p > r$ (počet množin, tj. cifry se nepřenášejí). Pro každou množinu $S_i$ definujeme číslo zápisem v soustavě o základu $p$:
$$a_i = \sum_{j=0}^{n-1} \chi_{S_i}(j) \cdot p^j, \quad \text{kde } \chi_{S_i}(j) = \begin{cases} 1 & j \in S_i \\ 0 & j \notin S_i \end{cases}.$$

Cíl: $K = \sum_{i=0}^{n-1} p^i = p^0 + p^1 + \dots + p^{n-1}$.

**Korespondence:** $\sum_{i \in J} a_i = K$ ⇔ $\{S_i : i \in J\}$ je rozklad $X$. Klíčové: protože $p > r$, není nikdy "přenos" mezi ciframi — každá cifra v $p$-soustavě musí mít přesně 1 jedničku, tj. každý prvek $X$ je pokryt právě jednou.

#### 3.7.6 SubsetSum → batoh, dělení kořisti (*doh §4.4.22*)

**Batoh** (knapsack, decision version). Vstup: $n$ předmětů s váhami $w_i$ a cenami $c_i$, kapacita $W$, požadovaná cena $C$. Otázka: existuje výběr $J$ s $\sum_J w_i \le W$ a $\sum_J c_i \ge C$?

**Dělení kořisti** (partition). Vstup: čísla $a_1, \dots, a_n$. Otázka: lze je rozdělit na 2 disjunktní podmnožiny se stejnými součty?

**Převod ze SubsetSum** (snadný):
- **Batoh:** vezmi $w_i = c_i = a_i$, $W = C = K$.
- **Dělení kořisti:** pro SubsetSum s cílem $K$ a součtem $S$: přidej prvek $a_{n+1} = |S - 2K|$; pak dělení kořisti existuje ⇔ SubsetSum má řešení.

⇒ **batoh a dělení kořisti jsou NPC.**

> **Pseudopolynomiální algoritmus** (přednáška T9). Batoh má **DP algoritmus s časem $O(nW)$**, který je polynomiální v $n$ a *hodnotě* (nikoli v délce zápisu) $W$. To je tzv. **pseudopolynomiální** algoritmus. Existuje pro batoh, neexistuje pro TSP, který je **silně NP-úplný** (i pro malá čísla zůstává NPC).

#### 3.7.7 3-CNF-SAT → klika (*doh §4.4.23–4.4.26*) ⭐

**Problém klik** ($k$-clique). Vstup: graf $G$, číslo $k$. Otázka: existuje v $G$ klika (úplný podgraf) o $\ge k$ vrcholech?

**Převod.** Mějme $\varphi$ s $k$ klauzulemi $C_1, \dots, C_k$, každá má 3 literály.

**Graf $G$ — $k$-partitní:**
- pro každou klauzuli $C_j$: jedna "strana" se 3 vrcholy odpovídajícími literálům klauzule,
- **hrana** mezi vrcholy $l$ (v $C_i$) a $l'$ (v $C_j$, $i \ne j$) **právě tehdy, když nejsou komplementární** (tj. $l' \ne \lnot l$); v rámci jedné strany žádné hrany.

**Korespondence:** $\varphi$ splnitelná ⇔ $G$ má kliku velikosti $k$.

- **⇒:** Ohodnocení splňuje $\varphi$ ⇒ v každé klauzuli vyber jeden pravdivý literál. Vybrané vrcholy tvoří kliku (různé strany, žádné dva komplementární).
- **⇐:** Klika velikosti $k$ má právě 1 vrchol v každé straně. Nastavíme literály odpovídající vrcholům kliky na TRUE (nejsou komplementární ⇒ konzistentní). Každá klauzule má pravdivý literál.

**Velikost:** $3k$ vrcholů, $O(k^2)$ hran ⇒ polynomiální.

#### 3.7.8 Klika → nezávislé množiny (*doh §4.4.27–4.4.30*)

**Nezávislá množina** (independent set). $N \subseteq V$ taková, že žádná hrana grafu $G$ nemá oba konce v $N$. Vstup: $G$, $k$. Otázka: existuje nezávislá množina o $k$ vrcholech?

**Převod.** Pro daný $G = (V, E)$ definuj **doplňkový (opačný) graf** $G^{op} = (V, E^{op})$:
$$\{u, v\} \in E^{op} \;\;\Longleftrightarrow\;\; u \ne v \land \{u, v\} \notin E.$$

**Pozorování:** $A \subseteq V$ je klika v $G$ ⇔ $A$ je nezávislá množina v $G^{op}$.

⇒ Otázka „existuje v $G$ klika o $k$ vrcholech?" ⇔ „existuje v $G^{op}$ nezávislá množina o $k$ vrcholech?".

**Velikost:** $|V|$ se nemění, počet hran $|E| + |E^{op}| = \binom{n}{2}$ ⇒ polynomiální.

#### 3.7.9 Nezávislé množiny → vrcholové pokrytí (*doh §4.4.31–4.4.34*) ⭐

**Vrcholové pokrytí** (vertex cover). $B \subseteq V$ tak, že každá hrana má aspoň 1 konec v $B$. Vstup: $G$, $k$. Otázka: existuje pokrytí o $\le k$ vrcholech? (Hledáme minimum — $V$ je triviálně pokrytí.)

**Klíčové pozorování:**
$$N \subseteq V \text{ je nezávislá} \;\;\Longleftrightarrow\;\; V \setminus N \text{ je vrcholové pokrytí.}$$

**Důkaz:** $N$ nezávislá ⇔ žádná hrana není uvnitř $N$ ⇔ každá hrana má aspoň 1 konec mimo $N$, tj. v $V \setminus N$ ⇔ $V \setminus N$ je pokrytí.

⇒ V $G$ existuje nezávislá množina o $k$ vrcholech ⇔ v $G$ existuje vrcholové pokrytí o $n-k$ vrcholech (kde $n = |V|$).

**Velikost:** triviálně polynomiální.

#### 3.7.10 Vrcholové pokrytí → ham. cyklus (orientovaný) (*doh §4.4.35–4.4.38*)

**Hamiltonovský cyklus** (orient.). Vstup: orientovaný graf $G$. Otázka: existuje cyklus procházející **každým** vrcholem právě jednou?

**Idea převodu.** Speciální **pomocný graf $H$** o 4 vrcholech a 6 orientovaných hranách s vlastností: ham. cyklus musí $H$ projít buď (a) jedním kontinuálním průchodem všech 4 vrcholů, nebo (b) dvojím průchodem, který pokaždé navštíví 2 a 2.

**Konstrukce $G'$ z $G = (V, E)$:**
- za **každou hranu** $G$ vložíme kopii grafu $H$,
- přidáme **selektorové vrcholy** $1, 2, \dots, k$ (k = velikost hledaného pokrytí),
- hrany propojují kopie $H$ tak, aby cesta selektoru $j$ mohla "vstoupit" do $H$ pro hrany incidentní s vrcholy v pokrytí,
- selektory propojené tak, že každý ham. cyklus musí vybrat přesně $k$ vrcholů do pokrytí.

**Velikost:** $4|E| + k$ vrcholů, $O(|E| + k|V|)$ hran ⇒ polynomiální.

**Korespondence:** $G$ má vrcholové pokrytí o $k$ vrcholech ⇔ $G'$ má hamiltonovský cyklus.

#### 3.7.11 Vrcholové pokrytí → ham. kružnice (neorientovaná) (*doh §4.4.39–4.4.40*)

Konstrukce analogická k 3.7.10, pomocný graf je technicky složitější, ale princip stejný. Důsledek: **ham. kružnice ∈ NPC**.

> **Rozdíl ham. cyklus vs ham. kružnice:** *cyklus* obecně může být orientovaný i neorientovaný; *kružnice* v Demlové konvenci značí neorientovanou variantu (jeden uzavřený sled bez opakování vrcholů). Mnoho autorů (Cormen et al.) říká **ham. cycle** v obou případech.

#### 3.7.12 Ham. kružnice → TSP (*doh §4.4.41–4.4.42*) ⭐

**Problém obchodního cestujícího** (TSP). Vstup: úplný graf $K_n$ s nezápornými vzdálenostmi $d(i, j)$, číslo $K$. Otázka: existuje uzavřená trasa procházející všemi vrcholy s celkovou délkou $\le K$?

**Převod z ham. kružnice $G = (V, E)$ na úplném grafu $K_n$ se vzdálenostmi**:
$$d(i, j) = \begin{cases} 1 & \{i, j\} \in E \\ 2 & \{i, j\} \notin E \end{cases}$$
a $K = n$.

**Korespondence:** $G$ má ham. kružnici ⇔ TSP najde trasu délky $\le n$ (totiž přesně $n$, pomocí samých hran $G$).

#### 3.7.13 Ham. cyklus → orient. ham. cesta (*doh §4.4.43–4.4.44*)

**Hamiltonovská cesta.** Vstup: orient. graf $G$, vrcholy $s, t$. Otázka: existuje cesta z $s$ do $t$ procházející **každým** vrcholem **právě jednou**?

**Převod.** Pro vstup ham. cyklu na grafu $G$ se zvoleným startovním vrcholem $v_0$:
- "rozřízneme" $v_0$ na dva vrcholy $v_0^{\text{in}}$ a $v_0^{\text{out}}$,
- všechny vstupní hrany $v_0$ směřujeme do $v_0^{\text{in}}$, výstupní vychází z $v_0^{\text{out}}$,
- ptáme se na ham. cestu z $v_0^{\text{out}}$ do $v_0^{\text{in}}$.

**Korespondence:** ham. cyklus přes $v_0$ ⇔ ham. cesta v rozříznutém grafu.

#### 3.7.14 Ham. cesta → nejdelší cesty → nejkratší cesty v orient. grafu (*doh §4.4.45–4.4.48*)

**Nejdelší cesta** (longest path) v orient. grafu. Vstup: $G$, $s, t$, číslo $K$. Otázka: existuje (jednoduchá) cesta z $s$ do $t$ délky $\ge K$?

**Převod z ham. cesty:** vezmi všechny hrany s váhou 1 a $K = n - 1$. Ham. cesta ⇔ jednoduchá cesta s $n - 1$ hranami.

⇒ **nejdelší cesta v orient. grafu ∈ NPC.**

**Nejkratší cesta v orient. grafu se zápornými cykly** (longest paths se redukují přes záporné váhy):

**Převod:** Z nejdelší cesty s váhami $w(e)$ vytvoř nejkratší cestu s váhami $-w(e)$ — jednoduchá cesta délky $\ge K$ ⇔ jednoduchá cesta délky $\le -K$. (Pozn.: Pro neorientovaný graf hledání jednoduchých nejkratších cest se zápornými hranami zahrnuje NPC, protože by mohlo cyklit.)

⇒ **nejkratší cesta v orient. grafu se zápornými cykly ∈ NPC.**

> **Pozor — kontrast s P:** Nejkratší cesta v orientovaném grafu **bez záporných cyklů** je v P (Bellman-Ford $O(mn)$). Nejkratší cesta s **nezápornými** hranami je v P (Dijkstra $O(m + n \lg n)$).

#### 3.7.15 Obecná $k$-barevnost a další NPC úlohy

**$k$-barevnost pro pevné $k \ge 3$** je NPC. Redukce $3$-color $\propto_p\ k$-color: stačí přidat klikku $K_{k-3}$ a propojit ji se všemi vrcholy původního grafu — nové vrcholy potřebují $k - 3$ nových barev, původní vrcholy zbývající 3 barvy.

**Další klasické NPC úlohy (mimo Demlové textu, ale komise je může zmínit):**

| Úloha | Vstup | Otázka |
|---|---|---|
| **3-DIM-MATCHING** | 3 disjunktní množiny $X, Y, Z$ velikosti $n$, $T \subseteq X \times Y \times Z$ | ∃ perfektní matching v $T$? |
| **SET-COVER** | množina $U$, systém $S$, $k$ | ∃ podmnožina $\le k$ z $S$ pokrývající $U$? |
| **DOMINATING SET** | graf $G$, $k$ | ∃ $D \subseteq V$ velikosti $\le k$ tak, že každý vrchol je v $D$ nebo má souseda v $D$? |
| **FEEDBACK VERTEX SET** | orient. graf, $k$ | ∃ $F$ vrcholů, jejichž odebrání udělá graf acyklickým? |
| **3-DIMENSIONAL SAT** (varianty) | různě | různě |

⇒ Všechny řešitelné polynomiální redukcí z některé výše uvedené NPC. Demlová je explicitně neuvádí, ale konceptuálně sem patří.

#### 3.7.16 Polynomiální "bratranci" NPC úloh — kontrast

Užitečné pro odpověď u státnice „čím je $X$ zajímavé":

| NPC úloha | Polynomiální "bratranec" v P | Klíčový rozdíl |
|---|---|---|
| **SAT** | **2-SAT** (klauzule max 2 literály) | Implikační graf + SCC, $O(n + m)$ |
| **3-SAT** | **HornSAT** (klauzule max 1 pozitivní literál) | Unit propagation, lineární čas |
| **3-barevnost** | **2-barevnost** (bipartitní test) | BFS, $O(n + m)$ |
| **Klika** | **2-klika** (existence hrany) | $O(m)$ |
| **TSP** | **TSP na stromu** | DP po stromu, $O(n)$ |
| **Ham. cyklus** | **Eulerovský cyklus** (každou hranu právě 1×) | Test sudého stupně, lineární |
| **Vrcholové pokrytí v obecném $G$** | **Vrcholové pokrytí v bipartitním $G$** (Königova věta) | Max matching, $O(m \sqrt n)$ |
| **Batoh** | **Zlomkový batoh** | Greedy podle ceny/váhy, $O(n \lg n)$ |
| **ILP** | **LP** (bez celočíselnosti) | Polynomiálně řešitelné (simplex, vnitřní bod) |
| **Nejdelší cesta** | **Nejdelší cesta v DAG** | Topologické pořadí + DP, $O(n + m)$ |

> **Lekce:** **Drobná změna v zadání** (omezení literálů, struktura grafu, povolení zlomků) může změnit P ⇒ NPC. To je klasická "past" otázka — komise se ráda ptá „a co kdybychom místo $X$ uvážili $Y$, je to ještě NPC?".

### 3.8 ⭐ Heuristiky a aproximace pro NPC úlohy (*doh §4.4.49–4.4.59*)

**Heuristika** = polynomiální algoritmus bez záruky kvality.
**Aproximační algoritmus** = polynomiální algoritmus s **garantovanou** kvalitou.

**$R$-aproximační algoritmus** pro minimalizaci: pro každou instanci $I$ dává řešení s hodnotou $A(I)$ splňující
$$A(I) \le R \cdot \text{OPT}(I), \quad R \ge 1.$$
(Pro maximalizaci: $A(I) \ge R \cdot \text{OPT}(I)$ s $R \le 1$.)

#### 3.8.1 Vrcholové pokrytí — 2-aproximace (*doh §4.4.51–4.4.52*)
```
C := ∅
while E ≠ ∅:
  vyber libovolnou hranu {u, v}
  C := C ∪ {u, v}
  odstraň u, v + všechny incidentní hrany
```
**Tvrzení:** $|C| \le 2 |C_{\min}|$.

**Důkaz:** Vybrané hrany tvoří párování $F$ ⇒ $|C| = 2|F|$. Žádná hrana z $F$ nesdílí vrchol s jinou ⇒ jakékoliv pokrytí musí mít aspoň 1 vrchol z každé hrany v $F$ ⇒ $|C_{\min}| \ge |F|$. Tedy $|C| = 2|F| \le 2|C_{\min}|$.

> **Naivní heuristika "ber vrchol s největším stupněm"** je **horší než 2-aproximace** — existují grafy, kde najde $\Theta(k \log k)$ vrcholů místo optimálních $k$.

#### 3.8.2 TSP — obecně neaproximovatelný (*doh §4.4.54–4.4.55*)

**Věta:** Kdyby pro TSP existoval $r$-aproximační algoritmus, pak $\mathbf{P} = \mathbf{NP}$.

**Důkaz redukcí z ham. kružnice:** Pro graf $G$ definujeme TSP na úplném grafu $K_n$ se vzdálenostmi $d(i,j) = 1$ pro hrany $G$, $d(i,j) = rn+1$ jinak. Pokud $G$ má ham. kružnici, OPT = $n$; jinak OPT $> rn$. $r$-aproximace by rozlišila tyto případy a tedy rozhodla ham. kružnici v polynomiálním čase.

#### 3.8.3 Metric TSP (trojúhelníková nerovnost) — aproximace existuje ⭐

**Předpoklad:** $d(i, j) \le d(i, k) + d(k, j)$ (Δ-nerovnost).

**2-aproximace** (*doh §4.4.57–4.4.58*):
1. Najdi minimální kostru $(V, K)$.
2. DFS průchod $K$.
3. Trasa $T$ = pořadí prvních návštěv vrcholů.

Důkaz: $|K| \le \text{OPT}$ (odebráním libovolné hrany ham. kružnice dostáváme kostru). DFS prochází každou hranu kostry 2× ⇒ $|T| \le 2|K| \le 2\,\text{OPT}$.

**Christofidesův algoritmus — 3/2-aproximace** (*doh §4.4.59*):
1. Min kostra $(V, K)$.
2. Vrcholy lichého stupně v $K$ → indukovaný úplný graf $H$.
3. Najdi nejlevnější perfektní párování $P$ v $H$.
4. Eulerovský tah v $(V, P \cup K)$ (každý vrchol má sudý stupeň).
5. Trasa $T$ = pořadí prvních návštěv.

Garance $|T| \le (3/2) \cdot \text{OPT}$. Demlová (*doh §4.4.60*) uvádí, že **odhady jak pro 2-aproximaci, tak pro Christofidesův algoritmus nelze (v rámci těchto metod) zlepšit**. *Pozn.: v obecné literatuře byl poměr $3/2$ pro metrický TSP v r. 2021 těsně překonán (Karlin–Klein–Oveis Gharan), ale to už je nad rámec kurzu.*

### 3.9 Třída co-NP (*doh §4.5*)

$$\boxed{\; L \in \mathbf{co\text{-}NP} \;\;\Longleftrightarrow\;\; L^c \in \mathbf{NP}.\;}$$

Doplňky NP-úloh ležící v NP automaticky neumíme dokázat.

**Příklady co-NP:**
- USAT (nesplnitelné formule) ∈ co-NP. Naopak USAT ∈ NP ⇔ ∃ krátký certifikát nesplnitelnosti, což je otevřený problém.
- TAUT (tautologie) ∈ co-NP.

**Otázka co-NP = NP je otevřená.** Důsledek: $\mathbf{co\text{-}NP} = \mathbf{NP} \Leftrightarrow$ ∃ NPC úloha, jejíž doplněk je v NP (*doh §4.5.6*).

**Lemma (*doh §4.5.5*):** $L_1 \propto_p L_2 \Rightarrow L_1^c \propto_p L_2^c$.

---

## 4. Třídy paměťové složitosti PSPACE a NPSPACE. Savitchova věta

### 4.1 Definice (*doh §4.6*)

$$\boxed{\; L \in \mathbf{PSPACE} \;\;\Longleftrightarrow\;\; \exists\,\text{DTM přijímající } L \text{ s polynomiální paměť. složitostí}.\;}$$

$$\boxed{\; L \in \mathbf{NPSPACE} \;\;\Longleftrightarrow\;\; \exists\,\text{NTM přijímající } L \text{ s polynomiální paměť. složitostí}.\;}$$

**Pozor:** TM s polynomiální pamětí **nemusí** být polynomiálně omezený v čase, ani se nemusí zastavit (může se zacyklit v rámci polynomu buněk).

**Triviální inkluze:** $\mathbf{P} \subseteq \mathbf{PSPACE}$, $\mathbf{NP} \subseteq \mathbf{NPSPACE}$ — polynomiální čas implikuje polynomiální paměť.

### 4.2 Omezení počtu konfigurací (*doh §4.6.6*)

**Věta.** Existuje konstanta $c$ závislá na TM tak, že TM s paměťovou složitostí $p(n)$ se na vstupu délky $n$ buď zastaví po nejvýše $c^{p(n) + 1}$ krocích, nebo se nezastaví nikdy.

**Myšlenka:** Počet různých konfigurací je $\le p(n) \cdot |Q| \cdot |\Gamma|^{p(n)} < c^{p(n) + 1}$ pro $c = |\Gamma| + |Q|$. Pokud TM nezopakuje konfiguraci do $c^{p(n)+1}$ kroků, musí se zastavit; pokud zopakuje, je v nekonečné smyčce.

**Důsledek (*doh §4.6.8*):** Pokud $L \in $ PSPACE (resp. NPSPACE), lze zvolit TM, který se vždy zastaví po max $c^{q(n)}$ krocích.

### 4.3 ⭐⭐ Savitchova věta (*doh §4.6.10*)

$$\boxed{\;\mathbf{PSPACE = NPSPACE.}\;}$$

Tj. nedeterminismus nezvyšuje sílu z hlediska paměti (zatímco pro čas je to otevřený problém).

**Nástin důkazu** (NPSPACE ⊆ PSPACE):

Nechť $M$ je NTM přijímající $L$ s pam. složitostí $p(n)$. Deterministicky simulujeme $M$ s paměťovou složitostí $O(p^2(n))$.

**Procedura DOSTUP$(I, J, m)$** — vrací TRUE/FALSE: "lze se dostat z konfigurace $I$ do $J$ v ≤ $m$ krocích":
```
DOSTUP(I, J, m):
  if m == 1:
    return (I == J) OR (I ⊢ J v jednom kroku M)
  for každou konfiguraci K (velikosti ≤ p(n)):
    if DOSTUP(I, K, ⌊m/2⌋) AND DOSTUP(K, J, ⌈m/2⌉):
      return TRUE
  return FALSE
```

**Hlavní výpočet:** přijetí $w$ existuje ⇔ DOSTUP$(I_0, I_f, 2^{c \cdot p(n)})$ = TRUE.

**Analýza paměti:** rekurzivní strom hloubky $\log_2 m = O(p(n))$; na každém zásobníku $O(p(n))$ paměti pro trojici $(I, K, m)$ ⇒ celkem $O(p^2(n))$.

### 4.4 Hierarchie tříd

$$\mathbf{P} \subseteq \mathbf{NP} \subseteq \mathbf{PSPACE} = \mathbf{NPSPACE}.$$

Demlová dokazuje $\mathbf{P} \subseteq \mathbf{PSPACE}$ (*§4.6.3*), $\mathbf{NP} \subseteq \mathbf{NPSPACE}$ (*§4.6.5*) a $\mathbf{PSPACE} = \mathbf{NPSPACE}$ (Savitch, *§4.6.10*). Žádná z inkluzí $\mathbf{P} \subseteq \mathbf{NP} \subseteq \mathbf{PSPACE}$ není dokázána jako striktní.

> **Mimo text Demlové (kontext).** Doplníme-li třídu $\mathbf{EXPTIME}$, platí $\mathbf{P} \subseteq \mathbf{NP} \subseteq \mathbf{PSPACE} \subseteq \mathbf{EXPTIME}$ a z věty o časové hierarchii víme $\mathbf{P} \subsetneq \mathbf{EXPTIME}$. Z toho plyne pouze to, že **alespoň jedna inkluze v celém řetězci $\mathbf{P} \subseteq \mathbf{NP} \subseteq \mathbf{PSPACE} \subseteq \mathbf{EXPTIME}$ je striktní** — *nikoli* nutně některá z těch před PSPACE. Konkrétně $\mathbf{P} \subsetneq \mathbf{PSPACE}$ **dosud dokázáno není**.

---

## 5. Pravděpodobnostní algoritmy. Randomizované TM. Třídy RP, ZPP, co-RP

### 5.1 ⭐ Randomizovaný TM (RTM) (*doh §4.8.1*)

RTM má **2 a více pásek**:
- 1. páska — vstupní (jako u DTM)
- 2. páska — **náhodná pozadí (random tape)**: posloupnost bitů 0/1, každý bit s pravděpodobností 1/2, nezávisle. Tato páska se **nepřepisuje** (read-only random source).

Přechodová funkce:
$$\delta: (Q \setminus F) \times \Gamma \times \{0, 1\} \to Q \times \Gamma \times \{L, R, S\}^2$$

**Praktická poznámka:** bity druhé pásky se generují **lazy** (až když je hlava potřebuje), takže RTM = "DTM s přístupem k toku náhodných bitů".

### 5.2 ⭐ Třída RP — Monte-Carlo (*doh §4.8.4*)

$$\boxed{\; L \in \mathbf{RP} \;\;\Longleftrightarrow\;\; \exists\,\text{RTM } M:\;}$$
1. $w \notin L \Rightarrow \Pr[M \text{ přijme } w] = 0$ (**žádné falešné ANO**)
2. $w \in L \Rightarrow \Pr[M \text{ přijme } w] \ge 1/2$
3. ∃ polynom $p(n)$: každý běh ≤ $p(n)$ kroků

**Monte-Carlo charakter:** odpověď "ANO" je důvěryhodná; "NE" může být falešné (s pravděpodobností < 1/2 v jednom běhu).

**Příklad:** Miller-Rabinův test prvočíselnosti ukazuje, že $L_s$ (složená čísla) ∈ RP.

### 5.3 ⭐ Boost — zesílení pravděpodobnosti (*doh §4.8.6*)

**Tvrzení:** Pokud $L \in $ RP, pak pro **libovolné** $0 < c < 1/2$ existuje polynomiální RTM se $\Pr[\text{úspěch} \mid w \in L] \ge 1 - c$.

**Konstrukce:** Spustíme původní RTM $k$-krát nezávisle. Pokud aspoň jeden běh řekne ANO ⇒ vrátíme ANO. Pravděpodobnost úplného selhání $\le (1/2)^k$. Pro $k = \lceil \log_2(1/c) \rceil$ dostaneme požadované.

Po 100 nezávislých bězích je chyba $< 2^{-100}$ — astronomicky nižší než pravděpodobnost hardwarové chyby počítače.

### 5.4 ⭐ Třída ZPP — Las-Vegas (*doh §4.8.7–4.8.9*)

$$\boxed{\; L \in \mathbf{ZPP} \;\;\Longleftrightarrow\;\; \exists\,\text{RTM } M:\;}$$
1. $w \notin L \Rightarrow \Pr[\text{přijme}] = 0$
2. $w \in L \Rightarrow \Pr[\text{přijme}] = 1$ (**vždy správná odpověď**)
3. $\mathbb{E}[\text{počet kroků}] = p(n)$ polynom (**pouze očekávaný** polynomiální čas)

**Las-Vegas charakter:** odpověď je vždy správná, ale doba běhu je náhodná veličina.

**Tvrzení:** $L \in $ ZPP $\Rightarrow L^c \in $ ZPP. (Prohození koncových/nekoncových stavů — Las-Vegas symetrický k oběma stranám.)

Pro RP **analogie neplatí** (RP je asymetrická). Proto definujeme:

### 5.5 Třída co-RP (*doh §4.8.11*)

$$\mathbf{co\text{-}RP} = \{ L \mid L^c \in \mathbf{RP} \}.$$

### 5.6 ⭐ Klíčová věta o vztazích (*doh §4.8.12*)

$$\boxed{\;\mathbf{ZPP = RP \cap co\text{-}RP.}\;}$$

**Důkaz $\supseteq$:** Nechť $M_1$ je Monte-Carlo RTM pro $L$ (RP) a $M_2$ Monte-Carlo pro $L^c$ (co-RP), oba s polynomiálním stop-časem $p$. Sestrojíme Las-Vegas $M$: střídá kroky $M_1$ a $M_2$ po blocích po $p(n)$ krocích — vrátí odpověď, jakmile jeden řekne ANO. **Vždy správná odpověď** (Monte-Carlo žádné falešné ANO).

**Důkaz $\subseteq$:** Nechť $M_1$ je Las-Vegas pro $L$ s $\mathbb{E} = p(n)$. Markovova nerovnost: $\Pr[\text{běh} > 2p(n)] \le 1/2$. Definujeme Monte-Carlo $M'$: spusť $M_1$ na $2p(n)$ kroků, pokud dosud nedoběhl, vrať NE.

### 5.7 Hierarchie pravděpodobnostních tříd (*doh §4.8.13*)

$$\mathbf{P} \subseteq \mathbf{ZPP} \subseteq \mathbf{RP} \subseteq \mathbf{NP}, \qquad \mathbf{ZPP} \subseteq \mathbf{co\text{-}RP} \subseteq \mathbf{co\text{-}NP}.$$

### 5.8 ⭐ Miller-Rabinův test prvočíselnosti (*doh §4.7.7–4.7.8*, přednáška T11)

**Vstup:** velké liché $n$. **Výstup:** "prvočíslo" / "složené".

**Algoritmus:**
1. Rozlož $n - 1 = 2^l m$, $m$ liché.
2. Náhodně zvol $a \in \{1, \dots, n-1\}$.
3. Spočti $a^m \bmod n$. Pokud $\equiv 1$ ⇒ vrať **"prvočíslo"**.
4. Postupně: $a^{2m}, a^{4m}, \dots, a^{2^l m} \bmod n$.
5. Pokud $a^{2^l m} \not\equiv 1 \bmod n$ ⇒ vrať **"složené"**.
6. Najdi nejmenší $k$: $a^{2^k m} \not\equiv 1 \land a^{2^{k+1} m} \equiv 1$. Pokud $a^{2^k m} \equiv -1$ ⇒ vrať **"prvočíslo"**; jinak vrať **"složené"**.

**Korektnost (*doh §4.7.8*):**
1. **Výstup "složené" ⇒ $n$ je opravdu složené.** (Malá Fermatova: pro prvočíslo $a^{p-1} \equiv 1$ ⇒ není krok 5; v tělese $\mathbb{Z}_p$ má 1 jen 2 odmocniny $\pm 1$ ⇒ není krok 6 jako "složené".)
2. **Výstup "prvočíslo" ⇒ $n$ je prvočíslo s pravděpodobností > 1/2.** (Mimo Carmichaelových čísel — pro ně argument využívá vlastní podgrupu $K \subset \mathbb{Z}_n^*$.)

⇒ $L_s$ (složená čísla) $\in $ **RP**, $L_p$ (prvočísla) $\in $ **co-RP**.

V kontextu kryptografie: po 100 iteracích Miller-Rabina je pravděpodobnost chybného označení $< 2^{-100}$, prakticky 0.

---

## 6. Rozhodnutelnost a nerozhodnutelnost. Rekursivní a rekursivně spočetné jazyky

### 6.1 ⭐ Definice (*doh §5.1.1–5.1.2*)

**Rekursivní jazyk** (R, *recursive language* = decidable):
$$L \in \mathbf{R} \;\;\Longleftrightarrow\;\; \exists\,\text{TM rozhodující } L\ (\text{přijímá a vždy se zastaví}).$$

**Rekursivně spočetný jazyk** (RS, *recursively enumerable* = semi-decidable):
$$L \in \mathbf{RS} \;\;\Longleftrightarrow\;\; \exists\,\text{TM přijímající } L\ (\text{pro } w \notin L \text{ se buď neúspěšně zastaví, nebo se nezastaví vůbec}).$$

**Vlastnosti**:
- $\mathbf{R} \subseteq \mathbf{RS}$ (zřejmé).
- $\mathbf{R} \ne \mathbf{RS}$ (striktně menší — viz dále).
- $L \in \mathbf{R} \Rightarrow L^c \in \mathbf{R}$ (stačí prohodit koncové a nekoncové stavy v rozhodujícím TM).
- $L, L^c \in \mathbf{RS} \Rightarrow L \in \mathbf{R}$ (paralelní simulace obou).

**Tři možnosti pro libovolný jazyk $L$ (*doh §5.1.6*):**
1. $L \in \mathbf{R}$ a $L^c \in \mathbf{R}$.
2. Jeden z $L, L^c$ je v RS, druhý ne (asymetrický případ — typický pro nerozhodnutelné úlohy).
3. Žádný z $L, L^c$ není v RS.

**"Nerekurzivní" = "algoritmicky neřešitelný" = "nerozhodnutelný"** — všechny tři termíny synonymní.

### 6.2 ⭐ Kód Turingova stroje (*doh §5.1.7*)

Standardní binární kódování TM $M = (Q, \{0,1\}, \Gamma, \delta, q_1, B, F = \{q_2\})$:
- stavy očíslujeme $q_1, q_2, \dots$, abeceda $X_1 = 0, X_2 = 1, X_3 = B, \dots$, směry $D_1 = L, D_2 = R$
- přechod $\delta(q_i, X_j) = (q_k, X_l, D_r)$ je kódován jako $w = 0^i 1\,0^j 1\,0^k 1\,0^l 1\,0^r$
- celý TM: $\langle M \rangle = 111\,w_1\,11\,w_2\,11\,\dots\,11\,w_p\,111$

**Očíslování binárních slov** (*doh §5.1.8*): slovo $w$ ↦ číslo $1w$ v binárním zápisu. Slovo $\varepsilon$ = 1., $0$ = 2., $1$ = 3., $00$ = 4., …, $100110$ = 102. slovo. Tato bijekce $\mathbb{N} \to \{0,1\}^*$ je základem diagonalizace.

### 6.3 ⭐⭐ Diagonální jazyk $L_d$ a diagonální argument (*doh §5.1.9–5.1.10*)

Slova nesplňující formát $\langle M \rangle$ považujeme za kód TM s prázdným jazykem.

$$\boxed{\; L_d = \{ w \mid \text{TM s kódem } w \text{ nepřijímá slovo } w \}.\;}$$

**Věta:** **Neexistuje TM přijímající $L_d$.** Tj. $L_d \notin \mathbf{RS}$.

**Diagonální argument** (Cantorův styl). Kdyby $L_d = L(M)$ pro nějaký TM $M$. Ten má kód $\langle M \rangle = w_i$ pro nějaké $i$. Dvě možnosti:
- $w_i \in L_d$ ⇒ z definice $L_d$: TM s kódem $w_i$ (= $M$) nepřijímá $w_i$ ⇒ $w_i \notin L(M) = L_d$. **Spor.**
- $w_i \notin L_d$ ⇒ TM $M$ přijímá $w_i$ ⇒ $w_i \in L(M) = L_d$. **Spor.**

⇒ $L_d$ neexistuje jako přijímaný jazyk žádného TM.

### 6.4 ⭐ Univerzální jazyk $L_{UN}$ a univerzální TM (*doh §5.1.11–5.1.14*)

$$\boxed{\; L_{UN} = \{ \langle M \rangle\,w \mid w \in L(M) \}.\;}$$

**Univerzální Turingův stroj** (universal TM): 4-páskový TM $U$:
- páska 1: vstup $\langle M \rangle\,w$
- páska 2: simuluje pásku simulovaného $M$
- páska 3: udržuje aktuální stav $M$
- páska 4: pomocná

$U$ načte $\langle M \rangle$, zkontroluje formát, zkopíruje $w$ na pásku 2, a krok za krokem simuluje výpočet $M$ na $w$.

**Důsledek:** $L_{UN} \in \mathbf{RS}$.

**Věta:** $L_{UN} \notin \mathbf{R}$ (jinak by se rozhodoval i $L_d$ — spor s 6.3).

⇒ $L_{UN}$ je v $\mathbf{RS} \setminus \mathbf{R}$ — **klasický příklad nerozhodnutelného semi-rozhodnutelného jazyka**.

### 6.5 ⭐ Redukce nerozhodnutelnosti (*doh §5.1.15–5.1.16*)

**Turingova redukce** (algoritmická): $L_1 \propto L_2$ ⇔ ∃ algoritmus $A$ s $w \in L_1 \Leftrightarrow A(w) \in L_2$.

**Důsledky:** pokud $U \propto V$:
1. $V$ rozhodnutelný ⇒ $U$ rozhodnutelný.
2. $U$ nerozhodnutelný ⇒ $V$ nerozhodnutelný.
3. $U \notin $ RS ⇒ $V \notin $ RS.

### 6.6 ⭐ Další klasické nerozhodnutelné úlohy

**Problém (ne)prázdnosti jazyka** $L_e, L_{ne}$ (úloha „je $L(M)$ prázdný?"):
- $L_e = \{ \langle M \rangle \mid L(M) = \emptyset \}$ — **není v RS** ($L_{UN} \propto L_{ne}$).
- $L_{ne} = \{ \langle M \rangle \mid L(M) \ne \emptyset \}$ — je v $\mathbf{RS} \setminus \mathbf{R}$.

**Riceova věta (*doh §5.1.19*):**
$$\boxed{\;\text{Jakákoli netriviální vlastnost rekursivně spočetných jazyků je nerozhodnutelná.}\;}$$
("Netriviální" = aspoň jeden RS jazyk ji má a aspoň jeden ne.)

Důsledky: nelze rozhodnout, zda $L(M) = \emptyset$, $L(M)$ je konečný, $L(M)$ je regulární, $L(M) = \Sigma^*$, …

**Postův korespondenční problém (PCP) (*doh §5.2.2–5.2.8*):** Dány $A = (w_1, \dots, w_k)$, $B = (x_1, \dots, x_k)$. Existuje $i_1, \dots, i_r$ s $w_{i_1} \dots w_{i_r} = x_{i_1} \dots x_{i_r}$?

**Věta:** $L_{UN} \propto$ MPCP $\propto$ PCP ⇒ PCP je **nerozhodnutelný**.

**Další nerozhodnutelné úlohy přes PCP (*doh §5.2.10–5.2.15*):**
- **Víceznačnost bezkontextových gramatik**.
- Pro 2 bezkontextové gramatiky $G_1, G_2$: $L(G_1) \cap L(G_2) = \emptyset$, $L(G_1) = L(G_2)$, $L(G_1) \subseteq L(G_2)$, $L(G_1) = \Sigma^*$.
- **Tiling problém**.

### 6.7 Role $L_{UN}$ vs SAT

| Pro | "Univerzální těžkou" úlohu hraje |
|---|---|
| Třídu **NPC** | **SAT** (Cookova věta) |
| Třídu **nerozhodnutelných** | $L_{UN}$ |

---

## Klíčové věty ke státnici (shrnutí) ⭐

| Věta | Č. | Tvrzení |
|---|---|---|
| ⭐ **Master Theorem** | 1.3.5 | Řešení rekurzí $T(n) = aT(n/b) + f(n)$ ve 3 případech. |
| ⭐ **Ekvivalence k-páskového a 1-páskového TM** | 3.1.20 | Kvadratické zpomalení $O(T^2)$. |
| ⭐ **Cookova věta** | 4.3.7 | SAT je NP-úplná; kódování běhu NTM jako CNF. |
| ⭐ **Savitchova věta** | 4.6.10 | PSPACE = NPSPACE; procedura DOSTUP. |
| ⭐ **Miller-Rabin** | 4.7.7–4.7.8 | RP test prvočíselnosti. |
| ⭐ **ZPP = RP ∩ co-RP** | 4.8.12 | Vztah Las-Vegas a Monte-Carlo. |
| ⭐ **Riceova věta** | 5.1.19 | Žádná netriviální vlastnost RS jazyků není rozhodnutelná. |
| ⭐ **Diagonální argument** | 5.1.10 | $L_d \notin $ RS. |

**Hierarchie tříd:**
$$\mathbf{P} \subseteq \mathbf{ZPP} \subseteq \mathbf{RP} \subseteq \mathbf{NP} \subseteq \mathbf{PSPACE} = \mathbf{NPSPACE} \subseteq \mathbf{EXPTIME}.$$

**Řetěz NPC redukcí (klíčový pro státnici):**
$$\text{SAT} \to \text{3-SAT} \to \begin{cases} \text{3-barevnost} \to \text{ILP},\ \text{rozklad} \to \text{SubsetSum} \to \text{batoh} \\ \text{klika} \to \text{nezávislé množiny} \to \text{vrcholové pokrytí} \to \text{ham. cyklus/kružnice} \to \text{TSP, ham. cesta} \to \text{nejdelší/nejkratší cesty v orient. grafu} \end{cases}$$

---

## Časté otázky u státnice — odkazy

Konkrétní otázky z minulých státnic ke každému tématu viz `questions.md` v této složce. Souhrnný archiv všech předmětů: `../../frequently_asked_questions/oi_wiki_committee_archive.md`.

**Nejčastěji zkoušená témata u TAL** (podle 37 zaznamenaných otázek):
1. Polynomiální redukce a Cookova věta (21 otázek) — viz §3.4–3.7.
2. Rekursivní a rekursivně spočetné jazyky (6 otázek) — §6.
3. Třídy P, NP, NPC, co-NP (5 otázek) — §3.3, §3.5, §3.9.
4. Heuristiky a aproximace (1 otázka, ale často doplňující) — §3.8.
5. Turingovy stroje (1 otázka, definice je triviálně požadována vždy) — §2.
6. Pravděpodobnostní algoritmy a Miller-Rabin (zatím 0 v archivu, ale stejně očekávaná otázka) — §5.

---

## Reference

- **Učebnice kurzu:** Demlová M., *Teorie algoritmů* (skripta, 67 stran, 2026), `materials/tal-doh.pdf`.
- **Týdenní přednášky:** `materials/tals*w.pdf` (T1–T14, semestr L 2025/26).
- **Oficiální sylabus a anotace:** [BE4M01TAL — FEL ČVUT](https://fel.cvut.cz/cz/education/bk/predmety/48/63/p4863106.html).
- **Stránka kurzu:** [math.fel.cvut.cz/en/people/demlova/tal](https://math.fel.cvut.cz/en/people/demlova/tal/).
- **Knihy doporučené v sylabu:**
  - Kozen, D. C. *The Design and Analysis of Algorithms.* Springer-Verlag, 1991.
  - Harel, D. *Algorithmics: The Spirit of Computing.* 3rd ed., Addison-Wesley, 2002.
  - Talbot, J., Welsh, D. *Complexity and Cryptography: An Introduction.* Cambridge University Press, 2006.
- **Dodatečné referenční:**
  - Sipser, M. *Introduction to the Theory of Computation.* 3rd ed., Cengage, 2012.
  - Hopcroft, J., Motwani, R., Ullman, J. *Introduction to Automata Theory, Languages, and Computation.* 3rd ed., Addison-Wesley, 2006.

