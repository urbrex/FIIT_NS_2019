# FIIT_NS_2019
# Motivácia

Predikcia športových zápasov je celkom lukratívna a zaujímavá oblasť. Takúto predikciu môžeme rozdeliť na 2 oblasti, a to na dlhodobú predpoveď, kedy predpovedáme výsledok celej sezóny, alebo krátkodobú predpoveď, kedy predikujeme víťaza zápasu, prípadne počet strelených gólov.

Cieľom nášho projektu je predikcia počtu strelených gólov v zápase NHL. Pri počte strelených gólov v zápase zohráva úlohu viacero faktorov, ako napríklad forma jedného, či druhého klubu, forma jednotlivých hráčov a brankárov, miesto kde sa daný zápas odohráva a podobne. Preto sa pokúsime nájsť správny pomer medzi týmito faktormi.


# Podobné projekty

Pri štúdiu projektov využívajúcich neurónové siete na predpovedanie športových
výsledkov sme narazili na niekoľko podobných projektov. V tejto práci popíšeme nasledujúce dva:

-   “Use of Performance Metrics to Forecast Success in the National Hockey League” z Ottavskej univerzity -
    https://www.scribd.com/embeds/158486700/content?referrer=https%3A%2F%2Fwww.redditmedia.com%2Fmediaembed%2F1jzrs5
-   “Predicting football results using neural networks” od Andreho Ostraka
    https://mc.ai/predicting-football-results-using-neural-networks/

## Prvá práca 
Prvá práca sa zaoberá tou istou témou ako náš projekt, na ktorom tiež budeme predpovedať hokejové zápasy z NHL. V práci sa pracovalo z rôznymi vstupnými dátami, ktoré sa v zásade delili na základný model a rozšírený model. V základnom sa pracovalo s nasledovnými štatistikami prevzatými z portálov www.TSN.ca a www.NHL.com

-   počet strelených gólov
    
-   počet obdržaných gólov,
    
-   rozdiel v počte gólov
    
-   úspešnosť presiloviek
    
-   úspešnosť oslabení
    
-   percentuálny počet striel z ktorých padol gól
    
-   úspešnosť brankárov
    
-   výherná séria
    

V rozšírenom modeli sa pracovalo so štatistikami ako:

-   priemerný počet striel za posledné 3 zápasy
    
-   Fenwick Close % (štatistika, ktorá spracuje držanie puku, počet striel na bránu a mimo bránu)
    
-   PDO- vyjadruje číslo, ktoré malo vyjadriť šťastie tímu v zápase, či v krátkodobom časovom období
    
-   počet gólov strelených v hre 5 vs. 5
    

Modely boli trénované na základných, rozšírených a zmiešaných dátach. Modely boli spracovávané štatistickou predpoveďou pomocou softvéru WEKA(boli tu použité 2 rôzne štatistické prístupy), naivným Bayesovym klasifikátorom a neurónovou sieťou vytvorenou pomocou softvéru WEKA.

S výsledkov experimentu vyplynulo, že modely so základnými a zmiešanými dátami majú lepšie výsledky ako model s rozšírenými dátami. Najlepšie výsledky mala neurónová sieť na zmiešaných dátach, ktorá dokázala predpovedať správne 59,4% zápasov.

Následne sa v práci vykonali ďalšie experimenty, v ktorých sa menil spôsob výpočtu PDO, ktorý však nemal veľký vplyv na výsledky.

## Druhá práca
Druhá práca, ktorú sa nám podarilo nájsť, sa od našej líšila v tom, že autor v nej nepredpovedal výsledky hokejových, ale futbalových zápasov z 11 európskych líg. Autor pracoval na datasete futbalových zápasov z rokov 2008-2016. Do datasetu boli pridané hodnotenia hráčov z hier FIFA od herného štúdia EA GAMES.

Autor si musel upraviť dáta hráčov, pretože každý hráč mal 38 rôznych zdatností. Pri analýze dát boli zistené mnohé závislosti medzi rôznymi zdatnosťami. Po vyhodnotení týchto závislostí boli odstránené niektoré zbytočné, či korelujúce.

Následne prebehlo testovanie na rôznych atribútoch využitím kNN. Z výsledkov experimentov na datasetoch so súčtom všetkých atribútov, súčtom všetkých atribútov podľa formácií, súčtom celkových hodnotení, súčtom celkových hodnotení podľa formácií, súčtom všetkých atribútov okrem chytania a súčtom všetkých atribútov okrem chytania podľa formácií vyplynulo, že je najvhodnejšie použiť celkové hodnotenia hráčov(53,7%) alebo celkové hodnotenia hráčov podľa formácií(53,6%).

