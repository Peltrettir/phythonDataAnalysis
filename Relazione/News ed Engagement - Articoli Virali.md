# News ed engagement - Articoli Virali

### SEZIONI

1. [Introduzione](#introduzione)
2. [Preparazione dataset](#preparazione)
   1. [Scrematura](#preparazione.1)
   2. [Gestione dei null](#preparazione.2)
3. [Analisi dei dati](#analisi)
   1. [Distribuzione interazioni](#analisi.1)
   2. [Interazioni per fonte](#analisi.2)
   3. [Normalizzazione interazioni](#analisi.3)

## <a name="introduzione"/> Introduzione

Questa relazione ha come obbiettivo l'analisi di un dataset arbitrario scelto tra quelli disponibili su [Kaggle](https://www.kaggle.com), rispettando alcuni vincoli, tra cui:

 * Utilizzo di libreriaìe pandas, numpy
 * Utilizzo d Boolean/Fancy indexing
 * Presenza di grafici
 * Lettura e scrittura file
  
Individuato il dataset [Internet news data with user engagement](https://www.kaggle.com/szymonjanowski/internet-articles-data-with-users-engagement) ci si è posta la seguente domanda:

>Qual'è, se esiste, l'orario migliore per pubblicare un articolo di giornale?

Per rispondervi si è considerato il rapporto tra il numero di interazioni dei lettori e l'orario di pubblicazione degli articoli.

## <a name="preparazione"/> Preparazione dataset

#### notebook: [Preparazione Dataset](http://localhost:8888/notebooks/work/Preparazione%20Dataset.ipynb?token=123qwe)

Si inspeziona il dataset con lo scopo di alleggerirlo il più possibile. Vengono inoltre gestiti i pochi casi di dati mancanti.

### <a name="preparazione.1"> 1. Scrematura

Già da un primo squardo si nota che molte delle colonne non ci saranno d'aiuto, essendo noi interessati alla relazione tra l'orario di pubblicazione e la risposta dei lettori. Di tutte le informazioni relative al contenuto degli articoli viene mantenuto solo il titolo, fondamentale per dare contesto al numero di interazini.\
Le colonne *source_id* e *source_name* sono ridondanti. Viene quindi creata una tabella di dettaglio che mappa l'idenrificativo della fonte al nome completo, questa tabella conterrà tutte le informazioni delle testate giornalistiche.\ Esaminandola si identifica una *source* fittizia, che viene eliminata.

Alla tabella di dettaglio viene aggiunto il numero di articoli presenti per ogni *source*. In questo modo è possibile valutare quanto la singola testata giornalistica sia rappresentata nel dataset.\
ESPN, con 82 articoli, risulta essere è significativamente meno presente rispetto alle altre e potrebbe essere eliminata. Si sceglie però di tenerla per osservarne l'engagement.

### <a name="preparazione.2"> 2. Gestione dei null

Si traccia una heatmap che mette in evidenza la distribuzione dei valori nulli nel dataset. Sono pochi e fortunatamente appartengono tutti agli stessi tre articoli, che vengono perciò scartati.

## <a name="analisi"> Analisi dei dati

#### notebook: [Analisi Dati 1](http://localhost:8888/notebooks/work/Analisi%20Dati%201.ipynb?token=123qwe)

### <a name="analisi.1">  Distribuzione interazioni

La distribuzione delle interazioni su tutti gli articoli, non divisi per testata, risulta pesantemente spostata verso lo zero, con valori massimi piottosto elevati. Ciò significa che un numero ridotto di articoli conta la vasta maggioranza delle interazioni. Potremmo definire questi articoli "virali".

Risulta inoltre evidente che la colonna *comment_plugin* presenta un numero di interazioni molto ridotto rispetto alle altre. Per approfondire questa anomalia si procede al conteggio degli articoli con engagement non nullo, divisi per tipo di interazione.\
Con solo 50 aricoli che presentano un'interazione ed un massimo di 15 interazioni per articolo si rafforza *comment_plugin* non sia un tipo di interazione che non rappresenta fedelmente l'engagement dei lettori, e che sia perciò da scartare.

Studiando la distribuzione delle interazioni divise per fonte, l'ipotesi di cui sopra ha la sua conferma, in quanto la maggior parte delle testate non registrano interazioni per *comment_plugin*, che viene quindi scartata.\
In questa fase si nota anche che la testata ESPN, oltre ad essere sottorappresentata per numero di articoli, ha engagement pari a zero. Si decide di scartarla.

### <a name="analisi.2"> Interazioni per fonte

Si realizza quindi una serie di istogrammi per rappresentare la distribuzione delle interazioni divise per fonte e per tipo, mettendo in evidenza l'articolo con engagement maggiore.\
Guardando questi grafici si ipotizza che la grande differenza di interazioni tra l'articolo migliore ed il resto degli articoli sia legata ad una notizia o avvenimento di particolare interesse.\
<a name="bias">Se così fosse introdurremmo un bias nella nostra indagine analizzando questi articoli insieme agli altri in quanto è ragionevole assumere che questi siano pubblicati "il prima possibile" rispetto all'avvenimento in questione, il che influenzerebbe il rapporto orario-engagement.
 
Viene quindi stilata una lista dei titoli ed orari di pubblicazione degli articoli migliori divisi per ogni fonte e tipo di interazione.\
Da questa lista si scopre che gli articoli migliori coprono notizie differenti gli uni dagli altri. Non solo, per una stessa testata l'articolo con più interazioni spesso cambia a seconda del tipo di interazione. Si abbandona quindi l'ipotesi di un bias introdotto dagli articoli virali.

#### notebook: [Analisi Dati 2](http://localhost:8888/notebooks/work/Analisi%20Dati%202.ipynb?token=123qwe)

### <a name="analisi.3"> Normalizzazione interazioni


