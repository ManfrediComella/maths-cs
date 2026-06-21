# Approssimazione universale, classificazione e teoria della misura

## Guida ragionata al risultato di Cybenko

Questo documento raccoglie e integra le spiegazioni sviluppate nella conversazione sul teorema di approssimazione universale e sulla sua estensione ai problemi di classificazione. L'obiettivo è intro...

---

# 1. Il risultato teorico di riferimento

Il primo risultato è il **Teorema di approssimazione universale** (Universal Approximation Theorem), nella formulazione resa celebre da George Cybenko nel 1989.

In forma intuitiva, esso afferma che una rete feed-forward con:

- un solo strato nascosto;
- un numero sufficientemente grande di neuroni;
- una funzione di attivazione sigmoidale continua;

può approssimare con precisione arbitraria qualsiasi funzione continua definita su un insieme compatto di $\mathbb R^n$.

Non è necessario che la funzione da approssimare sia differenziabile: la continuità è sufficiente.

Nello stesso articolo, Cybenko presenta anche un risultato per le **funzioni di decisione**, cioè funzioni che associano a ogni input una classe. Poiché una funzione di classificazione è normalment...

---

# 2. Il problema di classificazione

## 2.1 Dominio degli input

Fissiamo un numero naturale $n$, che rappresenta il numero di caratteristiche di ogni input.

Indichiamo con

$$I_n=[0,1]^n$$

il cubo unitario in $n$ dimensioni.

Per esempio:

- se $n=1$, allora $I_1=[0,1]$;
- se $n=2$, allora $I_2=[0,1]\times[0,1]$, un quadrato;
- se $n=3$, allora $I_3=[0,1]^3$, un cubo.

Un input è quindi un vettore

$$x=(x_1,\ldots,x_n)\in I_n.$$ 

## 2.2 Classi e regioni decisionali

Supponiamo di avere $k$ classi, numerate:

$$1,2,\ldots,k.$$ 

Il dominio $I_n$ viene suddiviso in $k$ regioni:

$$P_1,P_2,\ldots,P_k.$$ 

La regione $P_j$ contiene gli input appartenenti alla classe $j$.

Queste regioni formano una **partizione** quando:

1. sono a due a due disgiunte:

   $$P_i\cap P_j=\varnothing \qquad\text{se }i\neq j;$$

2. la loro unione è tutto il dominio:

   $$P_1\cup\cdots\cup P_k=I_n.$$ 

Definiamo la funzione di classificazione vera:

$$f:I_n\longrightarrow\{1,\ldots,k\}$$

ponendo

$$f(x)=j \quad\text{quando }x\in P_j.$$ 

Questa è la funzione che vogliamo rappresentare o approssimare con una rete neurale.

---

# 3. Che cosa restituisce la rete neurale?

La rete considerata produce un singolo numero reale:

$$G(x)\in\mathbb R.$$ 

Nella forma usata da Cybenko:

$$G(x)=\sum_{r=1}^{N}\alpha_r\,\sigma\!\left(w_r^\top x+\theta_r\right).$$ 

I simboli indicano:

- $N$: numero di neuroni nello strato nascosto;
- $r$: indice di un neurone nascosto;
- $w_r\in\mathbb R^n$: vettore dei pesi del neurone $r$;
- $w_r^\top x$: prodotto scalare tra $w_r$ e l'input $x$;
- $\theta_r\in\mathbb R$: bias del neurone;
- $\sigma:\mathbb R\to\mathbb R$: funzione di attivazione sigmoidale;
- $\alpha_r\in\mathbb R$: peso usato per combinare l'uscita del neurone $r$;
- $G(x)$: uscita reale complessiva della rete.

La rete non produce necessariamente un numero intero. Se la classe corretta è $2$, potrebbe produrre, per esempio,

$$G(x)=1.83.$$ 

È quindi necessario distinguere tra:

- l'**uscita numerica** $G(x)$;
- la **classe predetta** dalla rete.

---

# 4. Dall'uscita reale alla classe predetta

Definiamo una regola di decodifica che assegna a $G(x)$ l'etichetta intera più vicina:

$$\widehat{f}(x)
=
\arg\min_{c\in\{1,\ldots,k\}}
\lvert G(x)-c\rvert.
$$ 

Qui:

- $c$ rappresenta una possibile etichetta di classe;
- $\lvert G(x)-c\rvert$ è la distanza sulla retta reale tra l'uscita della rete e l'etichetta $c$;
- $\widehat{f}(x)$ è la classe la cui etichetta è più vicina a $G(x)$.

Il simbolo $\arg\min$ significa: scegli il valore di $c$ per cui la quantità indicata è minima.

## 4.1 Perché un errore inferiore a $1/2$ è sufficiente?

Fissiamo un particolare input $x$.

Indichiamo con

$$j=f(x)$$

la sua etichetta corretta.

Supponiamo che la rete soddisfi

$$|G(x)-j|<\tfrac12.$$ 

Introduciamo ora un'altra etichetta:

$$\ell\in\{1,\ldots,k\},
\qquad \ell\neq j.$$ 

