# TAL — Časté otázky u státnice

Archiv z `../../frequently_asked_questions/oi_wiki_committee_archive.md` filtrován na 37 zaznamenaných TAL otázek, seskupený podle tématu s odkazem na příslušnou sekci v `summary.md`.

**Formát.** Každá otázka je v originálním českém/slovenském znění s datem státnice, typem otázky (Společná/Oborová), jménem zkoušejícího a komentářem studenta o průběhu zkoušení a hloubce požadované odpovědi.

**Tip ke studiu:** pozornost věnujte zejména *doplňujícím otázkám*, které zkoušející kladli — ty ukazují, do jaké hloubky komise jde. Často je rozdíl mezi B a A právě v tom, jestli umíte definici recitovat, nebo ji umíte i vysvětlit a odvodit důsledky.

## Přehled frekvencí témat

| Téma | Počet otázek | Sekce summary |
|---|---:|---|
| Redukce a Cookova věta | 21 | §3.4–3.6 (Polynomiální redukce; NPC; Cookova věta) |
| Třídy P, NP, NPC, co-NP | 5 | §3.3, §3.5, §3.9 (Třídy P/NP/NPC/co-NP) |
| Rekursivní a rekursivně spočetné jazyky / nerozhodnutelnost | 6 | §6 (Rekursivní a rekursivně spočetné jazyky; diagonální a univerzální jazyk; Riceova věta) |
| Turingovy stroje | 1 | §2 (Turingovy stroje — DTM, vícepáskové, NTM) |
| Heuristiky a aproximace | 1 | §3.8 (Heuristiky, aproximační algoritmy, Christofides) |
| Ostatní / širší tématika | 3 | různé sekce summary.md |

---

## Redukce a Cookova věta

**Souvisí s:** §3.4–3.6 (Polynomiální redukce; NPC; Cookova věta)

### Q1 — 22.6.2023 (Společná), zkoušející: Žukovec

Redukce a polynomiální redukce úloh / jazyků. Cookova věta. Naznačte redukci SAT na 3-CNF SAT. Oba problémy zformulujte. Popsal jsem všechny definice a na konkrétní klauzuli ukázal redukci SAT na 3-CNF SAT, tam chtěla paní Žukovec dovysvětlit, jaktože je ta redukce polynomiální.

### Q2 — 22.6.2023 (Společná), zkoušející: Blaško

Popište DTM, NTM a popište k čemu se využívají. Uveďtě příklady alespoň tří dalších modelů Turingova stroje a diskutujte jejich ekvivalenci ve srovnání s DTM/NTM. Porovnejte časovou a paměťovou náročnost těchto modelů. U této otázky jsem byl zaskočen tím, že TAL zkouší Blaško namísto Žukovec, ale asi jsem na tom jen vydělal. Popsal jsem polovinu tabule jen definicemi. Zadefinoval jsem DTM a NTM. Dále jsem uvedl příklady dalších TM modelů - TM s „k“ páskami, Randomizovaný TM, Univerzální TM. Pro každý jsem se pokusil rozepsat jak fungují a k čemu se využívají. U rozepisování fungování univerzálního TM mě Blaško zastavil že mu to stačí.

### Q3 — 26.1.2022 (Společná), zkoušející: Žukovec

Redukce a polynomialni redukce uloh/jazyku, Cookova veta. Naznacte redukci SAT na 3-CNF SAT. Oba problemy zformulujte.

### Q4 — 26.1.2022 (Společná), zkoušející: doc. RNDr. Natalie Žukovec, Ph.D.

