# Analisi Predittiva dell'Abbandono Aziendale
### Employee Attrition

> **Corso:** Introduzione alla data science e al pensiero computazionale · Anno Accademico 2025/2026  
> **Università:** Università di Bologna

---

## Indice

- [Descrizione del Progetto](#descrizione-del-progetto)
- [Dataset](#dataset)
- [Obiettivi](#obiettivi)
- [Struttura del Repository](#struttura-del-repository)
- [Pipeline Metodologica](#pipeline-metodologica)
- [Modelli Utilizzati](#modelli-utilizzati)
- [Risultati](#risultati)
- [Istruzioni per l'Esecuzione](#istruzioni-per-lesecuzione)
- [Membri del Gruppo](#membri-del-gruppo)
- [Dichiarazione sull'Utilizzo dell'Intelligenza Artificiale](#dichiarazione-sullutilizzo-dellintelligenza-artificiale)

---

## Descrizione del Progetto

Questo progetto analizza il fenomeno dell'**employee attrition** — l'abbandono volontario dei dipendenti — con l'obiettivo di comprendere quali fattori ne determinano la probabilità e costruire modelli predittivi in grado di identificare tempestivamente i dipendenti a rischio.

Il lavoro segue un ciclo di vita strutturato di Data Science, che comprende la comprensione del dataset, l'analisi esplorativa (EDA), il preprocessing, la modellazione con algoritmi di Machine Learning e la valutazione critica dei risultati.

L'obiettivo finale è duplice:
- fornire uno **strumento predittivo** per identificare i dipendenti a rischio di abbandono;
- ottenere **insight strategici** a supporto delle decisioni HR.

---

## Dataset

| Caratteristica | Valore |
| --- | --- |
| **Numero di osservazioni** | 1.470 dipendenti |
| **Numero di variabili** | 35 (34 predittori + 1 target) |
| **Variabile target** | `Attrition` (Yes / No → 1 / 0) |
| **Valori mancanti** | Nessuno |
| **Sbilanciamento classi** | 83,88% No (resta) · 16,12% Yes (esce) |

### Categorie di Feature

Le 34 feature predittive sono suddivise in tre macro-categorie:

- **Demografiche e Personali:** `Age`, `Gender`, `MaritalStatus`, `DistanceFromHome`
- **Professionali ed Economiche:** `JobRole`, `MonthlyIncome`, `YearsAtCompany`, `TotalWorkingYears`, `OverTime`
- **Psicometriche (scala Likert 1–4):** `JobSatisfaction`, `EnvironmentSatisfaction`, `WorkLifeBalance`, `JobInvolvement`, `RelationshipSatisfaction`

### Operazioni di Data Cleaning

Sono state rimosse le seguenti variabili prive di contenuto informativo:

| Variabile rimossa | Motivazione |
| --- | --- |
| `Over18` | Valore costante `'Y'` per tutti i record (varianza nulla) |
| `EmployeeCount` | Valore costante `1` per tutti i record |
| `StandardHours` | Valore costante `80` per tutti i record |
| `EmployeeNumber` | Identificativo numerico univoco, nessun potere predittivo |

---

## Obiettivi

1. Analizzare statisticamente la distribuzione della variabile target e le sue relazioni con le feature.
2. Formulare e verificare **8 ipotesi di ricerca** sui fattori associati all'abbandono.
3. Condurre un'**analisi esplorativa multivariata** (EDA) per identificare i principali driver dell'attrition.
4. Addestrare e confrontare **3 modelli di classificazione** sul problema predittivo.
5. Valutare i modelli con metriche robuste rispetto allo sbilanciamento delle classi (Recall, F1-Score).
6. Derivare **implicazioni strategiche** per la gestione delle Risorse Umane.

---

## Struttura del Repository

```
employee-attrition/
│
├── data/
│   ├── raw/
│   │   └── Employee_Attrition.csv           # Dataset originale
│   ├── encoded/
│   │   └── employee_attrition_encoded.csv   # Dataset dopo encoding
│   └── documentation/
│       └── Employee_Attrition_Description.csv  # Descrizione delle variabili
│
├── figures/                                 # Grafici e visualizzazioni esportati
│
├── notebooks/
│   └── Employee_Attrition_Analisi e visualizzazione.ipynb  # Notebook principale
│
├── report/                                  # Report scientifico finale (LaTeX/PDF)
│
└── README.md
```

---

## Pipeline Metodologica

Il progetto è stato sviluppato seguendo sei macro-fasi operative:

1. **Setup del Progetto** — Configurazione dell'ambiente e del repository GitHub per il tracciamento cooperativo del codice.
2. **Descrizione e Comprensione del Dataset** — Analisi preliminare della struttura, controllo della qualità e formulazione delle ipotesi di ricerca.
3. **Analisi Esplorativa (EDA)** — Studio visuale e quantitativo delle relazioni tra feature e variabile target; identificazione del class imbalance.
4. **Modellazione** — Preprocessing (encoding, scaling, train/test split) e addestramento di 3 algoritmi di classificazione.
5. **Valutazione dei Risultati** — Confronto tramite accuracy, precision, recall, F1-score e matrici di confusione.
6. **Report Scientifico** — Redazione formale in LaTeX.

---

## Modelli Utilizzati

Sono stati addestrati e confrontati tre classificatori con approcci matematici differenti:

| Modello | Tipo | Note |
| --- | --- | --- |
| **Regressione Logistica** | Lineare, parametrico | Addestrato su dati scalati |
| **K-Nearest Neighbors (K-NN)** | Non lineare, instance-based | Addestrato su dati scalati; basato su distanze |
| **Random Forest** | Non lineare, ensemble | Addestrato su dati non scalati; `n_estimators=200`, `random_state=42` |

---

## Risultati

### Confronto delle Performance sulla Classe Minoritaria (Attrition = 1)

| Modello | Accuracy | Precision (Esce) | Recall (Esce) | F1-Score (Esce) |
| --- | :---: | :---: | :---: | :---: |
| **Regressione Logistica** | 0.880 | 0.725 | 0.408 | **0.523** |
| K-Nearest Neighbors | 0.839 | 0.500 | 0.070 | 0.123 |
| Random Forest | 0.834 | 0.455 | 0.141 | 0.215 |

> **Nota:** l'accuracy da sola è fuorviante in presenza di classi sbilanciate. La metrica più informativa per questo problema è l'**F1-Score sulla classe "Esce"**.

### Modello Migliore: Regressione Logistica

La Regressione Logistica ottiene il miglior F1-Score (0.523), la precision più alta (0.725) e il recall più alto (0.408) sulla classe di interesse. Il modello più semplice e interpretabile si dimostra anche il più efficace, confermando che la complessità algoritmica non garantisce prestazioni superiori in contesti di forte sbilanciamento senza tecniche di bilanciamento preventivo.

### Principali Fattori di Rischio Identificati

- **Lavoro Straordinario (`OverTime`):** tasso di attrition ~31% (correlazione r ≈ 0.25)
- **Basso Reddito (`MonthlyIncome`):** concentrazione di abbandoni sotto i $5.000/mese
- **Stato Civile Single (`MaritalStatus`):** tasso di attrition ~25.5% (r ≈ 0.18)
- **Giovane Età (`Age`):** maggiore incidenza under 35
- **Trasferte Frequenti (`BusinessTravel`):** tasso ~24.9% per *Travel Frequently*
- **Bassa Soddisfazione (`JobSatisfaction`):** relazione inversa con l'attrition
- **Stagnazione Carriera (`PromotionDelayRatio`):** effetto amplificato in combinazione con altri fattori

---

## Istruzioni per l'Esecuzione

### Prerequisiti

Assicurarsi di avere installato:
- Python 3.8+
- Jupyter Notebook o JupyterLab

### 1. Clonare il repository

```bash
git clone https://github.com/evelinenikokovacs/Employee_Attrition
cd employee-attrition
```

### 2. Installare le dipendenze

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

Oppure, se disponibile un file `requirements.txt`:

```bash
pip install -r requirements.txt
```

### 3. Avviare Jupyter

```bash
jupyter notebook
```

### 4. Eseguire il notebook

Aprire il file:

```
notebooks/Employee_Attrition_Analisi e visualizzazione.ipynb
```

ed eseguire le celle in ordine sequenziale (**Kernel → Restart & Run All**).

> **Nota:** il dataset grezzo si trova in `data/raw/Employee_Attrition.csv`. Il notebook genera automaticamente il dataset encoded in `data/encoded/`.

---

## Membri del Gruppo

| Nome | Matricola | Contributo |
| --- | :---: | --- |
| **Kovacs Evelin Eniko** | 1216140 | Analisi e visualizzazione dei dati (EDA); creazione e gestione del repository GitHub; scrittura del README |
| **Guarnieri Federico** | 1223187 | Preprocessing, modellazione e valutazione dei modelli di Machine Learning |
| **Amoroso Giovanni** | 1238995 | Redazione del report scientifico finale in LaTeX |

---

## Dichiarazione sull'Utilizzo dell'Intelligenza Artificiale

In conformità con le linee guida accademiche sulla trasparenza nell'uso di strumenti di AI generativa, i membri del gruppo dichiarano quanto segue:

| Membro | Strumento Utilizzato | Data | Ambito di Utilizzo |
| --- | --- | :---: | --- |
| **Amoroso Giovanni** | Strumento AI (non specificato) | A.A. 2025/2026 | Supporto nella formattazione e generazione di alcune **tabelle** all'interno del report scientifico finale in LaTeX |
| **Kovacs Evelin Eniko** | ChatGPT | 23 giugno 2026 | Ottimizzazione di alcune porzioni di **codice Python** per la creazione delle visualizzazioni nel notebook |
| **Kovacs Evelin Eniko** | Claude - Sonnet 4.6 | 29 giugno 2026 | Ottimizzazione di alcune porzioni di testo in README.md |
| **Guarnieri Federico** | — | — | Nessun utilizzo dichiarato |

> Tutto il codice generato con supporto AI è stato **revisionato, compreso e validato** dai rispettivi autori prima dell'integrazione nel progetto. La responsabilità dei contenuti rimane interamente in capo ai membri del gruppo.

---

*Progetto realizzato per il corso di Introduzione alla data science e al pensiero computazionale · Anno Accademico 2025/2026*
