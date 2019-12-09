## Dáta

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

## Model

Vytvorili sme 2 modely:

### Reziduálnu sieť

Táto sieť pozostáva z blokov o veľkosti 5 vrstiev(4\*dense a 1 add vrstva). 
Za takýmto blokom nasleduje dropout vrstva. 

```
X_input = keras.Input(input_shape)
    
X = keras.layers.Dense(256, activation='relu')(X_input)
X = keras.layers.Dense(128, activation='relu')(X)
X = dense_residual(X,128)

X = keras.layers.Dropout(0.2)(X) 
X = keras.layers.Dense(64, activation='relu')(X)
X = dense_residual(X,64)

X = keras.layers.Dropout(0.2)(X) 
X = keras.layers.Dense(32, activation='relu')(X)
X = dense_residual(X,32)

# output layer
X = keras.layers.Dense(classes, activation='softmax', name='fc' + str(classes))(X)

# Create model
model = keras.models.Model(inputs = X_input, outputs = X, name='Residual_model')
```

### Výsledky

| dataset 	| Adam   	| SGD 	| dropout 	| Epoch 	| batch 	| acc         	|
|---------	|--------	|-----	|---------	|-------	|-------	|-------------	|
| 1       	| 0,0001 	| x   	| 0,2     	| po 40 	| 128   	| 24.77/24.72 	|
| 2       	| 0.0001 	| x   	| 0,2     	| 74    	| 128   	| 25.23/24.95 	|
| 3       	| 0.0001 	| x   	| 0.2     	| 44    	| 128   	| 24.43/24.43 	|
| 4       	| 0.0001 	| x   	| 0.2     	| 36    	| 128   	| 24.79/23.97 	|
| 5       	| 0.0001 	| x   	| 0.2     	| 40    	| 128   	| 25.14/24.81 	|


| dataset 	| Adam   	| SGD 	| dropout 	| Epoch 	| batch 	| acc         	|
|---------	|--------	|-----	|---------	|-------	|-------	|-------------	|
| 1       	| 0.0001 	| x   	| 0.5     	| 74    	| 128   	| 24.57/24.73 	|
| 2       	| 0.0001 	| x   	| 0.5     	| 74    	| 128   	| 24.05/26.45 	|
| 3       	| 0.0001 	| x   	| 0.5     	| 74    	| 128   	| 24.62/24.94 	|
| 4       	| 0.0001 	| x   	| 0.5     	| 74    	| 128   	| 24.74/23.79 	|
| 5       	| 0.0001 	| x   	| 0.5     	| 74    	| 128   	| 24.23/26.01 	|



| dataset 	| Adam 	| SGD   	| dropout 	| Epoch 	| batch 	| acc         	|
|---------	|------	|-------	|---------	|-------	|-------	|-------------	|
| 3       	| x    	| 0.005 	| 0.2     	| 44    	| 128   	| 24.37/24.73 	|
| 4       	| x    	| 0.005 	| 0.2     	| 36    	| 128   	| 24.81/23.75 	|
| 5       	| x    	| 0.005 	| 0.2     	| 40    	| 128   	| 24.12/25.97 	|

Najlepšie výsledky sme mali pri Adam optimizer, learning rate 0.0001, dropout vrstvou 0.5, batch_size 128. V tomto prípade sa náš model nepretrénovával ako v iných prípadoch. Keď sme použili SGD, tak už pri 40 epoche sa začal učiť naspamäť a presnosť na testovacej množine začala klesať.

### Baseline model

-> ReLU 256 -> ReLU 128 -> ReLU 64 -> ReLU 32 -> Softmax 18 ->

image: https://ibb.co/MgjKvvY

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

### Model1

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