Definujte třídy P, NP, NPC a co-NP. Do kterých tříd patří 4-barevnost grafu? Problém zformulujte a vše zdůvodněte. Chtěl jsem začít formulací Turingova Stroje, to mě zastavila a říkala ať přeskočím. Tak jsem definoval P a NP pomocí TM. Poté jsem zadefinoval redukci a polynomiální redukci. Díky ní jsem definoval třídu NPC. Poté jsem laicky zadefinoval co-NP. Nakonec jsem vysvětlil co je 4-barevnost grafu, ale pak jsem se začal dost dopit, když se mě zeptala jak bych to vyřešil, co je to nedeterministický algoritmus atd… Jediné co bylo třeba odpovědět je, že se to dá ověřit pomocí nedeterministického alg., kvůli tomuto jsem málem zkoušku neudělal. Nicméně cítil jsem z ní, že mě nechce potopit, jen prostě cítila, že tam mám mezeru a čím víc mě chtěla navíst na odpověď tím víc jsem cítil jak se odhaluje moje nepochopení, a stres mi k tomu taky moc nedopomohl. Nakonec to odpověděl někdo jiný z komise, a tím otázka zkončila. Známka: E Odborná [BIN] [doc. Ing. Jiří Kléma, PhD.] - Zarovnání biologických sekvencí: jak a proč jej provádíme, popište alespoň jedno exaktní a jedno heuristické řešení zarovnání dvou sekvencí, k jakým změnám dojde pokud budeme zarovnávat více sekvencí současně. Po průseru z první otázky bylo jasné, že se pohybuju dost na tenkém ledě. Bylo cítit, že pokud doblbnu i tuto otázku, náhradní termín mě nemine. Myslím, že si toho byl vědom i pan docent Kléma. Poté, co jsem nastínil nějaký úvod o tom proč potřebuje...

### Q5 — 26.1.2022 (Společná), zkoušející: Mgr. Adam Rogalewicz, Ph.D.

Jazyky rekurzivní a rekurzivní spočetné. Popsat co to je (ve vztahu k Turingově stroji), uvést příklady úloh a to včetně úlohy co je mimo R i RS. Jak se zařadí R a RS v Chomského typech jazyků. Vztah R a RS, co když úloha i doplněk RS. Další věci, které si již nevybavuji. Zkoušející mi přišel velmi příjemný, nesnažil se mě v ničem vykoupat i když jsem se na pár věcech trochu zastavil či zamotal a něco jsem i řekl trochu špatně, ale většinou jsem to uvedl na pravou míru. Známka: B

### Q6 — 21.06.2018 (Společná), zkoušející: Žukovec

P, NP, NP-úplné a NP-těžké úlohy. Do jaké třídy složitosti patří hledání nejkratších cest v grafu. Jmenujte úlohu, která nepatří do NP. - Zadefinoval jsem všechny čtyři třídy plus polynomiální redukci, kterou jsem použil v definici NP-úplných a NP-těžkých úloh. Řekl jsem, že hledání nejkratší cesty v grafu patří do třídy P, pokud graf neobsahuje záporné cykly, v opačném případě patří do třídy NP-úplných problémů. Pak jsem zmínil Postův korespondenční problém jako příklad úlohy která nepatří do NP. Hned ze začátku se akorát doptala, jaké úlohy v definici mám na mysli, což jsem nepochopil, pak to upřesnila a chtěla slyšet, že jsem to definoval pro rozhodovací problémy a že pro vyhodnocovací i optimalizační problémy to platí stejně, protože se dají mezi sebou převádět. Více mi do toho neskákala, celou dobu se tvářila spokojeně a po každé definici kývla, že souhlasí. Hodnocení - A

### Q7 — 20.6.2018 (Společná), zkoušející: Zukovec

Definovat NP a NPC, polynomialni redukce, Cookova veta a jeji zneni. Bezny postup, ked im nebolo nieco jasne tak sa doptali a velmi docenili, ked to clovek rovno nakreslil na tabulu.

### Q8 — 12. 06. 2018 (Společná), zkoušející: Tišer

Co je Cookova věta, důkaz že polynomiální redukce úloh je tranzitivní Stručně jsem popsal Cookovu větu, vysvětlil její důsledek a definoval NPC. Důkaz tranzitivity: máme li úlohy u, v, w tak že se u polynomiálně redukuje na v se složitostí p(n) (polynom) a v se polynommiálně redukuje na w se složitostí q(n) (taky polynom) pak redukce z u na w přes v má složitost q(p(n)), což je také polynom. Složitost je tedy polynomiální, tudíž se v polynomiálně redukuje na w.

### Q9 — 05. 09. 2017 (Společná), zkoušející: Demlová

Popsat TSP, Knapsack a toky. U každého uvézt do jaké třidy patří. Popsat rozdíl mezi aproximačním a heuristickým algoritmem, uvést od nich příklad. Rozhodnout, zda se tok může poly. redukovat na Knapsack. Popsal jsem dané úlohy. U toků se paní Demlová doptávala (řekněme že moje definice nebyla úplně nejpřesnější), já jsem se u toho trošku zasekával, ale postupně jsme se dobrali k řešení. Víceméně jsem u toků vysvětloval, co to vůbec je tok, kirhoffův zákon apod. Zde jsem špatně uvedl, že toky jsou NP- Hard. Následně jsem popsal aprox. a heuristický algoritmus, uvedl příklad pro TSP a byl konec (ještě ze mě dostala, že to je pro metrický TSP a pro obecný nemusí platit). Celkově zkoušení s paní Demlovou bylo příjemné, oceňuji její reaktivní přístup - tzn. taková

