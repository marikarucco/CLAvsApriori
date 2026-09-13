# Cellular Learning Automata vs Apriori vs FP-Growth

Notebook dedicato all'implementazione e al confronto di tre approcci per
il **Frequent Itemset Mining**:

-   **Cellular Learning Automata (CLA)**, seguendo il metodo descritto
    nel paper *Frequent itemset mining using cellular learning automata*
    di Sohrabi e Roshani;
-   **Apriori**, implementato da zero;
-   **FP-Growth**, implementato da zero tramite la costruzione e
    l'analisi di un FP-Tree.

L'obiettivo principale è confrontare il comportamento e i **tempi di
esecuzione** dei tre algoritmi al variare della soglia minima di
supporto (`minsup`).

## Struttura del notebook

Il notebook è organizzato in quattro parti principali.

### 1. Cellular Learning Automata

La prima parte implementa il metodo CLA descritto nel paper di
riferimento.

Le principali strutture dati utilizzate sono:

-   `Item`: rappresenta un singolo item e il relativo supporto;
-   `Transaction`: rappresenta una transazione e il numero di volte in
    cui essa compare nel dataset;
-   `Cell`: rappresenta una cella dell'automa e contiene la relativa
    proximity list;
-   `Neighbor`: rappresenta un vicino all'interno di una proximity list.

Il processo di mining CLA comprende:

1.  individuazione degli **1-itemset frequenti**;
2.  **pruning e compressione** del dataset;
3.  costruzione delle **celle** e delle relative **proximity list**;
4.  pruning delle proximity list in base a `minsup`;
5.  scanning delle celle per generare gli itemset;
6.  rimozione degli itemset duplicati.

La funzione principale è:

``` python
cla_mining(dataset, minsup, max_len=3)
```

## 2. Apriori

La seconda parte implementa Apriori senza utilizzare librerie esterne
dedicate al frequent itemset mining.

L'algoritmo segue il classico approccio basato sulla generazione e sul
pruning dei candidati:

1.  individuazione degli 1-itemset frequenti;
2.  generazione dei candidati di dimensione successiva;
3.  verifica della **proprietà di antimonotonicità**;
4.  conteggio del supporto dei candidati attraverso una scansione del
    dataset;
5.  selezione degli itemset frequenti;
6.  ripetizione del processo fino a `max_len` oppure fino all'assenza di
    nuovi itemset frequenti.

La funzione principale è:

``` python
apriori_mining(dataset, minsup, max_len=3)
```

## 3. FP-Growth

La terza parte implementa FP-Growth utilizzando una struttura dati
**FP-Tree (Frequent Pattern Tree)**.

La classe principale è:

``` python
FPTreeNode
```

che mantiene:

-   nome dell'item;
-   supporto;
-   riferimento al nodo padre;
-   dizionario dei nodi figli.

Il processo comprende:

1.  individuazione degli item frequenti;
2.  ordinamento degli item in base al supporto;
3.  costruzione dell'**FP-Tree**;
4.  costruzione della **header table**;
5.  generazione delle **conditional pattern base**;
6.  estrazione ricorsiva degli itemset frequenti;
7.  rimozione dei duplicati.

La funzione principale è:

``` python
fpgrowth_mining(dataset, minsup, max_len=3)
```

A differenza di Apriori, FP-Growth evita la generazione esplicita di
tutti i candidati e sfrutta una rappresentazione compatta delle
transazioni.

## 4. Benchmark

L'ultima parte del notebook confronta **CLA, Apriori e FP-Growth** sui
seguenti dataset:

-   **Mushroom**
-   **Retail**
-   **PUMSB**
-   **Kosarak**

I dataset vengono scaricati automaticamente dal repository FIMI tramite
`wget`.

Per ogni dataset vengono utilizzati i seguenti valori di `minsup`:

``` python
[0.02, 0.04, 0.06, 0.08]
```

Il valore percentuale viene convertito in un supporto assoluto in
funzione del numero di transazioni del dataset.

Per ogni combinazione dataset/`minsup` vengono misurati:

-   tempo di esecuzione di CLA;
-   tempo di esecuzione di Apriori;
-   tempo di esecuzione di FP-Growth;
-   numero di itemset individuati;
-   uguaglianza degli insiemi di itemset prodotti dai tre algoritmi.

I tempi vengono infine visualizzati tramite grafici che riportano:

-   asse X → `minsup`;
-   asse Y → tempo di esecuzione in secondi.

## Dataset

I dataset utilizzati appartengono alla raccolta **FIMI** e vengono
scaricati direttamente nel notebook:

``` text
mushroom.dat
retail.dat
pumsb.dat
kosarak.dat
```

Il notebook si aspetta di trovare i file nella directory `/content`,
coerentemente con l'ambiente Google Colab.

## Requisiti

Il notebook utilizza Python 3 e le seguenti librerie:

``` text
matplotlib
```

Le altre funzionalità utilizzate (`time` e le strutture dati native di
Python) fanno parte della libreria standard.

## Esecuzione

Il notebook è pensato per essere eseguito in **Google Colab**.

1.  Aprire il notebook `CLAvsApriorivsFP_Growth.ipynb`.
2.  Eseguire le celle in ordine.
3.  I dataset verranno scaricati automaticamente.
4.  Verranno eseguite le implementazioni CLA, Apriori e FP-Growth.
5.  Al termine verranno prodotti i grafici relativi ai benchmark.

È possibile modificare la lista:

``` python
minsup_fractions = sorted([0.02, 0.04, 0.06, 0.08])
```

per effettuare il confronto con soglie di supporto differenti.

È inoltre possibile modificare `max_len` per controllare la dimensione
massima degli itemset considerati.


## Obiettivo del confronto

Il benchmark permette di osservare come i tre algoritmi si comportano su
dataset con caratteristiche differenti e come la variazione di `minsup`
influenzi il tempo necessario per estrarre gli itemset frequenti.

Oltre al confronto temporale, il notebook verifica anche che gli
algoritmi restituiscano gli stessi insiemi di itemset, quando il
confronto è applicabile, attraverso confronti tra `set`:

``` python
cla_set == apriori_set == fpgrowth_set
```

## Riferimento

L'implementazione di Cellular Learning Automata è basata sul metodo
presentato nel paper:

> Sohrabi, Roshani --- *Frequent itemset mining using cellular learning
> automata*

Il notebook cerca di seguire le fasi descritte nell'articolo per
l'implementazione del metodo CLA.
