# Past Exam Questions — CVUT FEL OI Master, Data Science

Compiled from the OI-Wiki state-exam committee archive (`statnice_komise_[OI-Wiki].pdf`, Czech/Slovak), supplemented as new past papers arrive.

**How to use this file.** For each subject:
1. The **Topic frequency** table shows which sub-topics from the official syllabus come up most often — start your studying there.
2. The individual questions are preserved verbatim (Czech/Slovak) with date and examiner, plus the candidate's notes on how the discussion went and what the examiner drilled into. Pay attention to the *follow-up* questions — those reveal what depth the committee expects.

**Coverage note.** The OI-Wiki archive is heavily skewed toward older exams (2015–2023) and toward students from other specializations. Three Data Science subjects (OSW, SSU, SMU) have no entries below — they were either renamed since these exams or not represented by the wiki contributors. Those sections will be filled in as Tomas provides further past questions.

---

## BE4M33PAL — Algorithms (PAL)

**Topic frequency** (36 entries total):

| Topic | Times asked |
|---|---:|
| Graphs: representations, traversal, MST | 23 |
| Network flows / matching | 19 |
| Search trees (B / B+ / AVL / RB / Splay / 2-3-4) | 8 |
| Heaps (binary / binomial / Fibonacci) | 8 |
| Strongly connected components (Tarjan / Kosaraju) | 3 |
| Shortest paths (Dijkstra / Floyd / Bellman-Ford) | 3 |
| Text search / finite automata | 2 |

### Graphs: representations, traversal, MST

**Q1 — 21.6.2023 (Společná), examiner: Berezovskyj**

Popište nalezení maximálního toku v sítích. Jak nalezneme maximální párování v bipartitním grafu a jak spolu tyto úlohy souvisí? Uvažujte graf hranově ohodnocený i hranově neohodnocený. Pokusil jsem se definovat problém, řekl jsem, že v orientovaném grafu se startem (s) a cílem (t) máme zadané hrany s dolním/horním omezení a s tokem a snažíme se maximalizovat tok z s do t. Popsal jsem Ford Fulkersona, bottleneck, stručně zmínil problém hledání initial feasible flow, řekl jsem, že to lze řešit za pomoci ILP a chaoticky nakreslil malý příkládek. Párování v bipartitním grafu jsem popsal za pomoci příkladu tanečních (v přednáškových videích z KO), u hranového ohodnocení jsem se trochu zadrhnul (asi se to propíše do UB?), takže mě to Berezovskyj ani nenechal moc doříct a řekl, že mu to stačí. Akorát se několikrát vracel k tomu co toky vlastně jsou a chtěl to nějak přesněji definovat … nedočkal se toho a naše konverzace vypadala takhle: Berezovský: A co je teda ten tok v grafu? Já: Přemisťování komodit ze zdroje do cíle. Berezovský: Jak byste to definoval? To tam jsou nějací trpaslíci? Já: (Už mě to trochu nebavilo a fakt jsem netušil co po mně chce) Ano, to jsou trpaslíci, kteří se po cestičkách snaží dostat ze startu do cíle a ty cestičky můžou být různě široké, takže mají omezení na maximální počet trpaslíků. (Chvíle ticha) Já: Ale některé cestičky mají i omezení na minimální počet trpaslíků, například na téhle musí vždy být minimálně jeden trpaslí...

**Q2 — 26.1.2022 (Společná), examiner: Mařík**

Doslova: „Zaveďte definici orientovaného grafu. Uveďte definici a vlastnosti silně souvislé komponenty v orientovaném grafu. Jaké vlastnosti má graf vzniklý kondenzací silně souvislých komponent? Co platí o silně souvislých komponentách orientovaného acyklického grafu? Na vhodně zvoleném případě demonstrujte jeden z algoritmů konstruce silně souvislých komponent v orientovaném grafu. Uveďte princip ještě jednoho dalšího takového algoritmu.“ Odpověděl jsem na všechny otázky postupně a nikdo neměl žádný problém. Popsal jsem algoritmy Kosarajuho a Tarjana - proč fungují a jejich složitosti (2x DFS vs 1x DFS). Chtěl jsem to začít kreslit, ale Mařík řekl, že mu to stačí. Mohlo to trvat tak 5 minut. Známka A.

**Q3 — 26.1.2022 (Společná), examiner: Vyskocil**

reprezentace grafu v pocitaci + co je minimalni kostra a popsat nejakej alg na jeji nalezeni - To jsem slusne vysral s hromadou prereku, ale vzdy jsem to dovysletlil obrazkem. Kdyz jsem rekl neco spatne, Vyskocil bud zakroutil hlavou nebo se zasmal, ale jinak docela v pohode. Kdyz jsem neco nevedel, tak se doptal a kdyz nestacilo, dovysvetlil jsem obrazkem. Musim rici, ze jsem z neho mel pomerne strach, ale kdyz tusite, tak pomuze. Odvozoval jsem slozitost kruskala, a i kdyz jsem udelal chyby, tak me vzdy nechal dovysvetlit, popr. se opravit. Nejdrive chtel zadefinovat, co je graf, to jsem nejak dal a nejakej namaloval. U reprezentaci jsem rekl list sousedu, matici sousednosti, laplacian a incidence matrix (tam jsem udelal nejake chyby). U kostry definic (tu jsem zase nejak posral, tak jsem namaloval obrazek). U alg. jsem vybral Kruskala, ale nejak jsem v tom motal, ale nejak jsme se dohodli. Znamka: C

**Q4 — 21.06.2018 (Společná), examiner: Mařík**

Haldy, jejich vlastnosti, jejich reprezentace, popsat operace Insert, AccessMin, DeleteMin, Delete na binarni haldě a jejich složitosti. Popsat binomialni a fibonacciho haldu a říct, kde byccom je použili a proč. - Popsal jsem binarni, d narni haldu, reprezentaci v poli a ukazal jednotlive operace na jednoduchem příkladě. Pak se ještě Mařík doptával kde a prox se pouziva binom. nebo fib. halda místo binární - chtel slyšet, že insert je amortizovane rychlejší (u toho jsem ještě definoval amortizovanou slozitost) Hodnocení - A

**Q5 — 20. 06. 2017**

Společná - [PAL] [KO] [Berezovskyj] - Specifikujte, kdy je hledání nejkratších nebo nejdelších cest polynomiální. Existuje případ, kdy je to těžká úloha s exp. složitostí? Řešte: Úloha je najít min kostru v úplném grafu s N vrcholy, jehož váhy hran jsou floaty. Graf je buď typu A nebo B. V A leží všechny váhy hran v intervalu (100, 200). V B může váha hrany nabývat jen jedné z 21 hodnot -1.0, -0.9, … 0.9, 1.0. navrhněte a popište metodu řešení pro A a B. Jak velký graf dokáže navržený alg. zpracovat za 1 sec na normálním PC?

**Q6 — 14. 6. 2016**

Společná - [PAL] [Mařík] - Grafy, reprezentace, DFS + BFS, MST (+ algoritmy) - široké téma, popsal jsem všechny algoritmy s pár přeřeky, které mě nechali po sobě opravit. Chtěli navíc co je řez a proč algoritmy zaručují správný výsledek. U Borůvky chtěli vědět kdy nebude fungovat (všechny hrany oceněné stejně). Tahali ze mě definice, ale věděl jsem tak půlku.

**Q7 — 4. 2. 2016**

Společná - [PAL] [Hanzálek] - Efektivní algoritmy pro standardní grafové úlohy: Tarjanův a Kosajaru-Sharirův algoritmus detekce silně souvislých komponent v orientovaném grafu. Konstrukce topologického uspořádání acyklického grafu. Algoritmy hledání minimální kostry grafu (Borůvkův, Primův, Kruskalův). Téma na povídání přes celý časový rozsah státnic, proto se vyzobávali jen základní definice SSK a Tarjan + Primův algoritmus.

**Q8 — 4. 6. 2015 (Společná), examiner: Mařík**

Nejkratší cesty, definovat problém, třidy složitosti s ním spojené. Dijkstův alg. jeho varianty + složitost. Floydův alg. pro orientovaný graf s nezápornými cykly. Definoval jsem problém nejraktší cesty, zařadil jeho varianty do různých tříd složitosti. U dijkstrova alg. ze mě chvíli tahal na čem to stěžejně pracuje (nejspíš chtěl slyšet, belmanovu rovnici - nejratší cesty se skládájí z nejkratších „podcest“) + využití haldy. U floyda chtěl slyšet základní princip a složitost.

**Q9 — 2. 6. 2015 (Společná), examiner: Velebil**

Algoritmy pro hledání minimální kostry Popsal jsem obecně co je kostra, zapomněl jsem na jeden podstatný detail (musí obsahovat všechny uzly), chviličku to ze mě doloval, nechal mě na tabuli nakreslit příklad a hned mi to došlo. Dále mě nechal mluvit, popsal jsem Prima a Kruskala, ukázal na grafu na tabuli, řekl jsem že oba vycházejí ze stejného principu (výběr nejlevnější hrany z řezu). Pak jsem ještě popsal Union Find a Borůvku. Překvapivě ho nezajímali složitosti (který mi trochu vypadly, uff :)), žádné doplňující otázky.

**Q10 — 20. 1. 2015 (Společná), examiner: Mařík**

Co je to minimální kostra grafu, uveďte základní algoritmy pro nalezení minimální kostry. Co je to Union-Find problém a ke kterému algoritmu se vztahuje. PGI: [MMA] [Berka] - Částicové systémy, dynamika. Stručně popsat částicové systémy z implementačního hlediska, klasifikovat jaké síly na částice působí. Vysvětlit pojem Zero moment point. posudky A/C, obhajoba B, zkoušení B → B 5. 6. 2014 - Ing. Richard Šusta, Ph.D. předseda K13135; Doc. Ing. Jiří Novák, Ph.D. místopředseda K13138; Ing. Ladislav Šmejkal, CSc. Automa; Ing. Pavel Píša, Ph.D. K13135; Ing. Michal Štěpanovský, Ph.D. Obhajoba DP - V pohodě.

**Q11 — 5. 6. 2014**

Společná [PAL] [Kubr] Grafy, orientované a neorientované, minimální kostra. Toky v sítích. Popsal jsem základní definice grafů a jejich vlastností z vypracovaných otázek. Pak jsem popsal kostru a minimální kostru a k nim tři známé algoritmy, u jednoho chtěl popsat jak pracuje (dle vlastního výběru). U toků v sítích jsem popsal z KO slidů jaké můžeme pomocí nich řešit problémy (maximální tok, připustný tok a nejlevnější tok). Chtěl ještě vědět, zda znám nějaký algoritmus na řešení toků, ale když viděl, že si nemůžu vzpomenout, tak okamžitě řekl že mu to stačí. Celou dobu se usmíval.

**Q12 — 5. 6. 2014**

Společná [PAL] [Kubr] Grafy a jejich reprezentace. Porovnání Chtěl vědět, jak můžeme reprezentovat grafy v paměti - matice vs. spojáky. Složitosti operací, paměťové složitosti, kdy se která hodí použít.

**Q13 — 4. 6. 2014 (Společná), examiner: Mařík**

definice neorientovaneho grafu, jak se reprezentuje v pocitaci, prochazeni grafu do sirky a do hloubky; definice kostry grafu, na vhodnem priklade ukazat jak funguji algoritmy na nalezeni kostry Ptali se me z toho jen na ty kostry, ukazal jsem Kruskala, Prima a Boruvku. Zkousel me Marik, ptal se na rozumny veci, byl v pohode. → za B