### Q10 — 21. 6. 2017 (Společná), zkoušející: Tišer

Turingovy stroje, rekurzivní a rekurzivně spočetné jazyky, algoritmicky neřešitelné úlohy. Definujte Turingův stroj, dokažte, že polynomiální redukce je tranzitivní relace Definici TS úplně přeskočil. Chtěl def poly redukce, pak definice tříd R a RS (na vztahy mezi nimi se neptal). Dál se ptal co to vlastně znamená, že ta redukce je polynomiální (na tom jsem dost drhnul - tam jsem nejdřív zkoušel formálně definovat asymptotickou složitost, ale co nakonec chtěl slyšet, a k čemu mě rychle dotlačil, bylo, že počet kroků toho algoritmu je nějaká funkce p(n), kde p je polynom a n vstup. Ohledně důkazu poly redukce jsem řekl něco ve smyslu „Předpokládejme, že máme úlohy u, v a w takové, že u se poly. redukuje na v, a zároveň se v poly. redukuje na w. A zároveň předpokládejme, že u se neredukuje na w. Nicméně existují algoritmy A a A' tak, že A redukuje u na v a A' v na w, čili můžeme aplikací těchto algoritmů na problém X této redukce dosáhnout: A'(A(X)). Nyní je třeba ještě dokázat, že tato redukce je polynomiální: tam jsem drhnul ještě víc, ale nakonec mě k tomu dokopal. Fakt hodně pomáhal. Pokud A' pracuje v polynomu p(n) a A v polynomu g(n), pak výsledný polynom bude p(g(n)). Tím zkouška skončila.

### Q11 — 15. 6. 2016

Společná - [TAL] [Bošanský] - Rekurzivní a rekurzivně spočitatelné jazyky, příklady jazyků a vztahy mezi nimi. Popsat randomizovaný TM a jejich třídy. Naprosto v pohodě učitel.

### Q12 — 2. 6. 2015 (Společná), zkoušející: Hekrdla

Definice polynomiální redukce problémů a jestli relace převoditelnosti je uspořádání – To bylo na papíře, ale při zkoušení se mě p. Hekrdla zeptal jen na to uspořádání. Začal jsem nejdřív definicí převoditelnosti, což pan Hekrdla možná nepochopil, nebo jsme si nějak nerozuměli, a začal mi něco vysvětlovat, což bylo podle mě trochu off topic, tak jsem chvíli nechápavě poslouchal, a pak jsem řekl, že to radši napíšu na tabuli, aby bylo jasno. Ukázal jsem reflexivitu a tranzitivitu. U antisymetrie jsem trochu spletl definici, takže jsem byl opraven že chci dokázat (a =< b & b =< a) → a = b. K tomu jsem řekl, že to neplatí, protože to, že 2 úlohy jsou na sebe navzájem převoditelné, neznamená, že se rovnají. Tudíž to není uspořádání. Pak se mě ptal, jak bych udělal, aby upořádání platilo, tak jsem nějak s dopomocí vymyslel, že by to platilo pro relaci ne na úlohách, ale na třídách úloh, které jsou stejně těžké (tj. a=<b a b=<a). Nakonec se mě zeptal, co by v takovém uspořádání byla množina NPC, já mu řekl, že maximum, a on byl spokojen. posudky A/A, obhajoba A, zkoušení A → celkem A 2. 6. 2015 SI - Doc. Ing. Jiří Vokřínek, Ph.D. -13141 - předseda, Ing. Adam Sporka, Ph.D. – 13139 – místopředseda, Ing. Zdeněk Buk, Ph.D. – 13142, Ing. Josef Hekrdla, CSc. – 13101, Doc. RNDr. Petr Šaloun, Ph.D. – VŠB Ostrava Obhajoba DP - Nástroj pro generování dokumentace vzdálených rozhraní (API) webových služeb aplikací Java Enterprise Edition - byli spokojeni, tema se jim ...

### Q13 — 21. 1. 2015 (Společná), zkoušející: Demlová

NPC, NP hard, Cookova věta a heuristické algoritmy pro úlohy v NP. → S pár drobnýma zaškobrtnutíma jsem popsal vše + odpověděl na její otázky → B UI: [BIA] [Kubalík] - Neuronové sítě, Popsat perceptronový neuron a pak jsem si mohl vybrat jestli chci popsat RBF nebo SOM sítě, vybral jsem si som → Trochu jsem se v tom motal a nepamatoval jsem si úplně přesně vzorečky, popis SOM už šel líp (jinak Kubalík byl strašně hodnej a radil, kde se dalo) → C posudky B/B, obhajoba B, zkoušení B,C → C, celkem C 20. 1. 2015 - Vokřínek (P), Navara (MP), Šůcha, Čmolík, Macek(IBM ČR) Obhajoba DP - OK, kromě Navary (kterýho chápu, že implementační práce moc nezajímala :) ) dávali docela pozor, zodpověděl jsem otázky z posudků a Šůcha a Macek se pak ještě ptali ze zájmu.

