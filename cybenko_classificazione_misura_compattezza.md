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

$$\widehat f(x)
=
\operatorname*{arg\,min}_{c\in\{1,\ldots,k\}}
|G(x)-c|.
$$ 

Qui:

- $c$ rappresenta una possibile etichetta di classe;
- $|G(x)-c|$ è la distanza sulla retta reale tra l'uscita della rete e l'etichetta $c$;
- $\widehat f(x)$ è la classe la cui etichetta è più vicina a $G(x)$.

Il simbolo $\operatorname{arg\,min}$ significa: scegli il valore di $c$ per cui la quantità indicata è minima.

## 4.1 Perché un errore inferiore a $1/2$ è sufficiente?

Fissiamo un particolare input $x$.

Indichiamo con

$$j=f(x)$$

la sua etichetta corretta.

Supponiamo che la rete soddisfi

$$|G(x)-j|<\frac12.$$ 

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
|G(x)-\ell|
\geq
|j-\ell|-|G(x)-j|
}.$$ 

Questa è una forma della **disuguaglianza triangolare inversa**.

L'interpretazione geometrica è semplice: la distanza tra $G(x)$ e l'etichetta concorrente $\ell$ non può essere inferiore alla distanza tra $j$ e $\ell$, meno lo spostamento di $G(x)$ risp...

Dato che

$$|j-\ell|\geq1$$

e

$$|G(x)-j|<\frac12,$$ 

segue che

$$|G(x)-\ell|>\frac12.$$ 

Contemporaneamente,

$$|G(x)-j|<\frac12.$$ 

Quindi $G(x)$ è più vicino a $j$ che a qualunque altra etichetta $\ell$, e pertanto

$$\widehat f(x)=j=f(x).$$ 

La conclusione non è

$$G(x)=f(x).$$ 

La conclusione corretta è

$$|G(x)-f(x)|<\frac12
\quad\Longrightarrow\quad
\widehat f(x)=f(x),$$

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

---

# 9. La norma uniforme $\|G-f\|_\infty$

Supponiamo che $G$ e $f$ siano due funzioni definite sullo stesso insieme $K$:

$$G,f:K\to\mathbb R.$$ 

Per ogni punto $x\in K$, l'errore è

$$|G(x)-f(x)|.$$ 

La **norma uniforme**, o norma del supremo, è definita come

$$\boxed{
\|G-f\|_\infty
=
\sup_{x\in K}|G(x)-f(x)|.
}$$ 

## 9.1 Che cos'è il supremo?

Il supremo di un insieme di numeri è il più piccolo numero maggiore o uguale a tutti gli elementi dell'insieme.

Per esempio,

$$A=\left\{1-\frac1m:m=1,2,\ldots\right\}$$

contiene

$$0,\frac12,\frac23,\frac34,\ldots$$ 

ma non contiene $1$. Tuttavia,

$$\sup A=1.$$ 

Il supremo viene usato perché il massimo potrebbe non essere raggiunto.

Se il dominio è compatto e la funzione $|G-f|$ è continua, il massimo viene invece raggiunto:

$$\|G-f\|_\infty
=
\max_{x\in K}|G(x)-f(x)|.$$ 

## 9.2 Interpretazione

La quantità

$$\|G-f\|_\infty$$

è il peggiore errore commesso su tutto il dominio.

La condizione

$$\|G-f\|_\infty<\varepsilon$$

significa esattamente

$$|G(x)-f(x)|<\varepsilon
\qquad
\text{per ogni }x\in K.$$ 

---

# 10. Convergenza uniforme e convergenza puntuale

Supponiamo di avere una successione di reti

$$G_1,G_2,G_3,\ldots.$$ 

## 10.1 Convergenza uniforme

Diciamo che $G_m$ converge uniformemente a $f$ quando

$$\|G_m-f\|_\infty\longrightarrow0.$$ 

Espandendo la definizione:

> per ogni $\varepsilon>0$, esiste un indice $M$ tale che, per ogni $m\geq M$ e per ogni $x\in K$,
> 
> $$|G_m(x)-f(x)|<\varepsilon.$$ 

Lo stesso indice $M$ deve funzionare per tutti i punti $x$.

L'ordine dei quantificatori è:

$$\forall\varepsilon>0\;
\exists M\;
\forall m\geq M\;
\forall x\in K.$$ 