**Q14 — 3. 6. 2014 (Společná), examiner: Vokřínek**

Orientované grafy, reprezentace grafů, procházení grafů, kostra, souvislost Pověděl jsem jak je možné reprezentovat grafy (různé matice), popsal DFS a BFS, algoritmy Prim, Boruvka a Kruskal a popsla jsem souvislost. Všechno v pohodě, ptal se ještě na složitost DFS. Vokříne s Demlovou bez problému

**Q15 — 3. 6. 2014 (Společná), examiner: Demlová**

Hledání nejkratší cesty. Jak se změní složitost pokud jsou topologicky vrcholy uspořádány. Co by se stalo pokud existuje polynomiální algoritmus pro hledání cesty v obecném grafu a jaký by to mělo vliv na třídy složitosti. Chtěl jsem začít s dijkstrou a hned mě zarazila a chtěla to nadefinovat. Potom se zeptala na Floyd- Warshall a chtěla vědět co se stane s maticí po třech průchodech a jak bude vypadat. S tím topologickým uspořádáním mě dostala protože jsem si myslel, že se složitost nezmění ale je lineární.

**Q16 — 25.1.2012**

Společná [PAL] Datove struktury pro reprezentaci orientovanych grafu. Minimalni kostra + popsat jeden algoritmus ktery ji nalezne. GI: Formalni popis UI. — Michal Uřičář [https://web.archive.org/web/20231130204019/mailto:uricam1@fel.cvut.cz] 2011/06/07 22:24

**Q17 — 25.1.2012 (Společná)**

2 základní algoritmy pro prohledávání grafů, popsat je + definice výrazů (strom, kostra, topologické uspořádání, úplný graf…….mraky výrazů) + sepsat k čemu je Topol. Uspoř., jak probíhá atd… SI: Máte vytvořit rezervační systém pro České Dráhy, tak aby jej cizí programátoři mohli využívat, napojovat se na něj… jaké technologie využijete, jaké máte možnosti a popište to… :) Takže WS a hotovo… Dominik Franěk Čus,

**Q18 — 25.1.2012 (Společná), examiner: Vyskočil**

datové struktury pro reprezentaci orientovaných grafů, minimální kostra, příklad algoritmu pro její nalezení a jeho popis. SI: Klíma: Problematika sdílení a ukládání dat v prostředí cloudu. Vhodnost použití jednotlivých řešení pro různé druhy aplikací. Myslím si, že jsem chytil super komisi. Avšak stejně jako psal kolega přede mnou - obhajoba práce v pohodě, ale občas mě pan Vyskočil svými otázkami zaskočil, a v jistých chvílích jsem vlastně pořádně nechápal, co ode mě vlastně chce slyšet. Stejně tak tomu bylo u ústní části jeho otázky. Pan Klíma naopak naprosto v pohodě, příjemné otázky na které šlo snadno nalést správné odpovědi. Celkově za C a naprostá spokojenost. Je čas se posunout dál :-) — Martin Průcha [https://web.archive.org/web/20231130204019/mailto:prucha.martin@gmail.com] 2011/06/09 21:52 Komise - Klima, Hanzalek, Vzskocil, Pisa, Patera

**Q19 — 25.1.2012**

Společná [PAL] Topologické uspořádání vrcholů a hran v grafu GI Speciální datové struktury pro výpočet kolizí Otázka ze společných témat mě trochu zaskočila, zkuste něco psát o topologickém uspořádání dvacet minut, přesto mě v tom Velebil dokázal nějaký čas koupat. Externistka se zase ptala u druhé otázky na věci, které se nebraly, ale to jí kolega Havran upozornil. Havran byl vůbec nejvíc v pohodě, celou dobu se usmíval. Naopak Žárovi se nic nelíbilo, kritizoval mě za to, jak prezentuji, mluvím a že jsem se vůbec odvážil nesouhlasit s oponentem, musel jsem přemýšlet nad každým slovem. Bylo před obědem mno, ještě než jsem zavřel dveře, vrhli se na stůl s jídlem. Komise: jako předchozí příspěvek, jen doplním, že ta externistka byla Kolingerová ze ZČU v Plzni (http://iason.zcu.cz/~kolinger/ [https://web.archive.org/web/20231130204019/http://iason.zcu.cz/~kolinger/]); Felkel tam taky nebyl (tipuju teda, že celý den)

**Q20 — 25.1.2012**

Společná [PAL] [překvapivě Náplava] Grafy, jejich reprezentace, procházení + Doplňující otázky z grafů. SI Webové služby. Programování, popis, vyhledávání. (Šusta) Doplňující otázky na úplný graf, počet jeho hran apod. Dál pak chtěli vidět na tabuli schéma volání webové služby. Naprosto bezproblémové. Komise: Šára, Klíma, Míkovec, Čmolík, Patera (externista)

**Q21 — 25.1.2012**

Společná [PAL] [Šára] Minimální kostra grafu, definice, existence, algoritmy. Měl doplňující otázky k definicím (+ chtěl vědět, co je multigraf), ale je naprosto v pohodě. SI REST - co definuje, porovnání se SOAP, operace v RESTu. (Čmolík) Také doplňující otázky k tématu, nic zákeřného. DP: posudky A/B, věcné otázky k DP spíš na základě posudků (hlavně Klíma), ale v pohodě. Výsledek: pro mě otázky super takže A/A, vzhledem k obhajobě diplomky A, celkem A. Skutečně nemají zájem killovat. Komise: Žára, R. Mařík, Míkovec, Vokřínek, Demlová, Tomáš Macek (IBM)

**Q22 — 6. 6. 2013**

Společná [PAL] [Macek] Grafy - pojmy (souvislost, strom, apod) + jejich reprezentace, procházení grafů, minimální kostra

**Q23 — 4. 6. 2013**

Společná [PAL] [Mařík] Neorientovaný graf, reprezentace, procházení, kostra

### Network flows / matching

**Q24 — 21.6.2023 (Společná), examiner: Berezovskyj**

Automaty, popište různé úlohy vyhledávání slov v textu, vyhledávání poškozených/poupravených slov. Slovníkový automat. Jde najednou vyhledávat více slov? Automaty jsem se moc neučil - znal jsem z PALů běžné slovníkové automaty, vyhledávání v textu, vyhledávání s max k přepsáními, vyhledávání s inserty, levhensteinovu vzdálenost.. snad u všeho jsem věděl, jak to řešit nebo jak ten automat vypadá. Nicméně když jsem to vysvětloval, tak mi Berezovskyj poměrně často skákal do řeči, zdálo se, že to chce slyšet hodně formálně (přitom když jsem na začátku řekl, že nejdřív definuju, co to je automat, tak mi řekl ať to přeskočím.. no a nakonci zkoušení ho stejně chtěl zadefinovat). Kreslil jsem tam ty automaty na tabuli, což v tom spěchu byl asi trochu škrabopis, který Berezovskyj z té dálky asi pořádně nepřečetl. Celkově toto zkoušení nebylo moc příjemné. Na to co jsem všechno řekl, bylo zde dle mého názoru hodnocení poměrně přísné.

**Q25 — 20. 1. 2015 (Společná), examiner: Čmolík**

Haldy, co to je, jaký jsou typy, popsat všechny možný operace a jejich složitosti. Stihnul jsem popsat komplet binární haldu a její operace a strukturu binomiální haldy a její operaci merge, pak se mě ptal jak bych reprezentoval binární haldu v programu a nelíbilo se mu, že uzly chci ukládat jako objekty s ukazatelema na rodiče a potomky :) Chtěl to jako pole kde se místo ukazatelů vypočítá index rodiče nebo potomka. To ze mě nakonec vymámil a ještě se zeptal jak a na kterých operacích se liší složitosti všech tří hald. Navara chtěl upřesnit složitost merge u binární hlady, kde jsem řekl že je to n*log(n) vkládáním prvků jedné haldy do druhé - chtěl rozlišit počet prvků jedné a druhé haldy. K popisu Fibonacciho haldy jsem se vůbec nedostal. SI: [OSP] [Šůcha] - Nástroje pro správu a sdílení kódu, popsat co jsou zač a jak se liší operace merge, rebase a cherry-pick v Gitu. Popsal jsem poměrně stručně CVS, SVN a Git, pak mě popohnal k popisu těch operací kde jsem věděl s jistotou akorát co je merge, o rebase jsem chvíli polemizoval že to je asi nastavení nějakýho commitu jako prvního a všechny před nim se zapomenou, tak chtěl vědět jak je to pokud mám víc větví, to už jsem tipoval, takže mě popohnal k cherry-pick operaci, kde jsem zamumlal že tam vim ještě míň než u rebase a Vokřínek se začal smát :) Řekl jsem co si myslim, že to je (nemyslim, že jsem se trefil), načež Šůcha prohlásil že mu to stačí. Celkově všichni v pohodě, příjemní, nechali mě mluvit...

### Search trees (B / B+ / AVL / RB / Splay / 2-3-4)

**Q26 — 14.6.2022 (Společná), examiner: Mařík**

B-strom definujte, na vhodném příkladě demonstrujte operaci find a insert při vícefázové strategii. Co je to B+ strom, jak se od B-stromu liší. Jaké další typy vyhledávacích stromů znáte?

**Q27 — 21. 6. 2017**

Společná - [PAL] [Lisý] - Definujte B, B+, R-B, 2-3-4, Splay stromy. Popište mechanizmus vkládání do B stromu. Stačily relativně obecné definice a vlastnosti ke všem stromům (neřešili jsme žádné rotace a vyvažování nějak apod. podrobně). Nakonec ukázal v přidání položky do B stromu pro obě dvě stategie a složitost této operace. U B a B+ stromů jsem řekl pár přeřeků, které jsem buď sám opravil nebo s hinty pana Lisého dal správně dohromady. U 2-3-4 a R-B ho zajímala jaké je spojitost mezi těmito stromy. Pan Lisý v pohodě, co jsem nevěděl tak se v tom dál nešťoural.

**Q28 — 7. 2. 2017**

Společná - [PAL] [Sloup] B a B+ stromy, jejich pouziti a slozitosti operaci

**Q29 — 2. 6. 2015 (Společná), examiner: Píša**

Porovnání výšek AVL a BR stromu. Nakonec chtěl Píša obecně vědět, co jsou binární stromy, jaké jsou druhy a pro jaké datové manipulace se který hodí. Poslední otázka byla, jakou datovou strukturu bych použil při čtení dat z hard disku.

**Q30 — 25.1.2012 (Společná)**

