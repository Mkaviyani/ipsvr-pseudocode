# ipsvr-pseudocode
Pseudocode and supplementary algorithms for the IPSVR manuscript.

\begin{algorithm}[H]
\caption{Penalty-aware Insertion Heuristic for Initial Vehicle Tour Construction}
\label{alg:insertion-heuristic}
\begin{algorithmic}[1]
\State \textbf{Input:} Assigned job set $C = \{(j,n)\}$ for vehicle $v$ and trip $t$, distance matrix $d$, due dates $dd$, vehicle departure time $DV$, penalty cost $\lambda$
\State \textbf{Output:} Constructed delivery tour $T$
\State Initialize tour: $T \gets [0, 0]$ \Comment{Start and end at depot (node 0)}
\State $U \gets C$ \Comment{Set of unrouted jobs}
\State Initialize total load $\gets 0$, total cost $\gets 0$
\vspace{0.25em}

\While{$U \neq \emptyset$}
    \State Initialize best cost $\text{BestCost} \gets \infty$, best job $c^* \gets \emptyset$, best position $p^* \gets -1$
    \For{each job $c = (j,n) \in U$}
        \For{each insertion position $p$ in $T$}
            \State Let $a = T[p-1]$, $b = T[p]$
            \State Compute insertion cost: $\Delta_d = d(a, c) + d(c, b) - d(a, b)$
            \State Estimate delivery time $(DT)$ of $c$: \\
            \hspace{1.5em} $DT_c =
            \begin{cases}
            DV + d(0, c), & \text{if } p = 1 \\
            DT_a + d(a, c), & \text{otherwise}
            \end{cases}$
            \State Compute penalty: $P_c = \max(0, DT_c - dd_c) + \max(0, dd_c - DT_c)$
            \State Compute total insertion cost: $\text{TotalCost}_c = \Delta_d + \lambda \cdot P_c$
            \If{$\text{TotalCost}_c < \text{BestCost}$}
                \State Update best insertion: $c^* \gets c$, $p^* \gets p$, $\text{BestCost} \gets \text{TotalCost}_c$
            \EndIf
        \EndFor
    \EndFor
    \State Insert $c^*$ into position $p^*$ in $T$
    \State Update tour metrics: total load, delivery times, total cost
    \State Remove $c^*$ from unrouted set: $U \gets U \setminus \{c^*\}$
\EndWhile
\vspace{0.25em}
\State \Return Constructed tour $T$
\end{algorithmic}
\end{algorithm}