## 10.2 Convergenza puntuale

La convergenza puntuale richiede invece che, per ogni punto $x$, l'indice necessario possa dipendere da quel punto.

L'ordine dei quantificatori è:

$$\forall x\in K\;
\forall\varepsilon>0\;
\exists M_x\;
\forall m\geq M_x.$$ 

## 10.3 Esempio: convergenza puntuale ma non uniforme

Consideriamo

$$G_m(x)=x^m,
\qquad x\in[0,1].$$ 

Per ogni $x<1$,

$$x^m\longrightarrow0.$$ 

Per $x=1$,

$$1^m=1.$$ 

Il limite puntuale è

$$f(x)=
\begin{cases}
0,&0\leq x<1,\\
1,&x=1.
\end{cases}$$ 

La funzione limite è discontinua, nonostante tutte le funzioni $G_m$ siano continue. La convergenza non è uniforme.

## 10.4 Perché una funzione discontinua non può essere limite uniforme di funzioni continue?

Vale il teorema:

> il limite uniforme di una successione di funzioni continue è continuo.

Di conseguenza, una funzione discontinua non può essere approssimata da funzioni continue con errore uniforme arbitrariamente piccolo su tutto il dominio.

Può però essere limite puntuale di funzioni continue, come mostra l'esempio $x^m$.

---

# 11. Sigma-algebre e insiemi misurabili

Partiamo da un insieme universo $X$, per esempio

$$X=\mathbb R^n$$

oppure

$$X=I_n=[0,1]^n.$$ 

Una **sigma-algebra** $\Sigma$ su $X$ è una collezione di sottoinsiemi di $X$ con tre proprietà.

## 11.1 Contiene l'insieme vuoto

$$\varnothing\in\Sigma.$$ 

## 11.2 È chiusa rispetto al complemento

Se

$$A\in\Sigma,$$ 

allora anche il complemento relativo all'universo $X$,

$$X\setminus A,$$ 

appartiene a $\Sigma$.

Il complemento dipende dall'universo scelto.

Per esempio, se

$$X=[0,1]$$

 e

$$A=\left[0,\frac12\right],$$ 

allora

$$X\setminus A
=
\left(\frac12,1\right].$$ 

## 11.3 È chiusa rispetto alle unioni numerabili

Se

$$A_1,A_2,A_3,\ldots\in\Sigma,$$ 

allora

$$\bigcup_{m=1}^{\infty}A_m\in\Sigma.$$ 

Da complemento e unione numerabile segue anche la chiusura rispetto alle intersezioni numerabili, tramite le leggi di De Morgan:

$$\bigcap_{m=1}^{\infty}A_m
= 
X\setminus
\left(
\bigcup_{m=1}^{\infty}(X\setminus A_m)
\right).$$ 

Un insieme $A\subseteq X$ è **misurabile rispetto a $\Sigma$** quando

$$A\in\Sigma.$$ 

---

# 12. La sigma-algebra di Borel

Consideriamo

$$X=\mathbb R^n.$$ 

Prendiamo la collezione di tutti gli insiemi aperti di $\mathbb R^n$. Vogliamo costruire la più piccola sigma-algebra che li contenga.

La **sigma-algebra di Borel** è

$$\mathcal B(\mathbb R^n)
=
\sigma(\text{insiemi aperti di }\mathbb R^n).$$ 

La notazione $\sigma(\cdot)$ significa: la più piccola sigma-algebra contenente la collezione indicata.

Più precisamente, si considerano tutte le sigma-algebre che contengono gli aperti e se ne prende l'intersezione.

Non basta applicare complemento e unione una sola volta: le operazioni possono essere ripetute e combinate numerabilmente.

La sigma-algebra di Borel contiene:

- tutti gli aperti;
- tutti i chiusi;
- tutte le unioni numerabili di chiusi;
- tutte le intersezioni numerabili di aperti;
- tutte le combinazioni ottenute ripetendo queste operazioni.

Gli elementi di $\mathcal B(\mathbb R^n)$ sono detti **insiemi boreliani**.

Nel cubo $I_n$, la sigma-algebra boreliana relativa è

$$\mathcal B(I_n)
=
\{B\cap I_n:B\in\mathcal B(\mathbb R^n)\}.$$ 

---

# 13. Misura di Lebesgue