Vyhledavani v textu z pohledu konecnych automatu. Nektere zakladni metriky priblizneho vyhledavani v textu. Co chteli slyset: Definici kon.automatu, vztah KA-regularni jazyk (ze ho rozpoznava), co je to regularni jazyk, levensteinova a hammingova vzdalenost (definice) SI: Objektově orientovaný návrh software, metodiky objektového návrhu, postupy rozvinutí technické specifikace na úrovni modulů do detailního objektového návrhu, návrhové vzory. Co chteli slyset: ja tam popsal co je napsano v te otazce tu na wiki. Me dojmy: Cenu za nejvetsiho rejpala ziskava pan Vyskocil, ktery nepusobil moc prijemne a spis se snazil potopit nez pomoct. — Kobe [https://web.archive.org/web/20231130204019/mailto:kobetvla@fel.cvut.cz] 2011/06/08 00:24

### Heaps (binary / binomial / Fibonacci)

**Q31 — 14. 6. 2016**

Společná - [PAL] [Marik] - Haldy - binarni, binomialni, Fibonacci jen zaklad. Ptal se na uplne zakladni veci, uplne bez problemu.

**Q32 — 4. 6. 2015 (Společná), examiner: Mařík**

Definice haldy. Popsání operací INSERT, ACCESS a DELETE na binární haldě a jejich složitost. Kdy bychom použili binomiální haldu a kdy Fibonacciho? Tohle šlo celkem samo. Ještě se doptal, jak bych spojil 2 binární haldy a jakou to má složitost. U Fibonacciho haldy chtěl slyšet, že problematické je mazání, kdy může konsolidace trvat dlouho, což může být problém u systémů reálného času. Celkově - Trochu jsem nebyl připraven na to, že se budou celou dobu ptát, což je např. u model checkingu trochu náročnější než jen převyprávět, co má člověk na papíře. Všichni se snažili ale spíš pomoct než potopit.

**Q33 — 25.1.2012 (Společná)**

Binarni a binomialni haldy, porovnat slozitosti. Amortizovana slozitost. GI: Testovani s a bez uzivateli. Kdy je vhodne pouzit co. — Petra Kalinova [https://web.archive.org/web/20231130204019/mailto:kalinpe1@fel.cvut.cz] 2011/06/08 01:21

**Q34 — 25.1.2012 (Společná)**

Binarni a fibonacciho halda - popsat. Srovnani z hlediska slozitosti operaci. Amortizovana slozitost (Vyskocil) Ptal se nasmysluplny veci, nejaky operace jak fungujou (delete, insert, konsolidace). Co je amortizovana slozitost, jakej je rozsil mezi prumernou a amortizovanou. Neprisel by mi, ze by nejak snazil potopit. SI: Jake webove aplikace jsou vhodne k nasazeni do cloudu a ktere ne ? Uctovani sluzeb v cloudu - priklad. (Klima) Klima byl v pohode, ptal se jaky aplikace bych dal do cloudu. Jaky bych tam nedal. To je vse. Celkova znamka C Meli nejaky dotazy k diplomce, ale nic hroznyho - posudky B a A, celkove C. — Petr Pokorny [https://web.archive.org/web/20231130204019/mailto:pokpetr@gmail.com] 2011/06/08 16:26

**Q35 — 25.1.2012 (Společná), examiner: Vyskočil**

Binární a Binomiální halda, porovnání časových složitostí jejich operací. Vysvětlit pojem amortizovaná složitost SI: Píša: Vysvětlit pojmy Spolehlivost, Dostupnost, MTBF, MTTF, Robustnost systému, Redundance, Model Publish Subscribe, možná další už nevím Obhajoba DP v pohodě. Co se týče zkoušky: Vyskočil mě svými otázkami trochu zaskočil, ale ptal se na vlastně jen na to, co jsem měl špatně (tudíž jsem i nevěděl :), pan Píša byl vpoho a nechal brouka žít :) — Tomáš Zajíček [https://web.archive.org/web/20231130204019/mailto:zajictom@fel.cvut.cz]

### Strongly connected components (Tarjan / Kosaraju)

**Q36 — 4. 6. 2015**

Společná - [PAL] [Mařík] - Souvislost, (silně) souvislé komponenty, Kosaraju-Sharir a Tarjan algoritmy. Definoval jsem základní pojmy a popsal a ukázal Tarjana. Na konci (málo času) ještě v jedné větě popsal Kosaraju-Sharir. Pan Mařík měl pár doplňujících dotazů (složitost, srovnání,…), jeden dotaz taky pan Velebil, všechno ok, prakticky bez zádrhelů, nálada po obhajobě bych řekl pookřála =).

---

## BE4M01TAL — Theory of Algorithms (TAL)

**Topic frequency** (37 entries total):

| Topic | Times asked |
|---|---:|
| Reductions, Cook theorem | 22 |
| Complexity classes P / NP / NPC / co-NP | 16 |
| Turing machines (DTM / NTM) | 10 |
| Recursive / r.e. languages, decidability | 9 |
| Randomized algorithms, RP/ZPP/co-RP | 4 |
| Other | 4 |
| Space complexity PSPACE / Savitch | 1 |

### Reductions, Cook theorem

**Q1 — 22.6.2023 (Společná), examiner: Žukovec**

Redukce a polynomiální redukce úloh / jazyků. Cookova věta. Naznačte redukci SAT na 3-CNF SAT. Oba problémy zformulujte. Popsal jsem všechny definice a na konkrétní klauzuli ukázal redukci SAT na 3-CNF SAT, tam chtěla paní Žukovec dovysvětlit, jaktože je ta redukce polynomiální.

**Q2 — 22.6.2023 (Společná), examiner: Blaško**

Popište DTM, NTM a popište k čemu se využívají. Uveďtě příklady alespoň tří dalších modelů Turingova stroje a diskutujte jejich ekvivalenci ve srovnání s DTM/NTM. Porovnejte časovou a paměťovou náročnost těchto modelů. U této otázky jsem byl zaskočen tím, že TAL zkouší Blaško namísto Žukovec, ale asi jsem na tom jen vydělal. Popsal jsem polovinu tabule jen definicemi. Zadefinoval jsem DTM a NTM. Dále jsem uvedl příklady dalších TM modelů - TM s „k“ páskami, Randomizovaný TM, Univerzální TM. Pro každý jsem se pokusil rozepsat jak fungují a k čemu se využívají. U rozepisování fungování univerzálního TM mě Blaško zastavil že mu to stačí.

**Q3 — 26.1.2022 (Společná), examiner: Žukovec**

Redukce a polynomialni redukce uloh/jazyku, Cookova veta. Naznacte redukci SAT na 3-CNF SAT. Oba problemy zformulujte.

**Q4 — 26.1.2022 (Společná), examiner: Mgr. Adam Rogalewicz, Ph.D.**

Jazyky rekurzivní a rekurzivní spočetné. Popsat co to je (ve vztahu k Turingově stroji), uvést příklady úloh a to včetně úlohy co je mimo R i RS. Jak se zařadí R a RS v Chomského typech jazyků. Vztah R a RS, co když úloha i doplněk RS. Další věci, které si již nevybavuji. Zkoušející mi přišel velmi příjemný, nesnažil se mě v ničem vykoupat i když jsem se na pár věcech trochu zastavil či zamotal a něco jsem i řekl trochu špatně, ale většinou jsem to uvedl na pravou míru. Známka: B

**Q5 — 12. 06. 2018 (Společná), examiner: Tišer**

Co je Cookova věta, důkaz že polynomiální redukce úloh je tranzitivní Stručně jsem popsal Cookovu větu, vysvětlil její důsledek a definoval NPC. Důkaz tranzitivity: máme li úlohy u, v, w tak že se u polynomiálně redukuje na v se složitostí p(n) (polynom) a v se polynommiálně redukuje na w se složitostí q(n) (taky polynom) pak redukce z u na w přes v má složitost q(p(n)), což je také polynom. Složitost je tedy polynomiální, tudíž se v polynomiálně redukuje na w.

**Q6 — 05. 09. 2017 (Společná), examiner: Demlová**

Popsat TSP, Knapsack a toky. U každého uvézt do jaké třidy patří. Popsat rozdíl mezi aproximačním a heuristickým algoritmem, uvést od nich příklad. Rozhodnout, zda se tok může poly. redukovat na Knapsack. Popsal jsem dané úlohy. U toků se paní Demlová doptávala (řekněme že moje definice nebyla úplně nejpřesnější), já jsem se u toho trošku zasekával, ale postupně jsme se dobrali k řešení. Víceméně jsem u toků vysvětloval, co to vůbec je tok, kirhoffův zákon apod. Zde jsem špatně uvedl, že toky jsou NP- Hard. Následně jsem popsal aprox. a heuristický algoritmus, uvedl příklad pro TSP a byl konec (ještě ze mě dostala, že to je pro metrický TSP a pro obecný nemusí platit). Celkově zkoušení s paní Demlovou bylo příjemné, oceňuji její reaktivní přístup - tzn. taková

**Q7 — 21. 6. 2017 (Společná), examiner: Tišer**

Turingovy stroje, rekurzivní a rekurzivně spočetné jazyky, algoritmicky neřešitelné úlohy. Definujte Turingův stroj, dokažte, že polynomiální redukce je tranzitivní relace Definici TS úplně přeskočil. Chtěl def poly redukce, pak definice tříd R a RS (na vztahy mezi nimi se neptal). Dál se ptal co to vlastně znamená, že ta redukce je polynomiální (na tom jsem dost drhnul - tam jsem nejdřív zkoušel formálně definovat asymptotickou složitost, ale co nakonec chtěl slyšet, a k čemu mě rychle dotlačil, bylo, že počet kroků toho algoritmu je nějaká funkce p(n), kde p je polynom a n vstup. Ohledně důkazu poly redukce jsem řekl něco ve smyslu „Předpokládejme, že máme úlohy u, v a w takové, že u se poly. redukuje na v, a zároveň se v poly. redukuje na w. A zároveň předpokládejme, že u se neredukuje na w. Nicméně existují algoritmy A a A' tak, že A redukuje u na v a A' v na w, čili můžeme aplikací těchto algoritmů na problém X této redukce dosáhnout: A'(A(X)). Nyní je třeba ještě dokázat, že tato redukce je polynomiální: tam jsem drhnul ještě víc, ale nakonec mě k tomu dokopal. Fakt hodně pomáhal. Pokud A' pracuje v polynomu p(n) a A v polynomu g(n), pak výsledný polynom bude p(g(n)). Tím zkouška skončila.

**Q8 — 15. 6. 2016**

Společná - [TAL] [Bošanský] - Rekurzivní a rekurzivně spočitatelné jazyky, příklady jazyků a vztahy mezi nimi. Popsat randomizovaný TM a jejich třídy. Naprosto v pohodě učitel.

**Q9 — 2. 6. 2015 (Společná), examiner: Hekrdla**

Definice polynomiální redukce problémů a jestli relace převoditelnosti je uspořádání – To bylo na papíře, ale při zkoušení se mě p. Hekrdla zeptal jen na to uspořádání. Začal jsem nejdřív definicí převoditelnosti, což pan Hekrdla možná nepochopil, nebo jsme si nějak nerozuměli, a začal mi něco vysvětlovat, což bylo podle mě trochu off topic, tak jsem chvíli nechápavě poslouchal, a pak jsem řekl, že to radši napíšu na tabuli, aby bylo jasno. Ukázal jsem reflexivitu a tranzitivitu. U antisymetrie jsem trochu spletl definici, takže jsem byl opraven že chci dokázat (a =< b & b =< a) → a = b. K tomu jsem řekl, že to neplatí, protože to, že 2 úlohy jsou na sebe navzájem převoditelné, neznamená, že se rovnají. Tudíž to není uspořádání. Pak se mě ptal, jak bych udělal, aby upořádání platilo, tak jsem nějak s dopomocí vymyslel, že by to platilo pro relaci ne na úlohách, ale na třídách úloh, které jsou stejně těžké (tj. a=<b a b=<a). Nakonec se mě zeptal, co by v takovém uspořádání byla množina NPC, já mu řekl, že maximum, a on byl spokojen. posudky A/A, obhajoba A, zkoušení A → celkem A 2. 6. 2015 SI - Doc. Ing. Jiří Vokřínek, Ph.D. -13141 - předseda, Ing. Adam Sporka, Ph.D. – 13139 – místopředseda, Ing. Zdeněk Buk, Ph.D. – 13142, Ing. Josef Hekrdla, CSc. – 13101, Doc. RNDr. Petr Šaloun, Ph.D. – VŠB Ostrava Obhajoba DP - Nástroj pro generování dokumentace vzdálených rozhraní (API) webových služeb aplikací Java Enterprise Edition - byli spokojeni, tema se jim ...

**Q10 — 5. 6. 2014 (Společná), examiner: Hekrdla**

Co je to redukce a co je to polynomiální redukce. Je relace redukce tranzitivní, reflexivní, uspořádání? Řek sem nějakej základ, nevěděl sem co je uspořádání, ale ani to nějak neřešil… Pak se doptal na nějaký věci ohledně NPC úloh. Zkoušení bylo hodně v pohodě, dostal sem z tý otázky A, i když sem žádnej zázrak nepředved.

**Q11 — 3.6. 2014**

Společná [TAL] [Hekrdla] Definujte co znamena, ze jedna rozhodovaci uloha se redukuji (polynomialne) na druhou. Je tto relace tranzitivni, reflexivni, usporadani? (výsl. A) Vsechno v pohode, pan Hekrdla se porad usmival a pomahal, ikdyz jsem zapomela definici usporadani (tranztivni, reflexivni, antisymetricka) dal mi A. Podle me hodne zalezi na dojmu od prezentace.

**Q12 — 5.6. 2013**

Společná [TAL] [Demlová] Demlová: Nejkratší cesty, popis problému, algoritmy, popsat algoritmy, jak by ovlivnil třídy složitosti fakt, že existuje poly. algoritmus, který řeší hledání vest v topolologicky označkovaném grafu. (výsl. C) Demlová byla, je a bude u statnic strašně hodná. Hodně pomáhala, když jsem řekl, nějakou (mnohdy zásadní) blbost, tak mě hned opravovala, ale dodávala, že v podstatě mám pravdu, nebo se aspoň usmívala. Rohodně strašně fajn jí v komisi mít, protože hodně zvedá náladu a nerejpe do diplomky.

### Complexity classes P / NP / NPC / co-NP

**Q13 — 20.6.2023 (Spoločná), examiner: Demlová**

Vysvetlite pojem nerozhodnuteľného jazyka/algoritmicky neriešiteľnej úlohy. Uveďte príklad takého jazyka/úlohy. Aký je vzťah medzi týmto pojmom a triedou rekurzívnych jazykov a triedou rekurzívne spočetných jazykov? Uvažujte následujúci problém: Rozhodnite, či Turingov stroj prijíma binárne slovo 00. Do ktorej triedy patrí jazyk tejto úlohy? Odpoveď poctivo zdôvodnite. Pojmy som definoval, potom som jej ukázal príklady nerozhodnuteľných jazykov/úloh. Naznačil som Poštov korešpondenčný problém a definoval diagonálny jazyk. Otázku s TM, ktorý prijíma binárne slovo 00 som moc nepochopil, povedal som, že patrí do triedy P, ale patrí do RS. Ešte sa spýtala kam patrí jazyk, ak jeho doplnok je rekurzívne spočetný - do triedy rekurzívnych jazykov. Od tej úlohy s binárnym slovom 00 som zostal nejaký domotaný, nakoniec mi dala „lepšie“ D. Bola celkom milá.

**Q14 — 26.1.2022 (Společná), examiner: doc. RNDr. Natalie Žukovec, Ph.D.**

Definujte třídy P, NP, NPC a co-NP. Do kterých tříd patří 4-barevnost grafu? Problém zformulujte a vše zdůvodněte. Chtěl jsem začít formulací Turingova Stroje, to mě zastavila a říkala ať přeskočím. Tak jsem definoval P a NP pomocí TM. Poté jsem zadefinoval redukci a polynomiální redukci. Díky ní jsem definoval třídu NPC. Poté jsem laicky zadefinoval co-NP. Nakonec jsem vysvětlil co je 4-barevnost grafu, ale pak jsem se začal dost dopit, když se mě zeptala jak bych to vyřešil, co je to nedeterministický algoritmus atd… Jediné co bylo třeba odpovědět je, že se to dá ověřit pomocí nedeterministického alg., kvůli tomuto jsem málem zkoušku neudělal. Nicméně cítil jsem z ní, že mě nechce potopit, jen prostě cítila, že tam mám mezeru a čím víc mě chtěla navíst na odpověď tím víc jsem cítil jak se odhaluje moje nepochopení, a stres mi k tomu taky moc nedopomohl. Nakonec to odpověděl někdo jiný z komise, a tím otázka zkončila. Známka: E Odborná [BIN] [doc. Ing. Jiří Kléma, PhD.] - Zarovnání biologických sekvencí: jak a proč jej provádíme, popište alespoň jedno exaktní a jedno heuristické řešení zarovnání dvou sekvencí, k jakým změnám dojde pokud budeme zarovnávat více sekvencí současně. Po průseru z první otázky bylo jasné, že se pohybuju dost na tenkém ledě. Bylo cítit, že pokud doblbnu i tuto otázku, náhradní termín mě nemine. Myslím, že si toho byl vědom i pan docent Kléma. Poté, co jsem nastínil nějaký úvod o tom proč potřebuje...

**Q15 — 21.06.2018 (Společná), examiner: Žukovec**

P, NP, NP-úplné a NP-těžké úlohy. Do jaké třídy složitosti patří hledání nejkratších cest v grafu. Jmenujte úlohu, která nepatří do NP. - Zadefinoval jsem všechny čtyři třídy plus polynomiální redukci, kterou jsem použil v definici NP-úplných a NP-těžkých úloh. Řekl jsem, že hledání nejkratší cesty v grafu patří do třídy P, pokud graf neobsahuje záporné cykly, v opačném případě patří do třídy NP-úplných problémů. Pak jsem zmínil Postův korespondenční problém jako příklad úlohy která nepatří do NP. Hned ze začátku se akorát doptala, jaké úlohy v definici mám na mysli, což jsem nepochopil, pak to upřesnila a chtěla slyšet, že jsem to definoval pro rozhodovací problémy a že pro vyhodnocovací i optimalizační problémy to platí stejně, protože se dají mezi sebou převádět. Více mi do toho neskákala, celou dobu se tvářila spokojeně a po každé definici kývla, že souhlasí. Hodnocení - A

**Q16 — 20.6.2018 (Společná), examiner: Zukovec**

Definovat NP a NPC, polynomialni redukce, Cookova veta a jeji zneni. Bezny postup, ked im nebolo nieco jasne tak sa doptali a velmi docenili, ked to clovek rovno nakreslil na tabulu.

**Q17 — 21. 1. 2015 (Společná), examiner: Demlová**

NPC, NP hard, Cookova věta a heuristické algoritmy pro úlohy v NP. → S pár drobnýma zaškobrtnutíma jsem popsal vše + odpověděl na její otázky → B UI: [BIA] [Kubalík] - Neuronové sítě, Popsat perceptronový neuron a pak jsem si mohl vybrat jestli chci popsat RBF nebo SOM sítě, vybral jsem si som → Trochu jsem se v tom motal a nepamatoval jsem si úplně přesně vzorečky, popis SOM už šel líp (jinak Kubalík byl strašně hodnej a radil, kde se dalo) → C posudky B/B, obhajoba B, zkoušení B,C → C, celkem C 20. 1. 2015 - Vokřínek (P), Navara (MP), Šůcha, Čmolík, Macek(IBM ČR) Obhajoba DP - OK, kromě Navary (kterýho chápu, že implementační práce moc nezajímala :) ) dávali docela pozor, zodpověděl jsem otázky z posudků a Šůcha a Macek se pak ještě ptali ze zájmu.

**Q18 — 5. 6. 2014**

Společná [TAL] [Šusta] NPC a NP hard, pak cokova veta a priklady nad nimy NPC a NP hard, pak cokova veta a priklady nad nimy chtel odeme pak tuninguv stroj, kdys jsem chtel rict definicy tak me zarazil a zacal ode me chtit neco jine , popravde jsem nepochopil co odeme chce

**Q19 — 5. 6. 2014 (Společná), examiner: Velebil**

NP úplné úlohy a redukce úloh. PGI: [GVG] [Sýkora] - Matematický model perspektivní kamery a homografie. posudky A/B, obhajoba A, zkoušení A → A 5. 6. 2014 - Doc. Ing. David Šišlák, Ph.D. - K13136 - předseda, Ing. Jiří Vokřínek, Ph.D. - K13136 - místopředseda, Ing. Zdeněk Buk, Ph.D. - K13136, Ing. Jan Kubr - K13136, Ing. Josef Hekrdla, CSc. - K1310, Externí člen: Dr.-Ing. Tomáš Macek – IBM ČR, Tajemník: Jan Hrnčíř, MSc. Obhajoba DP - Na pohodu, pár dotazů ze zájmu

**Q20 — 5. 6. 2014**

Společná [TAL] [Hekrdla] Pravděpodobnostní algoritmy, randomizovaný turingův stroj, třída úloh TP. Navrhněte pravděpodobnostní algoritmus pro výpočet čísla Pí. Ke všemu jsem něco málo řekl, algoritmus jsem nenavrhl, tak se zeptal jestli znám nějaké jiné (testy prvočíselnosti), dál se neptal.

**Q21 — 5. 6. 2014 (Oborová), examiner: Hekrdla**

Ulohy NP-uplne, NP-tezke. Co muzeme rict o uloze, o ktere vime, ze se na ni redukuje nektera NP-uplna uloha? (Hekrdla) Popsal jsem papir z obou stran, nakonec jsem z toho pouzil jen mensi cast. Rekl jsem definici NPC, vysvetlil polynomialni redukci a pak se me ptal na vztahy mezi tridami, napr. „Co by se stalo, kdybychom nasli DTM, ktery resi NP-tezkou ulohu?“. Celou dobu se usmival, nedelal problemy. diplomka B, otazky B, celkove B Tak TPJ sice ceka, ale nebojte se ho. :-) 5. 6. 2014 - Doc. Ing. David Šišlák, Ph.D. - K13136 - předseda, Ing. Jiří Vokřínek, Ph.D. - K13136 - místopředseda, Ing. Zdeněk Buk, Ph.D. - K13136, Ing. Jan Kubr - K13136, Ing. Josef Hekrdla, CSc. - K1310, Externí člen: Dr.-Ing. Tomáš Macek – IBM ČR, Tajemník: Jan Hrnčíř, MSc. Obhajoba DP - Všichni totálně znuděni, nedávali pozor, a pak nevěděli na co se zeptat :-)