La lettera $\ell$ rappresenta quindi **una qualunque classe concorrente diversa dalla classe corretta $j$**.

Poiché $j$ e $\ell$ sono interi distinti,

$$|j-\ell|\geq1.$$ 

Per esempio:

- se $j=2$ e $\ell=3$, la distanza è $1$;
- se $j=2$ e $\ell=5$, la distanza è $3$.

## 4.2 Il passaggio tramite la disuguaglianza triangolare

La disuguaglianza triangolare ordinaria afferma che, per due numeri reali $a$ e $b$,

$$|a+b|\leq |a|+|b|.$$ 

Scriviamo la differenza tra le due etichette $j$ e $\ell$ passando per il valore prodotto dalla rete $G(x)$:

$$j-\ell=(j-G(x))+(G(x)-\ell).$$ 

Applicando la disuguaglianza triangolare:

$$|j-\ell|
=
|(j-G(x))+(G(x)-\ell)|
\leq
|j-G(x)|+|G(x)-\ell|.
$$ 

Sottraiamo $|j-G(x)|$ da entrambi i membri:

$$|j-\ell|-|j-G(x)|
\leq
|G(x)-\ell|.
$$ 

Poiché il valore assoluto non cambia invertendo l'ordine della sottrazione,

$$|j-G(x)|=|G(x)-j|,$$ 

otteniamo

$$\boxed{
\lvert G(x)-\ell\rvert
\geq
|j-\ell|-\lvert G(x)-j\rvert
}.$$ 

Questa è una forma della **disuguaglianza triangolare inversa**.

L'interpretazione geometrica è semplice: la distanza tra $G(x)$ e l'etichetta concorrente $\ell$ non può essere inferiore alla distanza tra $j$ e $\ell$, meno lo spostamento di $G(x)$ risp...

Dato che

$$|j-\ell|\geq1$$

e

$$\lvert G(x)-j\rvert<\tfrac12,$$ 

segue che

$$\lvert G(x)-\ell\rvert>\tfrac12.$$ 

Contemporaneamente,

$$\lvert G(x)-j\rvert<\tfrac12.$$ 

Quindi $G(x)$ è più vicino a $j$ che a qualunque altra etichetta $\ell$, e pertanto

$$\widehat{f}(x)=j=f(x).$$ 

La conclusione non è

$$G(x)=f(x).$$ 

La conclusione corretta è

$$\lvert G(x)-f(x)\rvert<\tfrac12
\quad\Longrightarrow\quad
\widehat{f}(x)=f(x),$$

**dopo aver definito la decodifica all'intero più vicino**.

Il valore assoluto è qui la distanza euclidea nello spazio di uscita unidimensionale $\mathbb R$, non una distanza nello spazio degli input $\mathbb R^n$.

---

# 5. Insiemi aperti, chiusi e limitati

## 5.1 Intorno di un punto

Sulla retta reale, un intorno aperto di un punto $x$ è un intervallo

$$(x-r,x+r),
\qquad r>0.$$ 

In $\mathbb R^n$, l'analogo è una palla aperta:

$$B(x,r)
=
\{y\in\mathbb R^n:\|y-x\|_2<r\}.$$ 

La norma euclidea è

$$\|y-x\|_2
=
\sqrt{\sum_{i=1}^n(y_i-x_i)^2}.$$ 

## 5.2 Insieme aperto

Un insieme $U\subseteq\mathbb R^n$ è aperto quando, per ogni punto $x\in U$, esiste un raggio $r>0$ tale che

$$B(x,r)\subseteq U.$$ 

Intuitivamente, ogni punto di $U$ possiede un piccolo spazio attorno a sé interamente contenuto in $U$.

Per esempio,

$$(0,1)$$

è aperto in $\mathbb R$.

## 5.3 Insieme chiuso

Un insieme $F\subseteq\mathbb R^n$ è chiuso quando il suo complemento

$$\mathbb R^n\setminus F$$

è aperto.

Equivalentemente, un insieme chiuso contiene tutti i propri punti limite.

Per esempio,

$$[0,1]$$

è chiuso perché contiene anche i propri estremi $0$ e $1$.

## 5.4 Insieme limitato

Un insieme $K\subseteq\mathbb R^n$ è limitato quando esiste una palla di raggio finito che lo contiene interamente.

Formalmente, esistono un punto $x_0\in\mathbb R^n$ e un numero $R>0$ tali che

$$K\subseteq B(x_0,R).$$

---

# 6. Ricoprimenti e sottoricoprimenti

## 6.1 Ricoprimento aperto

Sia $K\subseteq\mathbb R^n$.

Un **ricoprimento aperto** di $K$ è una famiglia di insiemi aperti

$$\{U_i\}_{i\in J}$$

tale che

$$K\subseteq\bigcup_{i\in J}U_i.$$ 

Il simbolo $J$ è un insieme di indici, finito o infinito.

La condizione significa che ogni punto di $K$ appartiene ad almeno uno degli insiemi $U_i$.

Per esempio,

$$U_1=(-1,2)$$