Una misura è una funzione che assegna una dimensione agli insiemi misurabili.

La misura di Lebesgue in $\mathbb R^n$ viene indicata con

$$\lambda_n.$$ 

La lettera $\lambda$ è una convenzione; il pedice $n$ indica la dimensione dello spazio.

Formalmente,

$$\lambda_n:
\mathcal L(\mathbb R^n)
\longrightarrow
[0,+\infty],
$$

ove $\mathcal L(\mathbb R^n)$ è la collezione degli insiemi misurabili secondo Lebesgue.

La misura generalizza:

- la lunghezza per $n=1$;
- l'area per $n=2$;
- il volume per $n=3$;
- l'ipervolume per $n>3$.

## 13.1 Rettangoli

Se

$$R=[a_1,b_1]\times\cdots\times[a_n,b_n],$$ 

allora

$$\lambda_n(R)
=
\prod_{i=1}^n(b_i-a_i).$$ 

Per il cubo unitario,

$$\lambda_n(I_n)=1.$$ 

## 13.2 Additività numerabile

Se gli insiemi

$$A_1,A_2,A_3,\ldots$$ 

sono misurabili e a due a due disgiunti, allora

$$\lambda_n\left(
\bigcup_{m=1}^{\infty}A_m
\right)
=
\sum_{m=1}^{\infty}\lambda_n(A_m).$$ 

---

# 14. Insiemi di misura zero e completamento di Lebesgue

Un insieme $N\subseteq\mathbb R^n$ ha misura zero quando

$$\lambda_n(N)=0.$$ 

Questo non implica che $N$ sia vuoto.

Per esempio, un singolo punto ha misura zero:

$$\lambda_1(\{x\})=0.$$ 

Anche un insieme numerabile di punti ha misura zero.

## 14.1 Sottoinsiemi di insiemi nulli

Supponiamo che $N$ sia un insieme boreliano di misura zero e che

$$A\subseteq N.$$ 

Alcuni sottoinsiemi di $N$ sono boreliani, ma altri possono non esserlo.

La sigma-algebra boreliana non è quindi completa rispetto alla misura: può contenere un insieme nullo senza contenere tutti i suoi sottoinsiemi.

La sigma-algebra di Lebesgue viene ottenuta completando quella di Borel, cioè aggiungendo tutti i sottoinsiemi degli insiemi boreliani di misura zero.

In forma schematica, un insieme Lebesgue-misurabile può essere scritto come

$$B\cup A,$$ 

dove:

- $B$ è boreliano;
- $A$ è contenuto in un insieme boreliano di misura zero.

Quindi

$$\mathcal B(\mathbb R^n)
\subsetneq
\mathcal L(\mathbb R^n).$$ 

Ogni boreliano è Lebesgue-misurabile, ma non ogni insieme Lebesgue-misurabile è boreliano.

---

# 15. Funzioni misurabili

Una funzione

$$f:X\to\mathbb R$$

è misurabile quando la controimmagine di ogni insieme boreliano è misurabile.

Se $B\subseteq\mathbb R$ è boreliano, richiediamo

$$f^{-1}(B)
=
\{x\in X:f(x)\in B\}
\in\Sigma.$$ 

Nel problema di classificazione finita, la condizione diventa semplice. La funzione

$$f:I_n\to\{1,\ldots,k\}$$

è misurabile se ogni regione di classe

$$P_j=f^{-1}(\{j\})$$

è misurabile.

Questa è la ragione per cui Cybenko assume che le regioni $P_j$ siano sottoinsiemi misurabili del cubo.

---

# 16. Funzione indicatrice

Dato un insieme $A\subseteq X$, la sua funzione indicatrice è

$$\mathbf 1_A(x)
=
\begin{cases}
1,&x\in A,\\
0,&x\notin A.
\end{cases}$$ 

La funzione indicatrice traduce l'appartenenza a un insieme in un valore numerico.

Per esempio, se

$$A=[0,1/2],$$ 

allora

$$\mathbf 1_A(0.3)=1,$$ 

mentre

$$\mathbf 1_A(0.8)=0.$$ 

---

# 17. Funzioni semplici

Una funzione semplice è una funzione misurabile che assume soltanto un numero finito di valori.

Una forma standard è

$$s(x)
=
\sum_{i=1}^{m}a_i\,\mathbf 1_{A_i}(x),$$ 