### Q14 — 5. 6. 2014 (Společná), zkoušející: Velebil

NP úplné úlohy a redukce úloh. PGI: [GVG] [Sýkora] - Matematický model perspektivní kamery a homografie. posudky A/B, obhajoba A, zkoušení A → A 5. 6. 2014 - Doc. Ing. David Šišlák, Ph.D. - K13136 - předseda, Ing. Jiří Vokřínek, Ph.D. - K13136 - místopředseda, Ing. Zdeněk Buk, Ph.D. - K13136, Ing. Jan Kubr - K13136, Ing. Josef Hekrdla, CSc. - K1310, Externí člen: Dr.-Ing. Tomáš Macek – IBM ČR, Tajemník: Jan Hrnčíř, MSc. Obhajoba DP - Na pohodu, pár dotazů ze zájmu

### Q15 — 5. 6. 2014 (Společná), zkoušející: Hekrdla

Co je to redukce a co je to polynomiální redukce. Je relace redukce tranzitivní, reflexivní, uspořádání? Řek sem nějakej základ, nevěděl sem co je uspořádání, ale ani to nějak neřešil… Pak se doptal na nějaký věci ohledně NPC úloh. Zkoušení bylo hodně v pohodě, dostal sem z tý otázky A, i když sem žádnej zázrak nepředved.

### Q16 — 5. 6. 2014 (Oborová), zkoušející: Hekrdla

Ulohy NP-uplne, NP-tezke. Co muzeme rict o uloze, o ktere vime, ze se na ni redukuje nektera NP-uplna uloha? (Hekrdla) Popsal jsem papir z obou stran, nakonec jsem z toho pouzil jen mensi cast. Rekl jsem definici NPC, vysvetlil polynomialni redukci a pak se me ptal na vztahy mezi tridami, napr. „Co by se stalo, kdybychom nasli DTM, ktery resi NP-tezkou ulohu?“. Celou dobu se usmival, nedelal problemy. diplomka B, otazky B, celkove B Tak TPJ sice ceka, ale nebojte se ho. :-) 5. 6. 2014 - Doc. Ing. David Šišlák, Ph.D. - K13136 - předseda, Ing. Jiří Vokřínek, Ph.D. - K13136 - místopředseda, Ing. Zdeněk Buk, Ph.D. - K13136, Ing. Jan Kubr - K13136, Ing. Josef Hekrdla, CSc. - K1310, Externí člen: Dr.-Ing. Tomáš Macek – IBM ČR, Tajemník: Jan Hrnčíř, MSc. Obhajoba DP - Všichni totálně znuděni, nedávali pozor, a pak nevěděli na co se zeptat :-)

### Q17 — 4. 6. 2014 (Společná), zkoušející: Tišer

Třídy složitosti P a NP. Polynomiální redukce. Redukce SAT na 3CNF SAT. Ptal se Tišer, přerušil můj popis postupu redukce a zadal mi převést jednoduchou instanci (a & b & c & d).

### Q18 — 25.1.2012 (Společná)