**Q22 — 4. 6. 2014**

Společná [TAL] [Rogalewicz] Třídy složitosti (nevím co přesně bylo v zadání). Ptal se externí člen, pan Rogalewicz, působil na mě, že není moc schopný vyjádřit na co se vlastně chce zeptat. Nakonec ho samotné třídy složitosti moc nezajímali a ptal se spíš na P, NP. Co by se stalo, kdyby byl polynom. alg, který řeší NP atd. Pak se ptal na PSpace a NSpace, vztahy mezi nimi, zde si odpovídají. Je pozor, pro třídy P a NP využíval DTIME(f(n)) a NTIME(f(n)), Demlová po chvilce řekl, že ona v přednáškách používala P a NP, po chvilce si zřejmě vyjasnili rozdíl a Demlová požádala studenta, ať dál používá DTIME a NTIME. Celkově byl takový rýpavý.

**Q23 — 4. 6. 2014 (Společná), examiner: Tišer**

Třídy složitosti P a NP. Polynomiální redukce. Redukce SAT na 3CNF SAT. Ptal se Tišer, přerušil můj popis postupu redukce a zadal mi převést jednoduchou instanci (a & b & c & d).

**Q24 — 3. 6. 2014 (Společná), examiner: Demlová**

Třída P a NP; Příklad dvou úloh ze třídy P, dvou ze třídy NP a dvou úloh, které nepatří do NP (Uveď všechny použité předpoklady). Kam patří následující úloha: je dán ohodnocený orientovaný graf G, jeho dva vrcholy u, v a číslo k. Existuje nejkratší cesta, alespoň k?