dove:

- $m$ è un numero naturale finito;
- $A_1,\ldots,A_m$ sono insiemi misurabili;
- $a_1,\ldots,a_m$ sono numeri reali;
- $\mathbf 1_{A_i}(x)$ vale $1$ se $x\in A_i$, altrimenti $0$.

Per un dato $x$, si aggiunge $a_i$ ogni volta che $x$ appartiene ad $A_i$.

Normalmente si scelgono gli insiemi $A_i$ a due a due disgiunti. In questo caso $x$ appartiene al massimo a uno degli insiemi e quindi

$$s(x)=a_i
$$ quando $x\in A_i$.

## 17.1 Esempio

Siano

$$A_1=[0,1/2]$$

e

$$A_2=(1/2,1].$$ 

Definiamo

$$s(x)
=
3\mathbf 1_{A_1}(x)
+
7\mathbf 1_{A_2}(x).$$ 

Allora

$$s(x)=3
\qquad\text{se }x\in[0,1/2],$$ 

e

$$s(x)=7
\qquad\text{se }x\in(1/2,1].$$ 

---

# 18. Integrale di Lebesgue

Per una funzione semplice non negativa

$$s(x)
=
\sum_{i=1}^{m}a_i\mathbf 1_{A_i}(x),$$ 

con gli $A_i$ disgiunti e $a_i\geq0$, si definisce

$$\int_X s\,d\lambda_n
=
\sum_{i=1}^{m}a_i\,\lambda_n(A_i).$$ 

L'idea è:

> valore assunto dalla funzione moltiplicato per il volume della regione sulla quale assume quel valore.

Nell'esempio precedente,

$$\int_{[0,1]}s\,d\lambda_1
=
3\lambda_1([0,1/2])
+
7\lambda_1((1/2,1]).$$ 

Poiché entrambi gli intervalli hanno lunghezza $1/2$,

$$\int_{[0,1]}s\,d\lambda_1
=
3\cdot\frac12+7\cdot\frac12
=
5.$$ 

Le funzioni misurabili più generali vengono integrate approssimandole mediante funzioni semplici.

---

# 19. Che cosa significa $d\lambda_n$?

Nell'espressione

$$\int_X g(x)\,d\lambda_n(x),$$ 

il simbolo

$$d\lambda_n$$

non rappresenta formalmente un infinitesimo numerico.

Significa:

> integra la funzione $g$ usando la misura $\lambda_n$.

In generale possiamo scrivere

$$\int_X g\,d\mu,$$ 

dove $\mu$ è una misura qualsiasi.

Il simbolo dopo $d$ specifica quindi come vengono pesati gli insiemi del dominio.

Con la misura di Lebesgue,

$$d\lambda_n$$

significa che gli insiemi vengono pesati secondo il loro volume euclideo ordinario.

## 19.1 Collegamento tra misura e integrale

La misura di un insieme è l'integrale della sua funzione indicatrice:

$$\boxed{
\lambda_n(A)
=
\int_{\mathbb R^n}\mathbf 1_A(x)\,d\lambda_n(x).
}$$ 

Infatti,

$$\int 1\cdot\mathbf 1_A\,d\lambda_n
=
1\cdot\lambda_n(A).$$ 

Questo è il ponte concettuale fondamentale:

- la misura agisce sugli insiemi;
- l'integrale agisce sulle funzioni;
- l'integrale dell'indicatrice recupera la misura dell'insieme.

La notazione $dx$, comune negli integrali su $\mathbb R^n$, è spesso un'abbreviazione di

$$d\lambda_n(x).$$ 

---

# 20. Il teorema di Cybenko per la classificazione

Abbiamo:

- il cubo degli input

  $$I_n=[0,1]^n;$$

- una partizione misurabile

  $$P_1,\ldots,P_k;$$

- una funzione di classificazione

  $$f(x)=j
  \quad\text{quando }x\in P_j;$$

- una rete con uscita scalare $G(x)$;
- la misura di Lebesgue $\lambda_n$.

Il Teorema 3 di Cybenko afferma che, per ogni numero

$$\varepsilon>0,$$ 

esistono:

1. una rete $G$ con un solo strato nascosto;
2. un insieme $D\subseteq I_n$;

in modo che

$$\lambda_n(D)\geq1-\varepsilon$$

e