NP-úplné a NP-obtížné úlohy (definice, Cookova věta, polynomiální redukce) Zkoušel Hušek. Naprosto v pohodě, řekl že tam mám vše (hodně se mu líbil důkaz Cookovy věty) - prakticky jsem psal jen na to, na co se ptá otázka, nic navíc. Aby se neřeklo, dal pár doplňujících otázek na to, co je to vlastně vůbec to NP a jaké jsou vztahy P, NP, NPC a NP těžké. SI: WS + vlastnoti. Databáze + persistenntí frameworky. Zkoušel Klíma, takže opět pohoda. K WS jsem napsal minimum, co to je, jaké to má vlastnosti a nějaké termíny (soap, wsdl, rest, uddi), kde jsem ke každému napsal tak jednu větu. U DB jsem napsal, že máme relační a key-value, pak že máme nějaké ORM a přešel jsem na JPA. Doplňující otázky: jestli našlo uplatnění uddi v praxi, kdy se vyplatí používat ORM a kdy naopak ne. Obhajoba DP v pohodě, otázky měl jen Vyskočil (relativně k věci, ale je pravda, že některé moc přijemné nebyly. Nicméně pokud své DP opravdu rozumíte, neměli byste mít problém). — Roman Vaclavik [https://web.archive.org/web/20231130204019/mailto:vaclarom@fel.cvut.cz] 2011/06/08 17:49

### Q19 — 25.1.2012 (Společná)

Slozitost ulohy, algoritmu, P, NP, Cookova veta. Ptal se me Vyskočil, docela dost mi do toho rejpal. Napred se me pokusil pribit na formalni definici slozitosti, coz se mu nepovedlo, u Cookovy vety videl dukaz, tak se na ni ani moc neptal. Nakonec jsem se s nim dlouze dohadoval o tom, jestli polynomialni redukce musi probehnout v poly case. Nakonec jsem rekl, ze ano na NTM, on mi rekl, ze ne. V materialech mam, ze musi byt polynomialne slozita (coz na NTM v pripade P == NP je…(a proto se to taky dela))… Takze nevim, jestli narazel na NTM nebo na to, ze redukce nemusi byt v NP…to jsem fakt nepobral…kazdopadne me na tom slusne ukrizoval… SI: Bezpecnost v cloudu, porovnani s on-premise a local service provider. Private Cloud. Dotaz na SLA jakozto doplnujici otazka. Naprosto v pohode. Klima me skutecne hodne podrzel. U diplomky do me dost ryl Vyskocil. Nektery pripominky byly dobry. Jiny mi prisly divny (kdyz se me ptal, proc jsem nejaky architektonicky rozhodnuti neudelal jinak a implikoval, ze mam jiny pozadavky, nez jsem mel). A jiny mi prisly uplne ujety - proc nemam uzivatelskou dokumentaci (moje DP je javovska knihovna, proste jeden blbej jar, kterej se nakopiruje mezi knihovny…). Z hlediska prezentace mi to asi moc nevyslo…malo casu a asi jsem to spatne naformuloval, takze ty otazky pak podle toho byly divny… No vysledek: komise se podivala na muj prumer a patrne prehlasovala Vyskočila, aby mi nekazila cervenej diplom… Z cely obhajoby a statnic mam dost div...

### Q20 — 25.1.2012

Společná [TAL] Cookeova věta, co je NP-těžký problém (plus příklad NP-těžkého, co není NP-úplný). SI: Webové služby. Co to je, jaké se používají technologie, … Komise byla veselá, všecko v pohodě. Diplomka A/A, celkem jsem dostal B. (leden 2013) 23.1.2013, Komise: Havran (předseda), Bittner (místopředseda), Čmolík, Šůcha, Kohout (ZČU Plzeň)

### Q21 — 5.6. 2013

Společná [TAL] [Demlová] Demlová: Nejkratší cesty, popis problému, algoritmy, popsat algoritmy, jak by ovlivnil třídy složitosti fakt, že existuje poly. algoritmus, který řeší hledání vest v topolologicky označkovaném grafu. (výsl. C) Demlová byla, je a bude u statnic strašně hodná. Hodně pomáhala, když jsem řekl, nějakou (mnohdy zásadní) blbost, tak mě hned opravovala, ale dodávala, že v podstatě mám pravdu, nebo se aspoň usmívala. Rohodně strašně fajn jí v komisi mít, protože hodně zvedá náladu a nerejpe do diplomky.

---

## Třídy P, NP, NPC, co-NP

**Souvisí s:** §3.3, §3.5, §3.9 (Třídy P/NP/NPC/co-NP)

### Q22 — 2. 6. 2015 (Společná), zkoušející: Velebil

NPC a NPH. Zamotal jsem se do definice reduce a od té doby jsem se v tom koupal, ale řekl jsem většinu co jsem měl připravené. U těchto témat je dobré nedodostat se do situace kdy se člověk splete, protože se díky tomů může odhalit víc neznalostí. C Celkově - Nutí vás jít co nejrychleji k vybranému tématu, omáčku moc neuplatníte.

### Q23 — 5. 6. 2014

Společná [TAL] [Šusta] NPC a NP hard, pak cokova veta a priklady nad nimy NPC a NP hard, pak cokova veta a priklady nad nimy chtel odeme pak tuninguv stroj, kdys jsem chtel rict definicy tak me zarazil a zacal ode me chtit neco jine , popravde jsem nepochopil co odeme chce

### Q24 — 4. 6. 2014

Společná [TAL] [Rogalewicz] Třídy složitosti (nevím co přesně bylo v zadání). Ptal se externí člen, pan Rogalewicz, působil na mě, že není moc schopný vyjádřit na co se vlastně chce zeptat. Nakonec ho samotné třídy složitosti moc nezajímali a ptal se spíš na P, NP. Co by se stalo, kdyby byl polynom. alg, který řeší NP atd. Pak se ptal na PSpace a NSpace, vztahy mezi nimi, zde si odpovídají. Je pozor, pro třídy P a NP využíval DTIME(f(n)) a NTIME(f(n)), Demlová po chvilce řekl, že ona v přednáškách používala P a NP, po chvilce si zřejmě vyjasnili rozdíl a Demlová požádala studenta, ať dál používá DTIME a NTIME. Celkově byl takový rýpavý.

### Q25 — 3. 6. 2014 (Společná), zkoušející: Demlová

Třída P a NP; Příklad dvou úloh ze třídy P, dvou ze třídy NP a dvou úloh, které nepatří do NP (Uveď všechny použité předpoklady). Kam patří následující úloha: je dán ohodnocený orientovaný graf G, jeho dva vrcholy u, v a číslo k. Existuje nejkratší cesta, alespoň k?

### Q26 — 25.1.2012

Společná [TAL] [Velebil] definovat P a NP problemy a ke kazdemu uvest priklady GI: Barevny modely a komprese obrazku (Matas) * Otazky krasny, kdyz jsem je rozbalil, rikal jsem si jak to je v klidu. Nakonec to tak v klidu nebylo. K oborovy otazce GI jsem napsal asi 3 stranky, ale Matas jakoby to ignoroval a pri ustni se doptaval na nektery definice jako co je barva, gamut, jak PRESNE funguje DCT u JPEG komprese atd.. Obecne vzato at jsem mu rekl cokoliv, vzdycky se mu to nezdalo, vzdycky chtel neco vic, byt priznavam ze u otazky barva jsem se solidne zamotal.. Po prvni minute zkouseni u nej bych se sel nejraci zahrabat, docela me rozsekal. U otazky od Velebila uz to bylo lepsi a pomalu jsme se spolu dobirali k tomu, co chtel slyset. Nakonec mi dali z ustni E a z diplomky A (posudky A/A) a s prihlednutim k pomerne prumernym studijnim vysledkum jsem dostal vyslednou taktez prumernou - C. Mel jsem dojem, ze pisemna priprava otazek by mela slouzit jako zaklad hodnoceni ustni zkousky, tohle mi spis prislo jako „tak mi sem napiste vsechno co vite a ja vas pak zhodnotim podle toho, na co se budu ptat kolem“. Jinak receno Matase nezajimaji fakticke(encyklopedicke) znalosti, jako spis presne definice cehokoliv na co se zepta. Vzhledem k tomu, ze jsem mel pisemnou pripravu podle me docela dobrou, tak to E me docela nemile prekvapilo. Nu coz, na to se historie nepta.. Hodne stesti vsem v lete. Jinak zbyli dva „komisari“ : Sykora prakticky celou dobu mlcel, jen mel nejake konstruktivni do...

---

## Rekursivní a rekursivně spočetné jazyky / nerozhodnutelnost

**Souvisí s:** §6 (Rekursivní a rekursivně spočetné jazyky; diagonální a univerzální jazyk; Riceova věta)

### Q27 — 20.6.2023 (Spoločná), zkoušející: Demlová

Vysvetlite pojem nerozhodnuteľného jazyka/algoritmicky neriešiteľnej úlohy. Uveďte príklad takého jazyka/úlohy. Aký je vzťah medzi týmto pojmom a triedou rekurzívnych jazykov a triedou rekurzívne spočetných jazykov? Uvažujte následujúci problém: Rozhodnite, či Turingov stroj prijíma binárne slovo 00. Do ktorej triedy patrí jazyk tejto úlohy? Odpoveď poctivo zdôvodnite. Pojmy som definoval, potom som jej ukázal príklady nerozhodnuteľných jazykov/úloh. Naznačil som Poštov korešpondenčný problém a definoval diagonálny jazyk. Otázku s TM, ktorý prijíma binárne slovo 00 som moc nepochopil, povedal som, že patrí do triedy P, ale patrí do RS. Ešte sa spýtala kam patrí jazyk, ak jeho doplnok je rekurzívne spočetný - do triedy rekurzívnych jazykov. Od tej úlohy s binárnym slovom 00 som zostal nejaký domotaný, nakoniec mi dala „lepšie“ D. Bola celkom milá.

### Q28 — 26.1.2022 (Společná), zkoušející: Demlová

Vysvetlete vztah mezi rozhodovaci ulohou a jazykem. Definujte, co jsou to nerozhodnutelne problemy/jazyky a jejich vztah ke tride R a RE. Definujte pojem jazyk L1 se redukuje na jazyk L2.

### Q29 — 26.1.2022 (Společná), zkoušející: Žukovec

Definujte rekurzivni a rekurzivne spocetne jazyky. Do ktere tridy patri jazyk Postova korespondencniho problemu? Problem zformulujte. Vysvetlete co to je jazyk problemu.

### Q30 — 14. 06. 2018 (Společná), zkoušející: Žukovec

Definujte rekurzivní a rekurzivně spočetné jazyky. Uveďte příklady. Platí tvrzení, že pokud je jazyk rozhodován nedeterministickým turingovým strojem, tak je rekurzivní? Nejdříve jsem zadefinoval TM, potom rekurzivní a pak rekurzivně spočetné jazyky. U rekurzivně spočetný jsem řekl, že jsou algoritmicky neřešitelné a jako příklad uvedl Halting problem (problém zastavení TS). U tvrzení jsem řekl, že platí, protože to vychází přímo z definice rekurzivního jazyka, kde není specifikován typ TS. Doptala se na rozdíl mezi DTM a NTM a pár dalších otázek. Byla spokojená, za A.

### Q31 — 25.1.2012

Společná [TAL] Pojem algoritmus (Turingův stroj a jeho varianty) GI Částicové systémy (princip, uplatňující se síly, aplikace, zobrazování) Společnou otázku jsem si myslel, jak je v cajku, ale Velebil to po chvilce stočil k algoritmicky neřešitelným problémům a tam jsem si krapet zaplaval. Taky mi vytknul, že jsem neměl TS úplně dobře formálně zadefinovanej (popsal jsem to slovně + nějaký hokus pokusy o tu šestici, ale měl jsem tam asi nějaký chybky). Otázka z grafiky mě nepříjemně překvapila. Particly se braly v MMA, Berka v komisi nebyl a podle toho, co jsem si hledal o té Kolingerové, tak její obor byla spíš výpočetní geometrie (tj. VGA) a vyhledávání (DPG). Takže jsem se MMA učil jen tak zrychleně pro svůj klid, abych si pak nenadával. No, abych to zkrátil… sepsal jsem tam takový ty základní věci a pak ze mně dolovala ještě nějaký další pojmy, i pár (středoškolských) fyzikálních vzorečků jsem jí řekl :) Ale byla hodná a moc nerejpala (oproti tomu, co jsem si přečetl třeba na Exfortu). Hodní byli vlastně všichni (šel jsem druhý z rána), Žára byl milej (na rozdíl od příspěvku výše) a Havran taky neryl, takže nakonec jsem odešel ještě docela úspěšně. Komise: Slavík, Hušek, Mařík R. (TVS), Náplava (PIS), Hudec (externista)