**Q25 — 25.1.2012**

Společná [TAL] [Velebil] definovat P a NP problemy a ke kazdemu uvest priklady GI: Barevny modely a komprese obrazku (Matas) * Otazky krasny, kdyz jsem je rozbalil, rikal jsem si jak to je v klidu. Nakonec to tak v klidu nebylo. K oborovy otazce GI jsem napsal asi 3 stranky, ale Matas jakoby to ignoroval a pri ustni se doptaval na nektery definice jako co je barva, gamut, jak PRESNE funguje DCT u JPEG komprese atd.. Obecne vzato at jsem mu rekl cokoliv, vzdycky se mu to nezdalo, vzdycky chtel neco vic, byt priznavam ze u otazky barva jsem se solidne zamotal.. Po prvni minute zkouseni u nej bych se sel nejraci zahrabat, docela me rozsekal. U otazky od Velebila uz to bylo lepsi a pomalu jsme se spolu dobirali k tomu, co chtel slyset. Nakonec mi dali z ustni E a z diplomky A (posudky A/A) a s prihlednutim k pomerne prumernym studijnim vysledkum jsem dostal vyslednou taktez prumernou - C. Mel jsem dojem, ze pisemna priprava otazek by mela slouzit jako zaklad hodnoceni ustni zkousky, tohle mi spis prislo jako „tak mi sem napiste vsechno co vite a ja vas pak zhodnotim podle toho, na co se budu ptat kolem“. Jinak receno Matase nezajimaji fakticke(encyklopedicke) znalosti, jako spis presne definice cehokoliv na co se zepta. Vzhledem k tomu, ze jsem mel pisemnou pripravu podle me docela dobrou, tak to E me docela nemile prekvapilo. Nu coz, na to se historie nepta.. Hodne stesti vsem v lete. Jinak zbyli dva „komisari“ : Sykora prakticky celou dobu mlcel, jen mel nejake konstruktivni do...

**Q26 — 25.1.2012 (Společná)**

NP-úplné a NP-obtížné úlohy (definice, Cookova věta, polynomiální redukce) Zkoušel Hušek. Naprosto v pohodě, řekl že tam mám vše (hodně se mu líbil důkaz Cookovy věty) - prakticky jsem psal jen na to, na co se ptá otázka, nic navíc. Aby se neřeklo, dal pár doplňujících otázek na to, co je to vlastně vůbec to NP a jaké jsou vztahy P, NP, NPC a NP těžké. SI: WS + vlastnoti. Databáze + persistenntí frameworky. Zkoušel Klíma, takže opět pohoda. K WS jsem napsal minimum, co to je, jaké to má vlastnosti a nějaké termíny (soap, wsdl, rest, uddi), kde jsem ke každému napsal tak jednu větu. U DB jsem napsal, že máme relační a key-value, pak že máme nějaké ORM a přešel jsem na JPA. Doplňující otázky: jestli našlo uplatnění uddi v praxi, kdy se vyplatí používat ORM a kdy naopak ne. Obhajoba DP v pohodě, otázky měl jen Vyskočil (relativně k věci, ale je pravda, že některé moc přijemné nebyly. Nicméně pokud své DP opravdu rozumíte, neměli byste mít problém). — Roman Vaclavik [https://web.archive.org/web/20231130204019/mailto:vaclarom@fel.cvut.cz] 2011/06/08 17:49

**Q27 — 25.1.2012 (Společná)**

Slozitost ulohy, algoritmu, P, NP, Cookova veta. Ptal se me Vyskočil, docela dost mi do toho rejpal. Napred se me pokusil pribit na formalni definici slozitosti, coz se mu nepovedlo, u Cookovy vety videl dukaz, tak se na ni ani moc neptal. Nakonec jsem se s nim dlouze dohadoval o tom, jestli polynomialni redukce musi probehnout v poly case. Nakonec jsem rekl, ze ano na NTM, on mi rekl, ze ne. V materialech mam, ze musi byt polynomialne slozita (coz na NTM v pripade P == NP je…(a proto se to taky dela))… Takze nevim, jestli narazel na NTM nebo na to, ze redukce nemusi byt v NP…to jsem fakt nepobral…kazdopadne me na tom slusne ukrizoval… SI: Bezpecnost v cloudu, porovnani s on-premise a local service provider. Private Cloud. Dotaz na SLA jakozto doplnujici otazka. Naprosto v pohode. Klima me skutecne hodne podrzel. U diplomky do me dost ryl Vyskocil. Nektery pripominky byly dobry. Jiny mi prisly divny (kdyz se me ptal, proc jsem nejaky architektonicky rozhodnuti neudelal jinak a implikoval, ze mam jiny pozadavky, nez jsem mel). A jiny mi prisly uplne ujety - proc nemam uzivatelskou dokumentaci (moje DP je javovska knihovna, proste jeden blbej jar, kterej se nakopiruje mezi knihovny…). Z hlediska prezentace mi to asi moc nevyslo…malo casu a asi jsem to spatne naformuloval, takze ty otazky pak podle toho byly divny… No vysledek: komise se podivala na muj prumer a patrne prehlasovala Vyskočila, aby mi nekazila cervenej diplom… Z cely obhajoby a statnic mam dost div...

**Q28 — 25.1.2012**

Společná [TAL] Cookeova věta, co je NP-těžký problém (plus příklad NP-těžkého, co není NP-úplný). SI: Webové služby. Co to je, jaké se používají technologie, … Komise byla veselá, všecko v pohodě. Diplomka A/A, celkem jsem dostal B. (leden 2013) 23.1.2013, Komise: Havran (předseda), Bittner (místopředseda), Čmolík, Šůcha, Kohout (ZČU Plzeň)

### Turing machines (DTM / NTM)

**Q29 — 25.1.2012**

Společná [TAL] Pojem algoritmus (Turingův stroj a jeho varianty) GI Částicové systémy (princip, uplatňující se síly, aplikace, zobrazování) Společnou otázku jsem si myslel, jak je v cajku, ale Velebil to po chvilce stočil k algoritmicky neřešitelným problémům a tam jsem si krapet zaplaval. Taky mi vytknul, že jsem neměl TS úplně dobře formálně zadefinovanej (popsal jsem to slovně + nějaký hokus pokusy o tu šestici, ale měl jsem tam asi nějaký chybky). Otázka z grafiky mě nepříjemně překvapila. Particly se braly v MMA, Berka v komisi nebyl a podle toho, co jsem si hledal o té Kolingerové, tak její obor byla spíš výpočetní geometrie (tj. VGA) a vyhledávání (DPG). Takže jsem se MMA učil jen tak zrychleně pro svůj klid, abych si pak nenadával. No, abych to zkrátil… sepsal jsem tam takový ty základní věci a pak ze mně dolovala ještě nějaký další pojmy, i pár (středoškolských) fyzikálních vzorečků jsem jí řekl :) Ale byla hodná a moc nerejpala (oproti tomu, co jsem si přečetl třeba na Exfortu). Hodní byli vlastně všichni (šel jsem druhý z rána), Žára byl milej (na rozdíl od příspěvku výše) a Havran taky neryl, takže nakonec jsem odešel ještě docela úspěšně. Komise: Slavík, Hušek, Mařík R. (TVS), Náplava (PIS), Hudec (externista)

### Recursive / r.e. languages, decidability

**Q30 — 26.1.2022 (Společná), examiner: Demlová**

Vysvetlete vztah mezi rozhodovaci ulohou a jazykem. Definujte, co jsou to nerozhodnutelne problemy/jazyky a jejich vztah ke tride R a RE. Definujte pojem jazyk L1 se redukuje na jazyk L2.

**Q31 — 26.1.2022 (Společná), examiner: Žukovec**

Definujte rekurzivni a rekurzivne spocetne jazyky. Do ktere tridy patri jazyk Postova korespondencniho problemu? Problem zformulujte. Vysvetlete co to je jazyk problemu.

**Q32 — 14. 06. 2018 (Společná), examiner: Žukovec**

Definujte rekurzivní a rekurzivně spočetné jazyky. Uveďte příklady. Platí tvrzení, že pokud je jazyk rozhodován nedeterministickým turingovým strojem, tak je rekurzivní? Nejdříve jsem zadefinoval TM, potom rekurzivní a pak rekurzivně spočetné jazyky. U rekurzivně spočetný jsem řekl, že jsou algoritmicky neřešitelné a jako příklad uvedl Halting problem (problém zastavení TS). U tvrzení jsem řekl, že platí, protože to vychází přímo z definice rekurzivního jazyka, kde není specifikován typ TS. Doptala se na rozdíl mezi DTM a NTM a pár dalších otázek. Byla spokojená, za A.

**Q33 — 23.1.2013**

Společná [TAL] [Čmolík] Co je asymptotická a amortizovaná složitost, popište, vysvětlete na příkladu s dynam. polem, které se při naplnění natáhne na dvojnásobek. GI: kd-stromy pro vyhledávání nejbližších sousedů, k-nejbližších sousedů, vyhledávání v rozsahu. (hádejte :) ) Byla to neuvěřitelná haluz, jinak se to asi říct nedá. Šla jsem těsně před obědem (což je špatnej termín sám o sobě), navíc mě vzali až skoro o půl hodiny pozdějc, protože zjevně byli ve skluzu po někom z předchozích, takže k prezentaci DP jsem se dostala teprve v okamžiku, kdy už jsem měla skoro končit celou zkoušku. V důsledku tohohle zpoždění docházelo například k situacím, kdy u spol. otázky (kterou jsem zrovna věděla) to po pár minutách Čmolík uzavřel se slovy: „Tak asi dobrý, já s tim nebudu zdržovat.“ No, tak měl aspoň Havran čas mě kvalitně topit na tý oborový otázce (učila jsem se kd stromy obecně, protože tohle podle mě v okruzích explicitně nebylo, ale asi jsem si dpgčka měla přečíst celý, když jsem věděla, že bude v komisi… můjboj no, uznávám) a naprosto neuvěřitelným způsobem ještě předtím rozsekat na diplomce. Dělalo to na mě dojem, že je předem rozhodnutej mi to nedat, a hledá cokoliv, na čem by mě moh zabít. Což se mu podařilo, takže jsem odešla s D z otázek a F z diplomky, ačkoliv jsem měla posudky C/D. Jenže podle Havrana prej nesplňuje zadání a není na ní vůbec nic přínosnýho, takže bohužel. Čili v červnu znova a lépe… Mimochodem, když mi to sděloval, tak ve svý kritice prohlásil, že to v...

### Other

**Q34 — 26.1.2022 (Společná), examiner: ?**

Jake jsou nejlepsi zname algoritmy, ktere resi opt. verzi TSP s 0 chybou. Jak velke instance je mozne takovymi algoritmy resit? Jaka je jejich podstata? Vysvetlete jednotlive casti. Jaka je jejich slozitost. Jaka je slozitost jednotlivych casti?

**Q35 — 2. 6. 2015 (Společná), examiner: Velebil**