costituisce da solo un ricoprimento aperto di $[0,1]$, perché

$$[0,1]\subseteq(-1,2).$$ 

## 6.2 Sottoricoprimento

Un sottoricoprimento si ottiene scegliendo alcuni insiemi dalla famiglia originale, conservando però la proprietà di coprire tutto $K$.

Se

$$\{U_i\}_{i\in J}$$

è un ricoprimento e scegliamo un sottoinsieme di indici $J'\subseteq J$, allora

$$\{U_i\}_{i\in J'}$$

è un sottoricoprimento se

$$K\subseteq\bigcup_{i\in J'}U_i.$$ 

È un **sottoricoprimento finito** quando $J'$ contiene un numero finito di indici.

---

# 7. Compattezza

Un insieme $K\subseteq\mathbb R^n$ è compatto quando:

> da ogni ricoprimento aperto di $K$ è possibile estrarre un sottoricoprimento finito.

La parola fondamentale è **ogni**.

Non basta che esista un particolare ricoprimento finito. Per dimostrare che un insieme non è compatto basta trovare un solo ricoprimento aperto privo di sottoricoprimenti finiti.

## 7.1 Perché $(0,1)$ non è compatto?

Consideriamo

$$K=(0,1).$$

Per ogni intero $m\geq2$, definiamo

$$U_m=\left(\frac1m,1\right).$$ 

Ogni $U_m$ è aperto.

La famiglia

$$\{U_m:m=2,3,4,\ldots\}$$

copre tutto $(0,1)$. Infatti, preso un qualunque $x\in(0,1)$, possiamo scegliere $m$ abbastanza grande da avere

$$\frac1m<x,$$ 

quindi $x\in U_m$.

Pertanto

$$ (0,1)\subseteq\bigcup_{m=2}^{\infty}U_m.$$ 

Supponiamo ora di scegliere soltanto un numero finito di questi insiemi:

$$U_{m_1},U_{m_2},\ldots,U_{m_q}.$$ 

Indichiamo con

$$M=\max\{m_1,\ldots,m_q\}$$

il più grande indice scelto.

Poiché gli insiemi sono annidati,

$$U_{m_1}\cup\cdots\cup U_{m_q}
=
U_M
=
\left(\frac1M,1\right).$$ 

Questo insieme non contiene i punti di $(0,1)$ minori o uguali a $1/M$. Per esempio,

$$\frac{1}{2M}\in(0,1)$$

ma

$$\frac{1}{2M}\notin U_M.$$ 

Quindi nessuna selezione finita copre tutto $(0,1)$. Abbiamo trovato un ricoprimento aperto senza sottoricoprimento finito, quindi $(0,1)$ non è compatto.

Il fatto che $(-1,2)$ costituisca da solo un ricoprimento finito non cambia nulla: la compattezza richiede che la proprietà valga per **ogni** ricoprimento aperto.

## 7.2 Teorema di Heine-Borel

In $\mathbb R^n$, vale il teorema:

$$\boxed{
K\text{ è compatto}
\quad\Longleftrightarrow\quad
K\text{ è chiuso e limitato}.
}$$

Il cubo

$$I_n=[0,1]^n$$

è chiuso e limitato, quindi è compatto.

---

# 8. Compattezza tramite successioni

## 8.1 Successione

Una successione in un insieme $K$ è una lista infinita di punti:

$$x_1,x_2,x_3,\ldots,
\qquad x_m\in K.$$ 

Formalmente è una funzione

$$x:\mathbb N\to K.$$ 

## 8.2 Sottosuccessione

Una sottosuccessione si ottiene scegliendo infiniti termini della successione originale, senza modificarne l'ordine.

Scegliamo indici

$$m_1<m_2<m_3<\cdots$$

e consideriamo

$$x_{m_1},x_{m_2},x_{m_3},\ldots.$$ 

## 8.3 Convergenza

Una successione $x_m$ converge a un punto $x$ quando

$$\|x_m-x\|_2\longrightarrow0.$$ 

Più precisamente, per ogni $\varepsilon>0$ esiste un indice $M$ tale che, per ogni $m\geq M$,

$$\|x_m-x\|_2<\varepsilon.$$ 

## 8.4 Caratterizzazione sequenziale della compattezza

In $\mathbb R^n$, un insieme $K$ è compatto se e solo se ogni successione di punti di $K$ possiede una sottosuccessione convergente a un punto ancora appartenente a $K$.

### Esempio in $[0,1]$

Consideriamo

$$x_m=\frac1m.$$ 

Tutti i termini appartengono a $[0,1]$, e

$$x_m\longrightarrow0.$$ 

Il limite $0$ appartiene ancora a $[0,1]$.

### Perché lo stesso esempio mostra che $(0,1)$ non è compatto?

La successione

$$x_m=\frac1m$$

appartiene interamente a $(0,1)$, ma converge a

$$0\notin(0,1).$$ 

Qualunque sua sottosuccessione converge ancora a $0$, perché gli indici selezionati continuano a crescere verso infinito. Quindi questa successione non possiede una sottosuccessione convergent[...]