## Dáta
V našom projekte sme využili dataset ... Ktore tabulky

Vytvorili sme z neho 5 setov dát, nad ktorými sme učili naše modely.

### 1. Dataset

- Obsahuje počet strelených a obdržaných gólov za posledných 10 spoločných zápasov. 
- Obsahuje aj priemerné počty gólov z minulej sezóny.
- Celková dĺžka vektora je 34.

### 2. Dataset

- Obsahuje počet strelených a obdržaných gólov za posledných 5 spoločných zápasov.
- Obsahuje aj priemerné počty gólov z minulej sezóny.
- Obsahuje percentuálnu úspešnosť brankárov v minulej sezóne (0 - 1).
- Celková dĺžka vektora je 21.

### 3. Dataset

- Obsahuje počet strelených a obdržaných gólov za posledných 5 spoločných zápasov.
- Namiesto priemerných počtov gólov v minulej sezóne sme vypočítali priemer strelených a inkasovaných gólov v posledných 10 zápasoch za každź tím.
- Obsahuje aj priemernú úspešnosť branárov v minulej sezóne.
- Celková dĺžka vektora je  23.

### 4. Dataset

- Obsahuje úspešnosť brankárov v poslednom odohranom zápase pre každý tím.
- Obsahuje počty strelených a inkasovaných gólov v posledných 5 zápasoch pre každý tím osobitne. 

### 5. Dataset 
- Obsahuje úspešnosť brankárov v poslednom odohranom zápase pre každý tím.
- Obsahuje počty strelených a inkasovaných gólov v posledných 5 zápasoch pre každý tím osobitne.
- Zahrnuli sme aj štatistiky jednotlivých hráčov. A to ich formu z posledného odohraného zápasu. Zobrali sme 20 najdlhšie hrajúcich hráčov z daného tímu. Brali sme do úvahy iba ich štatistiky, ktoré mali vplyv na počet gólov v zápase, a to počet vstrelených gólov SG, počet asistencií A a počet striel na bránu NoS. Z tohoto sme vytvorili ich formu pomocou vzorca 0,7\*SG + 0,5\*A + 0.3\*NoS.

## Loss sanity check 
Jednou z metód ako si overiť správnosť modelu, tzv. sanity check, je to, že náš model má úvodnú stratu takú ako očakávame. Očakávanú stratu môžeme jednoducho vypočítať. Využívame Softmax klasifikátor, máme 18 tried. Z toho vyplýva rozdelenie - pravdepodobnosť **0.0556** pre jednu triedu(počet gólov). 
Takto získame očakávaný loss.
```
loss = 0
for value in array:
    loss -= (value*0.01*np.log(1/(number+1)))
loss
```
Výsledok je 2.89. Teda začiatočná strata v našom modeli by sa mala pohybovať okolo hodnoty 2.9 .

## Testovanie modelu

Dáta sme si rozdelili na testovaciu a validačnú množinu dvoma spôsobmi. 

- Najskôr sme testovali na len na poslednej sezóne, teda pomer train/test bol približne 8.5:1.5 . V tomto prípade bola presnosť nášho modelu menšia ako priemer. Tento výsledok bol spôsobený tým, že v poslednej sezóne padol v priemere na zápas iný počet gólov ako v ostatných sezónach.
- Druhý spôsob sme si vybrali náhodné rozdelinie dát, v pomere 8:2. V tomto prípade sme dostávali výsledky, ktoré boli zjednotené a stále sa držali okolo 25%.

## Experimenty

### Úspešnosť brankárov z daného zápasu
Vyskúšali sme experiment s tým, že sme vytvorili rovnaký dataset ako 4. dataset, no namiesto úspešnosti brankára z predchádzajúceho zápasu sme dali úspešnosť priamo z toho zápasu ktorý predpovedáme. Trénovali sme náš druhý model, zložený z reziduálnej neurónovej siete. 

#### Výsledok experimentu
Náš model sa dokázal naučiť predpovedať počty gólov s úspešnosťou 35%.

#### Zhodnotenie experimentu
Vieme, že úspešnosť brankárov pre daný zápas nemáme. No takto sme si overili, že sa náš model dokáže správne natrénovať, no s dátami, ktoré sú dostupné pred začiatkom zápasu nedokážeme predpovedať presný počet gólov v zápase. 
 

## Zhodnotenie

Keby sme hádali počet gólov v zápase na základe priemernáho počtu gólov v zápase, mali by sme takúto úspešnosť:
| počet gólov na zápas | úspešnosť |   | počet gólov na zápas | úspešnosť |
|----------------------|-----------|---|----------------------|-----------|
| 1                    | 2.68%     |   | 10                   | 1.72%     |
| 2                    | 2.50%     |   | 11                   | 1.90%     |
| 3                    | 15.37%    |   | 12                   | 0.32%     |
| 4                    | 9.43%     |   | 13                   | 0.28%     |
| 5                    | 24.54%    |   | 14                   | 0.02%     |
| 6                    | 10.40%    |   | 15                   | 0.04%     |
| 7                    | 17.97%    |   | 16                   | 0.01%     |
| 8                    | 5.57%     |   | 17                   | 0.01%     |
| 9                    | 7.24%     |   |                      |           |

Z tejto tabuľky vyplýva, že najlepšie výsledky by sme mali, keby sme odhadli 5 gólov v každom zápase. 

Z našich testov nám vyplýva, že predpovedať počet gólov v zápase nie je jednoduché. Obidva naše modely nám dokázali predpovedať s rovnakou úspešnosťou, a to vždy okolo 25%, teda rovnako ako keby sme na každý zápas predpovedali 5 gólov. Lepšiu presnosť sme nedokázali docieliť ani so zmenou jednotlivých hyperparametrov.

Myslíme si, že s informáciami, ktoré máme pred daným zápasom, nedokážeme predpovedať počet gólov s lepšou úspešnosťou ako je priemerná hodnota. Je to hlavne z toho dôvodu, že počet gólov záleží od rôznych faktorov a tie nie sú známe pred zápasom. Veľkú úlohu zohráva náhoda. 

Zhodnotenie - Ze sa to neda
Model1
    Dataset1-5
Model2
    Dataset1-5





