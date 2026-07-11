# Tetris (C++ / ncirses)

Implementazione del classico gioco **Tetris** in C++, con interfaccia realizzata tramite la libreria **ncurses**. Progetto sviluppato come lavoro di gruppo per il corso di Programmazione — A.A. 2023/2024, Università di Bologna.

---

Progetto sviluppato da:
- **Gaia Righi:** 
- **Lucia Parisi**
- **Maria Paola Klein Serini** 

---

## Descrizione del progetto

Il gioco riproduce le meccaniche classiche di Tetris: i tetramini cadono dall'alto del campo di gioco, il giocatore può muoverli e ruotarli, e al completamento di una riga questa viene rimossa aumentando il punteggio. Alla fine di ogni partita il punteggio viene salvato in una classifica persistente su file.

## Funzionalità

- Rendering del campo di gioco, del pezzo corrente e del punteggio tramite `ncurses`
- Movimento laterale, rotazione (oraria/antioraria) e caduta accelerata dei tetramini
- Rilevamento delle collisioni con i bordi del campo e con i pezzi già piazzati
- Rotazione "atomica": se la rotazione genera una collisione viene annullata e il pezzo torna alla configurazione precedente
- Rimozione automatica delle righe complete, con conseguente discesa delle righe superiori
- Classifica dei punteggi persistente tra sessioni di gioco (salvata su file di testo)
- Menu iniziale navigabile da tastiera (Gioca / Classifica / Esci)


## Architettura e scelte implementative

Il progetto è organizzato attorno a quattro componenti principali:

### `Tetramini` (`Tetramini.hpp` / `.cpp`)
Rappresenta un singolo tetramino. Ogni istanza gestisce:
- la **forma** (quadrato, I, L, J, Z, S, T), distinta tramite costanti `#define`
- il **colore**, assegnato nel costruttore in base alla forma
- una **matrice 4x4** che codifica la disposizione dei blocchi, usata sia per il disegno sia per le rotazioni
- i metodi di movimento (`sposta_sinistra`, `sposta_destra`, `scendi`) e rotazione (`ruota_senso_orario`, `ruota_senso_antiorario`)

### `Gioco` (`gioco.hpp` / `.cpp`)
È il motore del gioco: mantiene lo stato del campo (matrice booleana `campo` + matrice `colori`), il tetramino corrente, il punteggio e il flag di game over. Si occupa di:
- gestire l'input utente e il loop principale (`esegui`)
- controllare le collisioni (`controllaCollisioni`) contro bordi e pezzi già piazzati
- bloccare il tetramino a fine caduta (`bloccaTetramino`)
- individuare ed eliminare le righe complete (`controllaLinee`)
- disegnare campo e pezzo corrente tramite le finestre `ncurses`

### `classifica` (`classifica.hpp` / `.cpp`)
Gestisce il salvataggio e la visualizzazione dei punteggi:
- i punteggi sono mantenuti in memoria tramite una **lista doppiamente concatenata (bilista)**, definita in `my_list.hpp`/`.cpp`
- ad ogni partita il nuovo punteggio viene inserito in ordine decrescente (`inserisciPunteggio`)
- la classifica viene persistita su file di testo (`file.txt`) e ricaricata all'avvio (`salvaPunteggi`)

**Perché una bilista e non una lista singola o un array?** Nella schermata della classifica, se i punteggi salvati superano lo spazio disponibile a video, è necessario poter scorrere la visualizzazione sia in avanti che all'indietro. Una lista doppiamente concatenata permette di navigare in entrambe le direzioni senza dover ricalcolare o riattraversare l'intera struttura, risultando la struttura dati più adatta al problema.

### `my_list` (`my_list.hpp` / `.cpp`)
Implementazione della bilista generica utilizzata dalla classifica, con le funzioni di inserimento ordinato, stampa e deallocazione della lista.


---

## Istruzioni di Compilazione ed Esecuzione

```bash
mkdir build
cd build
cmake ..
```
Se sei su mac o linux:

```bash
make
./tetris
```

Se sei su windows:
```bash
mingw32-make
tetris.exe
```