### Q32 — 23.1.2013

Společná [TAL] [Čmolík] Co je asymptotická a amortizovaná složitost, popište, vysvětlete na příkladu s dynam. polem, které se při naplnění natáhne na dvojnásobek. GI: kd-stromy pro vyhledávání nejbližších sousedů, k-nejbližších sousedů, vyhledávání v rozsahu. (hádejte :) ) Byla to neuvěřitelná haluz, jinak se to asi říct nedá. Šla jsem těsně před obědem (což je špatnej termín sám o sobě), navíc mě vzali až skoro o půl hodiny pozdějc, protože zjevně byli ve skluzu po někom z předchozích, takže k prezentaci DP jsem se dostala teprve v okamžiku, kdy už jsem měla skoro končit celou zkoušku. V důsledku tohohle zpoždění docházelo například k situacím, kdy u spol. otázky (kterou jsem zrovna věděla) to po pár minutách Čmolík uzavřel se slovy: „Tak asi dobrý, já s tim nebudu zdržovat.“ No, tak měl aspoň Havran čas mě kvalitně topit na tý oborový otázce (učila jsem se kd stromy obecně, protože tohle podle mě v okruzích explicitně nebylo, ale asi jsem si dpgčka měla přečíst celý, když jsem věděla, že bude v komisi… můjboj no, uznávám) a naprosto neuvěřitelným způsobem ještě předtím rozsekat na diplomce. Dělalo to na mě dojem, že je předem rozhodnutej mi to nedat, a hledá cokoliv, na čem by mě moh zabít. Což se mu podařilo, takže jsem odešla s D z otázek a F z diplomky, ačkoliv jsem měla posudky C/D. Jenže podle Havrana prej nesplňuje zadání a není na ní vůbec nic přínosnýho, takže bohužel. Čili v červnu znova a lépe… Mimochodem, když mi to sděloval, tak ve svý kritice prohlásil, že to v...