NPC a NPH. Zamotal jsem se do definice reduce a od té doby jsem se v tom koupal, ale řekl jsem většinu co jsem měl připravené. U těchto témat je dobré nedodostat se do situace kdy se člověk splete, protože se díky tomů může odhalit víc neznalostí. C Celkově - Nutí vás jít co nejrychleji k vybranému tématu, omáčku moc neuplatníte.

**Q36 — 2. 6. 2015 (Společná), examiner: Velebil**

Algoritmy pro hledání minimální kostry Popsal jsem obecně co je kostra, zapomněl jsem na jeden podstatný detail (musí obsahovat všechny uzly), chviličku to ze mě doloval, nechal mě na tabuli nakreslit příklad a hned mi to došlo. Dále mě nechal mluvit, popsal jsem Prima a Kruskala, ukázal na grafu na tabuli, řekl jsem že oba vycházejí ze stejného principu (výběr nejlevnější hrany z řezu). Pak jsem ještě popsal Union Find a Borůvku. Překvapivě ho nezajímali složitosti (který mi trochu vypadly, uff :)), žádné doplňující otázky.

**Q37 — 4. 6. 2014**

Společná [TAL] [Rogalewicz] — Nevzpomínám si, na co přesně se ptal, ale působil velmi podobně, jako u předchozího studenta. Asi ne vyloženě zákeřně, ale úplně příjemný nebyl, hlavně co se jeho vyjadřovacích schopností týká (nemyšleno nijak zle).

---

## BE4M35KO — Combinatorial Optimization (KO)

**Topic frequency** (29 entries total):

| Topic | Times asked |
|---|---:|
| ILP — formulation, TSP via ILP, logical constraints | 15 |
| Network flows / cuts / Ford-Fulkerson | 11 |
| TSP — approximation, Christofides, k-OPT | 10 |
| CSP & AC-3 | 9 |
| Shortest paths / algorithms | 6 |
| Scheduling (Bratley / Horn / P||Cmax) | 5 |

### ILP — formulation, TSP via ILP, logical constraints

**Q1 — 22/6/2023 (Společná), examiner: ?**

Constraint satisfaction, AC3 Porovnal jsem problém s ILP, uvedl příklad (řešení Sudoku). Zadefinoval formálně CSP a uvedl obecný způsob řešení. Ukázal jednoduchý příklad, který byl v podstatě ILP (příklady doporučuji, zaberou hodně času, člověk si může vybrat konkrétní zadání, aniž by ho dostal od komise a nemusí pak zabíhat tolik do detailu), pak zadefinoval AC-3, co to znamená arc-consistency, napsal pseudokód a vyřešil příklad. Pak jsem řekl, že to je vše, co mám připraveno. Faigl řekl, že nemá žádné otázky, otočil se na zbytek komise, ti souhlasili. Tak se šlo dál.

**Q2 — 8.2.2023 (Společná), examiner: Šůcha**

formulace ILP a převod TSP na ILP úlohu. Celkově fajn obhajoba, jen u převodu TSP na ILP jsem nedal všechny omezující podmínky hned, ale postupně jsme se k nim dostali. Nakonec se mě zeptal, jak by šlo zadefinovat TSP ještě jinak a tato otázka směřovala k domácímu úkolu, kdy jsme si hráli s tzv. lazy podmínkami. To už jsem nedal a nakonec jsem za to dostal B.

**Q3 — 8.2.2023 (Společná), examiner: Šůcha**

Rozvrhovací úlohy P||Cmax a P|pmtn|Cmax. Určit do jaké třídy složitosti patří a popsat algoritmy pro řešení. Řekl jsem že P||Cmax je NP-hard, protože i 2||Cmax je NP-hard, a na to existuje redukce 2-partition. Pak že pmtn je relaxace, díky které to má polynomiální řešení, a naznačil jsem jak postupuje McNaughtnon. U P||Cmax chtěl slyšet jak se dá řešit optimálně. Řekl jsem, že se to určitě dá formulovat jako ILP a na to se zeptal na nějaké proměnné, které bych v tom použil. Úplně jsem zapomněl, že existuje i pseudopolynomiální algoritmus, který to řeší, ale ani se na něj neptal. Ještě jsem chvíli mluvil o heuristických metodách řešení a +- jsem popsal list scheduling.

**Q4 — 26.06.2018 (Společná), examiner: Werner**

Co je ILP? Třída složitosti? Relaxace pomocí LP. Co je totálně unimodulární matice a příklad úlohy? - U unimodulární matice jsem nevěděl definici, ale pán Werner naprosto super, snažil se pomáhat a když viděl, že se nechytám, tak řekl že mu to stačí a šli jsme dál.

**Q5 — 15. 6. 2016**

Společná - [KO] [Hanzálek] - Popsat ILP, TSP pomocí ILP, implikace, OR a fixní náklady v ILP.

**Q6 — 2. 6. 2015 (Společná), examiner: Vokřínek**

Algoritmy na nejkratší cesty, problem Obchodniho cestujiciho, heuristicke a aproximacni algoritmy

**Q7 — 2. 6. 2015 (Společná), examiner: Šůcha**

Formulace ILP, metody řešení, logické vazby, big M, formulovat obhcodního cestujícího.

**Q8 — 3. 6. 2014 (Společná), examiner: Demlová**

Hledání nejkratší cesty. Jak se změní složitost pokud jsou topologicky vrcholy uspořádány. Co by se stalo pokud existuje polynomiální algoritmus pro hledání cesty v obecném grafu a jaký by to mělo vliv na třídy složitosti. Chtěl jsem začít s dijkstrou a hned mě zarazila a chtěla to nadefinovat. Potom se zeptala na Floyd- Warshall a chtěla vědět co se stane s maticí po třech průchodech a jak bude vypadat. S tím topologickým uspořádáním mě dostala protože jsem si myslel, že se složitost nezmění ale je lineární.

**Q9 — 5.6. 2013 (Společná), examiner: Hanzálek**

TSP, definovat pomocí ILP, aproximační algoritmy pro TSP a jejich důkazy (Dvojitá kostra, Christofidesův alg.).

**Q10 — 5.6. 2013 (Společná), examiner: Hanzálek**

Definice TSP, operace OR a fixnich nakladu pomoci ILP

**Q11 — 25.1.2012**