Autor následne navrhol plne prepojenú neurónovú sieť s veľkým počtom neurónov na prvej vrstve, počet na ďalších vrstvách bol polovičný voči predchádzajúcej vrstve. Výsledky boli nestabilné a ani zmena veľkostí jednotlivých vrstiev nemala na výsledky veľký vplyv. Boli použité neurónové siete s veľkosťou najväčšej vrstvy 16, 32, 64, 128 a 256. Od veľkosti vrstvy 64 sa neurónové siete začali učiť naspamäť. Najlepšie výsledky mal dataset s celkovou silou hráčov a celkovou silou hráčov podľa formácie, ktorý bol trénovaný na NS 16–8–4–3. Jeho výsledky boli 53,6% pre dataset s celkovou silou a 53,7% pre dataset s celkovou silou podľa formácie.

Ďalším experimentom bolo pridanie tímových štatistík, ktoré výsledky iba zhoršili. Výsledky boli dobré(52%) pri modeli s dátami z posledných zápasov v kombinácii so všetkými atribútmi hráčov a plne prepojenou NS.

Najúspešnejšie modely sú následne testované na stávkach na 2000 zápasov. Tieto modely sú porovnávané so stávkovaním na domáci tím a stávkovaním na najnižší kurz. Z výsledkov vyplýva, že ak by sa vždy stavilo na domáci tím zisk by bol -54.1 eur, zisk pri stávkach na najnižší kurz by bol 15,9 eur, pri použití kNN by bol zisk 88.8 eur a pri použití plne prepojenej NS by bol zisk 99.8 eur.

# Dataset

Na trénovanie a aj na testovanie nášho modelu budeme využívať dáta zo stránky kaggle.com ([https://www.kaggle.com/martinellis/nhl-game-data](https://www.kaggle.com/martinellis/nhl-game-data)). Tento dataset obsahuje výsledky všetkých zápasov NHL od sezóny 2010/2011, až po koniec minuloročnej sezóny 2018/2019. Sú v ňom obsiahnuté aj podrobné dáta k jednotlivým zápasom

-   zoznam hráčov, ktorí nastúpili v danom zápase a zároveň aj ich štatistiky
    
-   zoznam brankárov, ktorí chytali v zápase
    
-   počet striel v zápase
    
-   a iné podrobné štatistiky.
    
My budeme využívať konkrétne tieto tabuľky:

### game
    
-  dáta o zápasoch a ich výsledkoch
    
-   11 434 riadkov, 16 stĺpcov
    
###  game_goalie_stats
    
-   dáta o brankároch, ktorí nastúpili v jednotlivých zápasoch
    
-   24 646 riadkov, 19 stĺpcov
   
### game_skater_stats
    

-   dáta o hráčoch, ktorí nastúpili v jednotlivých zápasoch
    
-   411 578 riadkov, 22 stĺpcov
    

### game_team_stats
    
-   podrobné informácie o zápasoch, ako počet striel a podobne
    
-   22 868 riadkov, 15 stĺpcov
    

###  team_info    

-   číselník pre tímy

-   33 riadkov, 6 stĺpcov

# Vysokoúrovňový návrh

Keďže na predpovedanie počtu gólov v jednotlivých zápasoch NHL vplýva viacero faktorov, našu neurónovú sieť budeme musieť trénovať na viacerých datasetoch, ktorých výsledky následne porovnáme

-   Prvým bude priemerný počet strelených a obdržaných gólov a výsledky vzájomných zápasov. Tento dataset nám bude slúžiť na porovnanie a vyhodnotenie.
    
-   Druhý dataset bude rozšírený o priemerný počet gólov v domácich/hosťujúcich zápasoch, a o úspešnosť brankárov.
    
-   Do tretieho datasetu pridáme formu, ktorú budeme počítať z počtu víťazstiev a gólov z posledných 10 zápasov.
    
-   Vo štvrtom datasete pozmeníme formulu na výpočet formy, a to tak že budeme brať do úvahy počet gólov v posledných 5 domácich/hosťujúcich (podľa toho, aký zápas má tím hrať) zápasoch a priemernú úspešnosť brankára.
-  Do posledného datasetu zahrnieme aj štatistiky jednotlivých hráčov ako napríklad počet plusových bodov, počet striel, gólov a odohraný čas
 
V projekte využijeme úplne prepojené neurónové siete, kde každá nasledovná vrstva bude mať polovičný počet neurónov ako predchádzajúca, keďže sa takéto neurónové siete osvedčili pri nami analyzovaných prácach. V projekte budeme experimentovať aj s inými vlastnosťami neurónovej siete aby sme zistili aká kombinácia parametrov a datasetu je najlepšia.

Naše neurónové siete budeme trénovať na dátach zo sezón 2010/2011 až 2017/2018. Testovacou množinou budú zápasy sezóny 2018/2019. Výsledky z našich neurónových sietí budeme porovnávať oproti štatistike, ktorá odhadne pre každý zápas počet gólov podľa priemerného počtu gólov na zápas hrajúcich tímov. Ďalšou štatistikou bude odhadovanie počtu gólov podľa priemerného počtu gólov z minulej sezóny. Úspešnosť neurónovej siete bude počítaná ako percentuálna úspešnosť správne odhadnutého počtu gólov. Následne by sme chceli porovnať predpovede našej siete s kurzami stávkovej kancelárie niké.sk a zistiť, či je úspešná do takej miery, aby dokázala generovať zisk.