$$|G(x)-f(x)|<\varepsilon
\qquad
\text{per ogni }x\in D.$$ 

Poiché

$$\lambda_n(I_n)=1,$$ 

la condizione

$$\lambda_n(D)\geq1-\varepsilon$$

significa che $D$ occupa quasi tutto il volume del cubo.

Definiamo l'insieme escluso

$$E=I_n\setminus D.$$ 

Allora

$$\lambda_n(E)
=
\lambda_n(I_n)-\lambda_n(D)
\leq\varepsilon.$$ 

---

# 21. Separare le due quantità di errore

Nell'enunciato originale lo stesso simbolo $\varepsilon$ viene usato sia per il volume dell'insieme escluso sia per l'errore numerico della rete.

Per chiarezza, usiamo due simboli distinti:

- $\eta>0$: volume massimo della parte esclusa;
- $\delta>0$: errore numerico massimo sul resto del dominio.

La formulazione diventa:

> per ogni $\eta>0$ e per ogni $\delta>0$, esistono una rete $G$ e un insieme misurabile $D\subseteq I_n$ tali che
> 
> $$\lambda_n(I_n\setminus D)<\eta$$
> 
> e
> 
> $$|G(x)-f(x)|<\delta
> \qquad
> \text{per ogni }x\in D.$$ 

Equivalentemente,

$$\sup_{x\in D}|G(x)-f(x)|<\delta.$$ 

Questa è un'approssimazione uniforme **su $D$**, non necessariamente sull'intero cubo $I_n$.

---

# 22. “A meno di un insieme di misura arbitrariamente piccola”

La frase significa:

> scelto un volume massimo $\eta>0$, piccolo quanto desideriamo, possiamo trovare una rete per la quale il controllo uniforme dell'errore vale fuori da un insieme $E$ avente volume inferiore a $\ldots$

Formalmente,

$$\lambda_n(E)<\eta$$

e

$$\sup_{x\in I_n\setminus E}|G(x)-f(x)|<\delta.$$ 

La parola “arbitrariamente” significa

$$\text{per ogni }\eta>0.$$ 

Non significa che esista un singolo insieme $E$ universale che funziona per tutte le precisioni. Quando cambiano $\eta$ o $\delta$, possono cambiare:

- la rete $G$;
- il numero di neuroni;
- i pesi;
- l'insieme escluso $E$.

Un insieme di misura piccola non deve necessariamente:

- contenere pochi punti;
- essere connesso;
- essere concentrato in una sola zona;
- avere diametro piccolo;
- essere una fascia regolare intorno alle frontiere.

Può essere frammentato e distribuito in tutto il dominio.

---

# 23. Perché non si può approssimare uniformemente una discontinuità su tutto il dominio?

Consideriamo una funzione di classificazione binaria:

$$f(x)
=
\begin{cases}
0,&x<1/2,\\
1,&x\geq1/2.
\end{cases}$$ 

La funzione è discontinua in

$$x=\frac12.$$ 

Supponiamo che $G$ sia continua e che valga

$$|G(x)-f(x)|<\delta
\qquad
\text{per ogni }x\in[0,1],$$ 

con

$$\delta<\frac12.$$ 

Nel punto $x=1/2$, abbiamo $f(1/2)=1$, quindi

$$G(1/2)>1-\delta>\frac12.$$ 

Per la continuità di $G$, anche per alcuni punti immediatamente a sinistra di $1/2$ deve valere

$$G(x)>\frac12.$$ 

Ma per quei punti

$$f(x)=0.$$ 

Quindi

$$|G(x)-f(x)|=|G(x)|>\frac12,$$ 

in contraddizione con l'ipotesi.

Pertanto, nessuna funzione continua può approssimare uniformemente questa funzione a gradino su tutto $[0,1]$ con errore inferiore a $1/2$.

---

# 24. Escludere una piccola regione attorno alla discontinuità

Scegliamo un numero $a>0$ e togliamo un piccolo intervallo attorno alla discontinuità:

$$E
=
\left(\frac12-a,\frac12+a\right).$$ 

La sua misura è

$$\lambda_1(E)=2a.$$ 

Il dominio rimanente è

$$D
=
\left[0,\frac12-a\right]
\cup
\left[\frac12+a,1\right].$$ 

Consideriamo la sigmoide logistica