---

## Turingovy stroje

**Souvisí s:** §2 (Turingovy stroje — DTM, vícepáskové, NTM)

### Q33 — 5. 6. 2014

Společná [TAL] [Hekrdla] Pravděpodobnostní algoritmy, randomizovaný turingův stroj, třída úloh TP. Navrhněte pravděpodobnostní algoritmus pro výpočet čísla Pí. Ke všemu jsem něco málo řekl, algoritmus jsem nenavrhl, tak se zeptal jestli znám nějaké jiné (testy prvočíselnosti), dál se neptal.

---

## Heuristiky a aproximace

**Souvisí s:** §3.8 (Heuristiky, aproximační algoritmy, Christofides)

### Q34 — 26.1.2022 (Společná), zkoušející: ?

Jake jsou nejlepsi zname algoritmy, ktere resi opt. verzi TSP s 0 chybou. Jak velke instance je mozne takovymi algoritmy resit? Jaka je jejich podstata? Vysvetlete jednotlive casti. Jaka je jejich slozitost. Jaka je slozitost jednotlivych casti?

---

## Ostatní / širší tématika

**Souvisí s:** různé sekce summary.md

### Q35 — 2. 6. 2015 (Společná), zkoušející: Velebil

Algoritmy pro hledání minimální kostry Popsal jsem obecně co je kostra, zapomněl jsem na jeden podstatný detail (musí obsahovat všechny uzly), chviličku to ze mě doloval, nechal mě na tabuli nakreslit příklad a hned mi to došlo. Dále mě nechal mluvit, popsal jsem Prima a Kruskala, ukázal na grafu na tabuli, řekl jsem že oba vycházejí ze stejného principu (výběr nejlevnější hrany z řezu). Pak jsem ještě popsal Union Find a Borůvku. Překvapivě ho nezajímali složitosti (který mi trochu vypadly, uff :)), žádné doplňující otázky.

### Q36 — 4. 6. 2014

Společná [TAL] [Rogalewicz] — Nevzpomínám si, na co přesně se ptal, ale působil velmi podobně, jako u předchozího studenta. Asi ne vyloženě zákeřně, ale úplně příjemný nebyl, hlavně co se jeho vyjadřovacích schopností týká (nemyšleno nijak zle).

### Q37 — 3.6. 2014

Společná [TAL] [Hekrdla] Definujte co znamena, ze jedna rozhodovaci uloha se redukuji (polynomialne) na druhou. Je tto relace tranzitivni, reflexivni, usporadani? (výsl. A) Vsechno v pohode, pan Hekrdla se porad usmival a pomahal, ikdyz jsem zapomela definici usporadani (tranztivni, reflexivni, antisymetricka) dal mi A. Podle me hodne zalezi na dojmu od prezentace.

---

## Zdroj

- Surový archiv: `../../frequently_asked_questions/oi_wiki_committee_archive.md` (sekce **BE4M01TAL — Theory of Algorithms (TAL)**).
- Původní PDF: `../../frequently_asked_questions/source_oi_wiki_committee_archive.pdf`.
- Pokrytí: státnice 2014–2023. Novější otázky lze doplnit, jakmile budou k dispozici.