Společná [KO] LP, rozhodovani, optimalizace, logicky formule a TSP GI: WTF: metody redukce barvy v obrazu (dither, half tone, barevna tabulka) * Všichni totálně v pohodě, Žára má dobrou náladu, byl milej. Hanzálek jakbysmet (to nikoho nepřekvapuje). Demlová veselá. Externista mlčí. Berka vypadá taky v klidu. Tzn.: Nebojte se! :) — Michal Benátský [https://web.archive.org/web/20231130204019/mailto:benatmic@fel.cvut.cz] 2011/06/07 22:24

**Q12 — 25.1.2012**

Společná [KO] [Hanzálek] Problém obchodního cestujícího. Ukázat složitost metrického TSP(převodem z HK). Existuje k-aprox. algoritmus pro řešení obecného (ne metrického) TSP? Dokažte. (opět převodem z HK, tentokrát ale ten druhý převod) - zkoušel Hanzálek a pak ještě chtěl ukázat TSP pomocí ILP. Papír kontrolovala i Demlová. GI: Raiozita. Výhody, nevýhody. Porovnání se sledováním paprsku. - zkoušel Žára — Martin Zachar [https://web.archive.org/web/20231130204019/mailto:zacham3@fel.cvut.cz] 2011/06/09 17:07

**Q13 — 25.1.2012**

Společná [KO] ILP, definovat dva problemy pomoci ILP (na miste pak po me chtel jeste cestujciho), popsat postup jak definovat problemy, jak zadat podminky (nasobeni velkym cislem pro vypnuti jedne z nerovnic atd.), Ax = b - co ta cisla v A a b znamenaji GI: rozdily mezi vizualizaci vektorovych a skalarnich dat, algoritmy pouzivane pri viz. skalarnich dat (marching cubes apod. a mapovani na barvy) — Kristýna Dudová [https://web.archive.org/web/20231130204019/mailto:dudovkri@fel.cvut.cz] 2011/06/07 21:47

**Q14 — 25.1.2012 (Společná)**

Metody reseni ILP slozitost pri optimalizacnich problemech a tech druhejch(co zi uz nevzpominam jake jsou a je mi to jedno) SI: Techniky správy a organizace rozsáhlých softwarových projektů. Nástroje pro správu verzí a vývojových větví zdrojových kódů, nástroje pro automatické generování dokumentace a podporu orientace v rozsáhlých projektech. Infrastruktury zajišťující spolupráci mezi vývojáři navzájem a i s uživateli. Systémy pro sledování a řešení chyb a uživatelskou podporu. Zkousejici se mne ptal co je wiki a zjeho vyrazu jsem pochopil, ze on to asi nevi :-) totalne me zdupali za DP, z ktere jsem mel posudky CB, nakonec mi z ni vykouzlili E a jeste mi rekli, ze mne malem vyhodili — janskpe1 [https://web.archive.org/web/20231130204019/mailto:janskpe1@fel.cvut.cz] 2011/06/08 12:24

**Q15 — 25.1.2012 (Společná), examiner: Hušek**

Úloha obchodního cestujícího. Aproximační algoritmy na TSP, - chcel po mne nejaky alogritmus co pouziva penalizacie ci nieco take… (Husek) SI: Architektura zaměřená na služby (SOA). Kvalita služeb (QoS), výběr založený na QoS, sociální výběr služeb, trust a reputace v otevřených SOA. (Patera) - ale nepytal sa ma vela na to… Rozsekali ma na DP hlavne teda Vyskocil + sa pridal aj Slavik… mriztoma [https://web.archive.org/web/20231130204019/mailto:mriztoma@fel.cvut.cz] 2011/06/08

### Network flows / cuts / Ford-Fulkerson

**Q16 — 26.1.2022 (Společná), examiner: Šůcha**

Definice 1|r,d|Cmax a jeho složitost. Popsat Bratleyho algo. - Velmi pohodová otázka. Popsal jsem 1|r,d|Cmax (1 zdroj, release, due date, minimalizace Cmax) a trochu naznačil notaci rozvrhovacích problémů. Šůcha se doptal na redukci, která dokazuje, že jde o NP-hard problém → 3-partition problem. Pak jsem na tabuli čmárnul Bratleyho, kde jsem naznačil, jak prořezává. Šůcha mě zarazil, že mu to stačí. Znamka: A?

**Q17 — 20. 06. 2018 (Společná), examiner: Filip**

Definovat graf, rozdíl mezi orientovaným a neorientovaným grafem. Definovat tah, sled a cestu. (Tohle je v první přednášce KO, kde jsou motivační příklady) Krásná otázka. Protože jsme to stihli asi za 3 minuty, tak se ještě doptával na reprezentaci grafu v paměti a na definici stromu a lesu. (Známka A)

**Q18 — 14. 06. 2018 (Společná), examiner: Hanzálek**

Maximální toky, Ford-Fulkersonův algoritmus a jeho složitost. Minimum capacity cut. Jak nalézt initial feasible flow pro FF. Popsal jsem max flow graf, lb ub flow, pak se Hanzálek doptával jak to funguje, že hledám ty zlepšující cesty. Spíš mě tím vedl a nechal mě odpovídat, než že bych to tam odřikával, snažil se i napovídat. K initial feasible flow jsme se ani nedostali, protože řekl, že mu to stačí. (známka B).

**Q19 — 5.6. 2013 (Společná), examiner: Hanzálek**

Toky, řezy, formulace LP, Ford-Fulkerson, nenulový počáteční Tok (za ten byl hodně štastný)

**Q20 — 25.1.2012**

Společná [KO] [Šůcha] Rozvrhování na jednom procesoru + popsat Bratleyův algoritmus. GI: (Slavík) Reprezentace 3D objektů, implementace množinových operací, případně převod mezi reprezentacemi. (spíš VIZ než DPG) * Slavík tradičně blbě rejpal a navíc se se mnou skoro hádal, že na papíře něco nemám, co jsem tam měl. A že to nenašel tam, kde to hledal, tak to je prej taky špatně… Docela mě tim přístupem zklamal. * Šůcha kladl docela inteligentní otázky a chtěl detailně vědět prořezávání v Bratleyově algoritmu. * Tuším, že to byl Vyskočil, tak ten mě docela dusil při otázkách na DP. Hledal v tom samý kraviny. — Jan Zimandl [https://web.archive.org/web/20231130204019/mailto:zimanjan@fel.cvut.cz] 2011/06/07 23:06

**Q21 — 25.1.2012 (Společná)**

měl jsem toky v sítích a svíčkový řezy, pak jsem něco čmáral pastelkama na tabuli a tvářil jsem se u toho dost zabraně. Z SI jsem dostal SOA celej okruh. Jestli někdo chcete vědět, jak můžete s dvěma béčkama z posudků odcházet s éčkem od komise (a to mi prej ještě přilepšili), klidně vám to řeknu. Devět měsíců práce a 20 tisíc řádků kódu je malej rozsah a měli byste přidat! Jo a Hušek umí KO, tak si nemyslete, že na něj vyzrajete :) Prostě úplná haluz, školu nechci už nikdy vidět. Leda že bych tam učil a snažil se dělat všechno 10x líp, než ty lovci grantů, který tam teď seděj a dělaj si tu svoji vědu. Pája

**Q22 — 25.1.2012 (Společná), examiner: Hanzálek**

Toky, rezy, ford=fulkerson a omezeni minimalniho toku: Skoro nic jsem nevedel, tim myslim, ze jsem tusil, ze ff je noce se zlepsujici cestou a minimalni pripustnej tok, jsem si rekl, ze nepotrebuju. Napred me hanzelek rekl, ze jsem dost umelecky neco napsal a pak ze to chtej znat presne, dusicka, hanzalek spis se snazil, abych neco rikal, vyskocil neustale do toho skakal a ptal se na vis nevis otazky (je to zavisle na poctu uzlu/hran/velikosti toku atp), osobne bych se vyhodil SI (klima): Webove sluzby, jazyky na popis ws, hovna a nejaky dalsi veci k ws: docela dlouho jsem doslova blekotal, obcas se na neco zeptal, parkrat me dostal (treba soap, jestli nutne musi byt pres http), ale celkove takova mila rozprava, vsichni dobra nalada. DP: posudky A/A, vecne otazky k DP nakonec A dp a D zkouseni, ale do posledni chvile jsem nevedel BTW: pres 2 patra se rozlihajici mlasknuti ruky a rostouci ples behem vypraveni (a zakrouceni hlavou), rozhodne neprida na odvaze, hadejte, kdo byl ten dotycnej Jakub Belescak [https://web.archive.org/web/20231130204019/mailto:jakub.belda@gmail.com] 2011/06/10 04:22 (sry za preklepz, ale trochu jsem to oslavil)

### TSP — approximation, Christofides, k-OPT

**Q23 — 26.1.2022 (Společná), examiner: ?**

Jake jsou nejlepsi zname algoritmy, ktere resi opt. verzi TSP s 0 chybou. Jak velke instance je mozne takovymi algoritmy resit? Jaka je jejich podstata? Vysvetlete jednotlive casti. Jaka je jejich slozitost. Jaka je slozitost jednotlivych casti?

### Shortest paths / algorithms

**Q24 — 18.06.2019 (Společná), examiner: Genyk-Berezovskyj**

Popište úlohu hledání maximálního toku v síti a úlohu hledání maximálního párování v bipartitním grafu. Jak spolu souvisí tyto dvě úlohy? Uvažte případ, že bipartitní graf je hranově neohodnocený a také případ, že je hranově ohodnocený. Určete jak řádově velký musí být graf na vstupu těchto úloh, aby řešení každé z nich zabralo cca 1 hodinu na běžném osobním počítači. Předpokládejte implementaci v kompilovaném jazyce. Popsal jsem slovně, jak vypadá graf u toků, jak jsou ohodnoceny hrany a stručně jak funguje Ford- Fulkerson. Pak jsem přešel k maximálnímu párování a zmínil jsem, že na něj lze graf převést. Ptal se, co je maximální tok a pak co je řez. K otázce jsem nakreslil jednoduchý graf, nenaznačil jsem orientaci hran šipkami; ani později, kdy jsem se odvolával ke grafu jsem nenaznačil orientaci, zpětně, když nad tím přemýšlím, tak to asi bylo docela matoucí. U bipartitních grafů jsem se trochu zamotal, nedokázal jsem je vhodně definovat. Na hranové ohodnocení se mě naštěstí neptal, u toho jsem neměl tušení (kromě „Hungarian“ algoritmu, který je ale vhodný jen pro kompletní grafy a který jsem nestihl zmínit). Taky jsem nevěděl jak vypočítat čas běhu, ale nakonec víceméně stačilo říct časovou složitost. (Podotkl, že moderní počítač vykoná cca 10^8 operací za sekundu.) Zeptal se mě, proč Ford Fulkerson při výběru nejkratších zlepšujících cest trvá O(m^2 * n), to jsem nevěděl. Hodnocení C.

**Q25 — 20. 06. 2017**

Společná - [PAL] [KO] [Berezovskyj] - Specifikujte, kdy je hledání nejkratších nebo nejdelších cest polynomiální. Existuje případ, kdy je to těžká úloha s exp. složitostí? Řešte: Úloha je najít min kostru v úplném grafu s N vrcholy, jehož váhy hran jsou floaty. Graf je buď typu A nebo B. V A leží všechny váhy hran v intervalu (100, 200). V B může váha hrany nabývat jen jedné z 21 hodnot -1.0, -0.9, … 0.9, 1.0. navrhněte a popište metodu řešení pro A a B. Jak velký graf dokáže navržený alg. zpracovat za 1 sec na normálním PC?

**Q26 — 5. 6. 2014 (Společná), examiner: Kubr**

Nejkratší cesty v grafu. Popsat algoritmy a rozdily mezi nimi Byl na mě hodnej, i když jsem se v tom pak trochu zamotal, když ho zajímala třída složitosti při přístupu do pole a způsob reprezentace prioritní fronty

**Q27 — 25.1.2012**

Společná [KO] [Hanzálek] Nejkratší cesty, Floydův algoritmus, záporné hrany - Ptal se Hanzálek - Krásná otázka :) GI Animace a modelování šatů - Berka - Chtěl to z fyzikálního hlediska, chtěl znát názvy článků a spojitej a pružinovej model mu nestačily… Ale byli hodný, i když sem Berkovi skoro nic neřek, tak odborná za C, Diplomka B a celkem B :) Komise - Žára, Havran, Velebil, Felkel(nebyl tam), nějaká externistka na grafiku

### Scheduling (Bratley / Horn / P||Cmax)

**Q28 — 5.6. 2013**

Společná [KO] [Hanzálek] rozvrhovani na jednom procesoru, nejaky algoritmus a rozvrhovani s podminkou suma Cj * Wj

**Q29 — 25.1.2012**

Společná [KO] Podmnožina z otázky 9.: Rozvrhování na paralel. proc., Algoritmy pro P||Cmax a P|pmtn|Cmax a jejich čas. náročnost. Vysvětlit časovou náročnost P2 ||Cmax. SI: Podmnožina z 21.: Cloud, definice, klasifikace (IaaS, PaaS…), výhody, nevýhody cloudu, třída app ne/vhodná pro nasazení na cloud. Doplňková ot: obsah SLA. Pan Šůcha chtěl poměrně přesně definované odpovědi. Pan Klíma ok. Jinak obhajoba byla celkem v pohodě, musel jsem ale zodpovědět kupu dobře mířených dotazů. Vyskočil neřekl ani slovo ani při jedné z částí. — Michal Founě [https://web.archive.org/web/20231130204019/mailto:founemi2@fel.cvut.cz] 2011/06/07 18:48

---

## BE4M36SAN — Statistical Analysis (SAN)

**Topic frequency** (5 entries total):

| Topic | Times asked |
|---|---:|
| Clustering (k-means, DBSCAN, hierarchical, spectral) | 3 |
| Robust regression / M-estimators | 1 |
| LDA / QDA / logistic regression | 1 |
| Dimensionality reduction (PCA, kernel PCA, t-SNE) | 1 |

### Clustering (k-means, DBSCAN, hierarchical, spectral)

**Q1 — 20.6.2023 (Oborová), examiner: Berka**

Vysvetlite princípy distance-based a density-based clustering a uveďte príklady algoritmov používajúcich tieto princípy. Popísal som k-means a DBScan. Chcel vedieť, aký tvar majú clustre z k-meansu. Závisí to od zvolenej metriky - ak L2 tak kruhový tvar, ak L1 tak štvorcový. Pri DBScan nie je nejaký presný tvar clusterov. Chcel vedieť, ako sa určí k pri k-means a potom chcel ešte jeden distance-based clustering algoritmus, nič mi nenapadlo tak mi poradil hierarchické zhlukovanie. To som nejako high-level popísal, Mařík sa ešte spýtal ako vzniká taxonómia z hierarchického zhlukovania. Berka bol v pohode, nakoniec mi to dal za C. Diplomka za A, skúška za C, celkovo B. 8.2.2023 - prof. Ing. Filip Železný, Ph.D (předseda); doc. Ing. Jiří Kléma, PhD.; doc. RNDr. Natalie Žukovec; doc. Ing. Přemysl Šůcha, Ph.D. (CIIRC); prof. Ing. Petr Berka, CSc. (externista - VŠE)

**Q2 — 8.2.2023 (Oborová), examiner: Berka**

Distance-based a Density-based clustering - popsat, co to je a příklady algoritmů. Celkem docela základní a pohodová otázka. Vysvětlil jsem oba principy, jako příklad jsem zvolil K-means a DBSCAN, popsal jsem všechny parametry. Doptal mě na metodu určení optimálního k u k-means a na jiné metriky než sumu kvadrátů vzdáleností, u Hammingové vzdálenosti chtěl zobecnění na číslové vektory, kde jsem moc nerozuměl, co po mě chce, nakonec byl spokojen, když jsem řekl, že se jedná o součet rozdílů v odpovídajících složkách obou vektorů. Nakonec kvůli Hammingu (asi) jsem dostal B. Diplomka za A, zkouška za B, celkově A. 8.2.2023 - prof. Ing. Filip Železný, Ph.D (předseda); doc. Ing. Jiří Kléma, PhD.; doc. RNDr. Natalie Žukovec; doc. Ing. Přemysl Šůcha, Ph.D. (CIIRC); prof. Ing. Petr Berka, CSc. (externista - VŠE)

**Q3 — 20.6.2018 (Oborová), examiner: Mrazova**

Clustering, rozne techniky a ich nevyhody, specialne popsat K- means,spectral clustering,hierachical clustering, t-SNE, SOM, PCA. Pri tejto otazke chcem pozdravit autora otazok, dat SOM na statnice fakt nie je cool :) (ked uz, tak tomu venovat trochu viac ako doslova 2 slidy). Pri k-means chceli vediet ako sa prepocitavaju centroidi, problem s inicializaciou, ake vzdialenostne funkce mozme pouzit, ako urcit k (nebolo treba vediet priamo techniky, len stacilo povedat,ze dajake existuju na zaklade ktorych mozme vyhodnotit homogennost/kvalitu clustrov), ze to dokonverguje, ale nemusi optimalne → problem s outliermi. Spectral clustering a t-SNE preco potrebujeme a ako sa da vyuzit/ ake vyhody oproti beznemu k-means. Pri t-SNE chcli Kullerback divergenci. Pak hierarchical clustering len zakladne, ze vysledkom je dendogram, ako vyzera a ako to mozme vytvorit (pristup aglomerative vs divisive). Pak to zacalo byt funny, lebo vcelku dlho sa zastavili na SOM. Ako su definovane neurony, ako si pamataju polohu v priestore, ako vyuzivaju topologiu priestoru, ako funguju jednotlive neurony a ako vedia jeden o druhom, aku maju vyhodu SOM oproti inym technikam, nakreslit co sa deje pri spracovani jednotlivych bodov, ako ich interpretovat. Mal som pocit,ze SOM bolo zakladom otazky a asi to najdolezitejsie pre nich ale to bude asi tym, ze pani Mrazova vyucije neuronky na matfyze :) ale celkovo bola velmi mila, cloveka naviedla a snazila sa pomoct. Vystupovala velmi pr...

### Robust regression / M-estimators

**Q4 — 8.2.2023 (Oborová), examiner: Kléma**

Metody robustní regrese a konkrétně porovnat s lineární regresí. Jaké jsou rozdíly a na příkladech ukázat kdy je lepší použít robustní regresi a kdy ne. M-odhady a jak jsou v robustní regresi použité. U téhle otázky jsem trochu víc zmatkoval a Kléma se mě snažil nějak navést, ale nevím jestli mi to úplně pomáhalo. Začal jsem tím že jsem obecně řekl co jsou robustní metody a k čemu se používají, pak jsem začal mluvit o lineární regresi a jaké předpoklady má - chyba je z normálního rozdělení, data jsou homoscedastic. Kléma chtěl vidět jaké kritérium se optimalizuje a pak jak se to změní v robustní regresi. Došli jsme k tomu, že se může například změnit norma která se používá (místo minimalizace součtu čtverců, minimalizovat absolutní hodnoty), a nebo minimalizovat jiné kritérium místo průměru/součtu např. medián. Doptával jsem na vliv odlehlých hodnot. U M- odhadů jsme zůstali jen u medianu a pro jaké rozdělení je to odhad střední hodnoty (Laplace) a jakou to má spojitost s residuals. Při obhajobě dávali všichni pozor tak sporadicky, například Železný zvednul čas od času oči a zakoukal se do prezentace a po ní se pak zeptal na něco, co zahlédl. Kléma se docela probral, když jsem v během prezentace zmínil, že jedna metoda dosáhla signifikantně lepších výsledků než jiná. Po prezentaci ho zajímalo jaký, statistický test jsem pro tohle použil. Celkem: A + B (A + B) = A 14.6.2022 - doc. Ing. Jiří Kléma, PhD. (předseda), Mgr. Miroslav Blaško, Ph.D. (místopředs...

### LDA / QDA / logistic regression

**Q5 — 14.6.2022 (Oborová), examiner: Kléma**

Lineární a kvadratická diskriminační analýza (LDA a QDA). Definujte, popište jejich parametry, vysvětlete jejich rozhodováni a učení. Kam se tyto metody řadí v souvislostmi s dalšími metodami učení s učitelem. Při prezentaci práce všichni pozorní, zaujatí. Celkově byla komise velmi přátelská, v dobré náladě, zkoušení bylo návodné, bylo možné o věcech diskutovat a v klidu se dobrat dobrého výsledku i s velmi skromnou přípravou k oborové otázce („Kléma na hrad!“ :)). Celkově vše hodnotili A. Ctěnému čtenáři přeju hodně stěstí v jeho boji a přidávám nabytou moudrost, která je důvod k mírnému optimismu - komise je v závěrečné bitvě této války na Vaší straně. Váš Ing. 26.1.2022 - doc. Ing. Jiří Vokřínek, PhD (předseda), Ing. Radek Mařík, CSc. (místopředseda), Mgr. Jakub Mareček, Ph.D., Ing. Jiří Filip, Ph. D., doc. RNDr. Natalie Žukovec

---

## BE4M39VIZ — Visualization (VIZ)

**Topic frequency** (2 entries total):

| Topic | Times asked |
|---|---:|
| Volumetric data / direct volume rendering | 1 |
| Scalar field visualization | 1 |

### Volumetric data / direct volume rendering

**Q1 — 26.06.2018**

Oborová [VIZ] [Sedláček] Vizualizace volumetrických dat. Popište dva přístupy pro vykreslování vol. dat (přímé a nepřímé vykreslování), uvedte příklady metod. Popište detailně dvě metody vykreslování a to metodu First Hit a Average Intensity Projection. - Popsal jsem princip paprsku a převodu 3D na 2D. Nevěděl jsem moc podrobnosti a věci do hloubky. Chtěli po mě i vzorec na výpočet AIP, to jsem dohromady nedal. Otázky D. Celkově C, nálada byla dobrá, pán Slavík docela vysmátý. 21.06.2018 - Pavel Pačes (předseda), Miroslav Bureš (místopředseda), Tomáš Macek (externista), Natalia Žukovec, Radek Mařík Obhajoba DP - Poslouchali víceméně všichni, po prezentaci padlo několik dotazů od Macka i Maříka (věnoval jsem se testování), což vyústilo v diskusi, ve které jsem dovysvětloval věci ohledně implementace. Dotazy byly k věci, bylo vidět, že je to opravdu zajímá a chtějí to pochopit. Během prezentace měl Macek zvláštní, řekl bych překvapený výraz, který mě moc neuklidňoval.

### Scalar field visualization

**Q2 — 05. 09. 2017 (Oborová), examiner: Malý**

Charakterizovat skalární data ve 2D uniformní mřížce (nebo tak nějak). Popsat 2 nejběžnější metody jak tato data vizualizovat. S jakými problémy se při vizualizaci potýkáme a jak je řešit. Taková více kecací otázka, takže přesně tak to probíhalo. Asi 8 minut jsem vysvětloval vizualizaci - naivní způsob zobrazení přímo skalárů špatně, nejčasteji se mapují na barvy, popsal jsem transfer funkci, popsal jsem že je nutné volit správně barvy s pár příklady, jako druhý způsob jsem uvedl konturování, lehce popsal alg. marching cubes, že to jde dělat i přes transfer fci ale už to není tak přesné. Snažil jsem se obecně zaměřit na to, co je pro vnímání člověka intuitivní. Nyní se zeptal na barvy - kolik tak člověk dokáže rozlišit barev, jak je intuitivnější volit barvy (HSV OK, RGB nic moc). A byl konec Zkoušení s panem Malým bylo také příjemné. Vypadalo to podle nátury otázky, že jsem většinu doby mluvil a pan Malý spíše přikyvoval. Nakonec za A. Celkově B. Atmosféra zkoušení byla skělá, všichni z dané komise působili pozitivním dojmem. Hodnocení bych popsal jako mírné. 21. 6. 2017 - doc. Ing. Pavel Pačes, Ph.D. (P), Ing. Radek Mařík, CSc. (MP), Mgr. Viliam Lisý, MSc., Ph.D., Dr. Ing. Tomáš Macek (IBM ČR), doc. RNDr. Jaroslav Tišer, CSc.

---

## BE4M33OSW — Ontologies and Semantic Web (OSW)

_No questions in the OI-Wiki archive for this subject. To be added._

---

## BE4M33SSU — Statistical Machine Learning (SSU)

_No questions in the OI-Wiki archive for this subject. To be added._

---

## BE4M36SMU — Symbolic Machine Learning (SMU)

_No questions in the OI-Wiki archive for this subject. To be added._

---

## BE4M36DS2 — Big Data / NoSQL (DS2)

**Topic frequency** (3 entries total):

| Topic | Times asked |
|---|---:|
| MapReduce / Hadoop / HDFS | 1 |
| XPath / XQuery (legacy) | 1 |
| Big Data fundamentals (CAP, distribution, scaling) | 1 |

### MapReduce / Hadoop / HDFS

**Q1 — 22.6.2023 (Oborová), examiner: Lisý**

MapReduce (architektura, funkce, toky dat, exekuce, použití). Hadoop (MapReduce, HDFS). U MapReduce jsem popsal mapper a reducer funkce, tam chtěl zkoušející vědět formát vstupů a výstupů, pak jsem nakreslil schéma komunikace. Důležitá byla shuffle fáze a rict k cemu je MapReduce dobry. Pouziti jsem ukazal na histogramu slov (nejvic basic ukazka). K Hadoopu jsem rekl ze je to framework pro distribuovane systemy a ze HDFS se tvari jako jeden celistvy filesystem. Uprimne kdybych neabsolvoval nepovinny predmet BDT tak bych toho moc nevěděl.

### XPath / XQuery (legacy)

**Q2 — 22.6.2023 (Oborová), examiner: Krátký**

XPath (výrazy cest, osy), XQuery (konstrukce, FLWOR) Poměrně přímočará otázka na ukázku fungování XPath a XQuery. Chtěl zadefinovat osy (axes) u Xpath a následně ukázat FLWOR konstrukci na příkladu, který jsem si na místě vymyslel. Nejednalo se o nic těžkého. Krátký byl v pohodě. Myslím, že jsem si vytáhl nejlepší otázky dne. Atmoška byla fajn a všichni byli po ránu ještě fresh.

### Big Data fundamentals (CAP, distribution, scaling)

**Q3 — 26.1.2022 (Oborová), examiner: Krátký**

managment BigData (CAP, distribuce, škálování, replikace) + wide column databáze - Tady jsem byl trošku vykoupán a naběhl jsem si. Krátký se chytil věcí, které jsem zmínil BTW a šel do hloubky. Na většinu přípravy ani nedošlo a točili jsme se na tom proč BigData nejsou vhodné pro relační databáze a jaký je rozdíl mezi relační a wide-column databází. Diskuze se zvhrla trošku v to, kdy on tvrdil „To můžeme i s relační DB“ a já hledal argumenty, proč ne a sypal na něj další databázové technologie a jaké výhody mají. Nakonec to utnul, že mu to stačí. Znamka: B/C? Vyhozen za dveře, za 2 minuty pozván zpět a sdělen verdikt B/B. Za jednotlivé otázky mi známku neřekli, ale tipuji podle svého výkonu, že KO bylo za A a ty DB za C. Komise za mě v pohodě. Celkem: B + B = B 09.2021 - doc. Ing. Michal Jakob, PhD. (predseda), RNDr. Jiří Vyskočil, PhD., Ing. Michal Sojka, PhD., doc. Ing. Pavel Herout, PhD., doc. RNDr. Natalie Žukovec, Ph.D. Obhajoba DP - Jelikoz jsem mel implementacni diplomku, udelal jsem kratsi, udernejsi prezentaci na 10 stranek. Konzultoval jsem ji s kolegy, od kterych jsem dostal supr feedback. Parkrat jsem si ji doma zkusil, aby casove vychazela na 10m a naucil se zacatek + konec. Pote jsem pozdravil komisi, predstavil sebe, vedouciho a praci. Bohuzel, po skvelem startu jsem se na pul minutu uspesne zasekl (z prezentacnich dovednosti bych uz asi fasoval F - ten predmet mi moc nepomohl). Nastesti jsem to rozdejchal a nejak odprezentoval. Pote se...

---
