# Cellular Learning Automata vs Apriori

Implementazione e confronto tra due algoritmi per il *frequent itemset mining*:

- **Cellular Learning Automata (CLA)**, basato sul metodo proposto nel paper *"Frequent itemset mining using cellular learning automata"* di Sohrabi e Roshani (2017);
- **Apriori**, algoritmo usato come baseline di confronto.

Il notebook implementa entrambi gli algoritmi in Python e ne confronta i tempi di esecuzione sul dataset **Mushroom** (UCI Machine Learning Repository).

## Struttura del notebook

### Parte 1 — Cellular Learning Automata
Implementazione:

- `find_1_itemsets`: individua gli itemset frequenti di dimensione 1 (sezione 3.1 del paper).
- `prune_and_compress_dataset`: elimina gli item non frequenti e comprime le transazioni duplicate (sezione 3.2).
- `run_cellular_automata` / `update_proximity_list`: costruisce le celle e le rispettive *proximity list* (sezione 3.3).
- `prune_cells` / `prune_proximity_list` / `collect_itemsets` / `scan_cells`: effettua il pruning delle proximity list e la scansione delle celle per estrarre gli itemset frequenti finali (sezione 3.4).
- `cla_mining`: funzione che unisce l'intera pipeline dell'algoritmo CLA.

### Parte 2 — Apriori
Implementazione:

- `find_1_itemsets_apriori`: itemset frequenti di dimensione 1.
- `generate_candidate`: genera i candidati di lunghezza *k* a partire dai frequenti di lunghezza *k-1*.
- `verify_antimonotonicity`: verifica che tutti i sottoinsiemi di un candidato siano frequenti.
- `count_candidates_support` / `filter_frequent_itemsets`: conteggio del supporto e filtro dei candidati.
- `apriori_mining`: funzione che unisce l'intera pipeline dell'algoritmo Apriori.

### Parte 3 — Confronto dei tempi di esecuzione
- `measure_execution_time`: misura il tempo di esecuzione di una funzione di ciascun algoritmo.
- `run_benchmark`: esegue CLA e Apriori a diversi valori di *minsup*, confrontando tempi e insiemi di itemset trovati.
- `plot_benchmark_results`: genera un grafico dei tempi di esecuzione al variare di *minsup*.

## Dataset

Il notebook utilizza il dataset **Mushroom** (`agaricus-lepiota.data`), lo stesso usato nella sezione *Experimental results* del paper di riferimento. Il file va posizionato in:

```
/content/agaricus-lepiota.data
```

(percorso predefinito per l'esecuzione su Google Colab). Il dataset è disponibile pubblicamente sull'[UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/73/mushroom).

## Requisiti

```
pandas
matplotlib
```

Le librerie `time` e `itertools` fanno parte della standard library di Python.

## Utilizzo

1. Aprire il notebook `CLAvsApriori.ipynb` su Google Colab (o Jupyter).
2. Caricare il file `agaricus-lepiota.data` nel percorso indicato.
3. Eseguire le celle in ordine: prima le implementazioni di CLA e Apriori, poi la sezione di benchmark.
4. Il benchmark stampa, per ogni valore di *minsup*, i tempi di esecuzione e verifica che i due algoritmi trovino lo stesso insieme di itemset frequenti; al termine viene mostrato un grafico comparativo dei tempi.

## Riferimenti

Sohrabi, M. K., & Roshani, S. (2017). *Frequent itemset mining using cellular learning automata*. Computers in Human Behavior.