$$\sigma(t)=\frac{1}{1+e^{-t}}$$

 e definiamo

$$G_s(x)
=
\sigma\left(s\left(x-\frac12\right)\right),$$ 

dove $s>0$ controlla la ripidità.

Per $x\leq1/2-a$, l'argomento della sigmoide è al più $-sa$, quindi $G_s(x)$ diventa vicino a $0$ quando $s$ cresce.

Per $x\geq1/2+a$, l'argomento è almeno $sa$, quindi $G_s(x)$ diventa vicino a $1$.

Aumentando $s$, possiamo rendere

$$\sup_{x\in D}|G_s(x)-f(x)|$$

piccolo quanto desideriamo.

Scegliendo $a$ piccolo, possiamo rendere

$$\lambda_1(E)=2a$$

piccolo quanto desideriamo.

Le due quantità sono controllate da due operazioni differenti:

- $a$ controlla il volume della regione esclusa;
- $s$ controlla la precisione fuori da quella regione.

---

# 25. Dall'approssimazione numerica alla classificazione corretta

Supponiamo che il teorema ci dia un insieme $D$ sul quale

$$|G(x)-f(x)|<\delta$$

con

$$\delta<\frac12.$$ 

Definiamo la classe predetta come l'etichetta intera più vicina:

$$\widehat f(x)
=
\operatorname*{arg\,min}_{c\in\{1,\ldots,k\}}
|G(x)-c|.
$$ 

Per quanto dimostrato tramite la disuguaglianza triangolare, per ogni $x\in D$ vale

$$\widehat f(x)=f(x).$$ 

Definiamo l'insieme degli errori di classificazione:

$$M
=
\{x\in I_n:\widehat f(x)\neq f(x)\}.$$

Poiché su $D$ la classificazione è corretta,

$$M\subseteq I_n\setminus D.$$ 

Pertanto,

$$\lambda_n(M)
\leq
\lambda_n(I_n\setminus D)
<\eta.$$ 

Quindi il volume dei punti classificati erroneamente può essere reso arbitrariamente piccolo.

È comunque possibile che

$$G(x)\neq f(x),$$ 

perché la rete produce un numero reale. Ciò che diventa esatto, dopo la decodifica, è

$$\widehat f(x)=f(x)$$

sul grande insieme $D$.

---

# 26. Il ruolo del teorema di Lusin

L'idea del teorema di Lusin, nel contesto attuale, è:

> una funzione misurabile può essere resa coincidente con una funzione continua, tranne che su un insieme di misura arbitrariamente piccola.

Più precisamente, per ogni $\eta>0$, si possono trovare:

- una funzione continua $h:I_n\to\mathbb R$;
- un insieme $D\subseteq I_n$;

in modo che

$$\lambda_n(I_n\setminus D)<\eta$$

e

$$h(x)=f(x)
\qquad
\text{per ogni }x\in D.$$ 

A questo punto si applica il teorema di approssimazione universale alla funzione continua $h$. Esiste una rete $G$ tale che

$$\sup_{x\in I_n}|G(x)-h(x)|<\delta.$$ 

Questa approssimazione è uniforme su tutto $I_n$, perché $h$ è continua.

Ma su $D$ vale

$$h(x)=f(x).$$ 

Quindi, per ogni $x\in D$,

$$|G(x)-f(x)|
=
|G(x)-h(x)|
<\delta.$$ 

La struttura logica è quindi:

$$f\text{ misurabile}
\xrightarrow{\text{Lusin}}
h\text{ continua su quasi tutto il dominio}
\xrightarrow{\text{approssimazione universale}}G\approx h.$$ 

---

# 27. Il teorema costruisce le superfici di separazione?

No. Il teorema dimostra che una rete adatta esiste, ma non fornisce:

- un algoritmo per trovare i pesi;
- il numero necessario di neuroni;
- la posizione dell'insieme escluso;
- la geometria delle frontiere decisionali;
- una garanzia che la discesa del gradiente trovi quella rete.

Dopo aver definito la decodifica all'intero più vicino, le frontiere tra le classi $j$ e $j+1$ sono associate a insiemi di livello del tipo

$$\{x\in I_n:G(x)=j+1/2\}.$$ 

Per esempio, tra le etichette $2$ e $3$, la soglia è

$$G(x)=2.5.$$ 

