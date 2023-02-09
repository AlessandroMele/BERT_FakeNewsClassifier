# BERT_FakeNewsClassifier
BERT, acronimo di *Bidirectional Encoder Representation From Transformers* è un framework basato su reti neurali per il Natural Language Processing.<br>
Il task su cui è stato addestrato il modello BERT è classificare una notizia scritta in inglese tra *Fake* (classe 1) e *Real* (classe 0).<br>

# Pre-processing dei dati
Il dataset usato per l'addestramento del modello è disponibile al seguente <a href="https://www.kaggle.com/competitions/fake-news/data?select=train.csv">link</a>.<br>

Dopo aver esplorato il dataset, sono state eliminate le colonne non significative ai fini dell’analisi, ossia *id*, *title* e a*uthor*.<br>
Successivamente, si è osservata la presenza di valori nulli e duplicati della colonna *text*, perciò si provveduto all’eliminazione delle stesse.<br>
Infine, il campo *text* è stato filtrato rimuovendo hyperlinks, emoji, stopwords e convertendo in caratteri minuscoli.<br>
Di seguito si illustra la struttura del file successivamente alla fase di pre-processing.
<br><img src="images/df_originale.png" width=40% height=40%><br>
<br><img src="images/df_train_post_processing.png" width=40% height=40%><br>

# Parametri e addestramento del modello
Il modello è stato addestrato suddividendo il dataset di partenza in:
- training, contiene l’80% dei dati e rappresenta l’input per l’addestramento del modello;
- validation, contiene il restante 20% e rappresenta l’input per la fase di validazione del modello.<br>
I dati vengono scelti in maniera casuale dal dataset di partenza.<br>

Il modello di partenza, *BertForSequenceClassification*, su cui è stato successivamente effettuato il fine-tuning, è composto da:
- *bert-base-uncased*, ovvero il modello BERT composto da 12 layer, 768 nodi nascosti, 12 attention heads e 110 milioni di parametri;
- a valle di *bert-base-uncased*, uno strato di classificazione a due classi.
Il tokenizer utilizzato è *BertTokenizer*, anch’esso basato su *bert-base-uncased*.

Per l’addestramento del modello si è deciso di impostare i parametri nel seguente modo:
- dimensione dei *batch* pari a 16;
- numero di *epoche* di addestramento pari a 4;
- ottimizzatore: *Adam*, impostando il learning rate pari a 2 ∗ 10−5 ed epsilon pari a 1 ∗ 10−8;

Si osserva, dall’andamento della *loss* di training e validation, che il numero ottimale di epoche di addestramento è pari a 1.<br>
<br><img src="images/loss.png" width=40% height=40%><br>

# Metriche di valutazione
Le seguenti metriche di valutazione sono state applicate sul dataset di test, disponibile al seguente <a href="https://www.kaggle.com/datasets/nopdev/real-and-fake-news-dataset">link</a>.<br>
Come nel pre-processing dei dati di training, anche qui si è provveduto a filtrare il campo *text* rimuovendo hyperlinks, emoji, stopwords e convertendo in caratteri minuscoli.<br>
Si è inoltre osservato che la colonna *label* era etichettata con testo (*FAKE* o *REAL*) anziché valori, perciò si è provveduto a convertirli con una funzione di mapping che assegni il valore 1 alle notizie *fake*, e 0 a quelle *real*.<br>
Di seguito le metriche:
- Accuracy: 0.84;
- Precision: 0.76;
- Recall: 0.99;
- F1 Score: 0.86.

Matrice di confusione:
<br><img src="images/confusion_matrix.png" width=40% height=40%><br>

Curva ROC e AUC:
<br><img src="images/roc_auc_curve.png" width=40% height=40%><br>