Questi insiemi possono funzionare come ipersuperfici decisionali, ma il teorema di esistenza non spiega come trovarli operativamente.

---

# 28. Mappa concettuale finale

Le regioni misurabili

$$P_1,\ldots,P_k$$

definiscono una funzione di classificazione

$$f:I_n\to\{1,\ldots,k\}.$$ 

La funzione $f$ può essere discontinua sulle frontiere tra le regioni.

Per il teorema di Lusin, possiamo trovare una funzione continua $h$ che coincide con $f$ su un insieme $D$ che occupa quasi tutto il cubo:

$$\lambda_n(I_n\setminus D)<\eta.$$ 

Il teorema di approssimazione universale fornisce una rete continua $G$ che approssima uniformemente $h$:

$$\sup_{x\in I_n}|G(x)-h(x)|<\delta.$$ 

Poiché $h=f$ su $D$,

$$\sup_{x\in D}|G(x)-f(x)|<\delta.$$ 

Se

$$\delta<\frac12$$ 

e decodifichiamo $G(x)$ scegliendo l'etichetta intera più vicina, allora

$$\widehat f(x)=f(x)
\qquad
\text{per ogni }x\in D.$$ 

Di conseguenza,

$$\lambda_n
\left(
\{x:\widehat f(x)\neq f(x)\}
\right)
<\eta.$$ 

La garanzia può essere riassunta come:

$$\boxed{
\text{errore numerico uniformemente piccolo su quasi tutto il dominio}
}$$

che, dopo una regola di decodifica appropriata, implica

$$\boxed{
\text{classificazione corretta su quasi tutto il dominio}.
}$$ 

Non si ottiene invece un'approssimazione uniforme arbitrariamente precisa della funzione discontinua in ogni punto del dominio.

---

# 29. Piano di studio suggerito

## Blocco 1 — Topologia elementare in $\mathbb R^n$

Studiare:

- distanza e norma euclidea;
- palle aperte;
- insiemi aperti e chiusi;
- punti limite;
- successioni e sottosuccessioni;
- limitatezza;
- compattezza;
- teorema di Heine-Borel.

Obiettivo: capire perché $[0,1]^n$ è compatto e perché la compattezza è utile per il controllo uniforme delle funzioni.

## Blocco 2 — Sigma-algebre e misurabilità

Studiare:

- sigma-algebre;
- complementi relativi all'universo;
- unioni e intersezioni numerabili;
- insiemi boreliani;
- funzioni misurabili;
- funzioni indicatrici.

Obiettivo: capire cosa significa che le regioni di classificazione $P_j$ sono misurabili.

## Blocco 3 — Misura di Lebesgue

Studiare:

- misura;
- additività numerabile;
- insiemi di misura zero;
- completamento della misura;
- proprietà vere quasi ovunque.

Obiettivo: interpretare rigorosamente

$$\lambda_n(I_n\setminus D)<\eta.$$ 

## Blocco 4 — Integrale di Lebesgue

Studiare:

- funzioni semplici;
- integrale delle funzioni indicatrici;
- relazione tra misura e integrale;
- significato di $d\lambda_n$.

Obiettivo: comprendere formule come

$$\lambda_n(A)=\int\mathbf 1_A\,d\lambda_n.$$ 

## Blocco 5 — Tipi di convergenza

Studiare:

- convergenza puntuale;
- convergenza uniforme;
- norma del supremo;
- convergenza quasi ovunque;
- convergenza in misura;
- norme $L^p$.

Obiettivo: distinguere chiaramente tra controllo del peggiore errore, errore medio e misura dell'insieme su cui l'errore supera una soglia.

## Blocco 6 — Applicazione a Cybenko

Studiare:

- teorema di Lusin;
- teorema di approssimazione universale;
- codifica e decodifica delle classi;
- insiemi di livello della rete;
- differenza tra esistenza e apprendimento.

---

# 30. Riferimenti essenziali

- George Cybenko, *Approximation by Superpositions of a Sigmoidal Function*, Mathematics of Control, Signals, and Systems, 1989.
- Teorema di Heine-Borel per la compattezza in $\mathbb R^n$.
- Teorema di Lusin per l'approssimazione di funzioni misurabili mediante funzioni continue fuori da insiemi di misura arbitrariamente piccola.
- Nozioni standard di misura e integrazione di Lebesgue.
