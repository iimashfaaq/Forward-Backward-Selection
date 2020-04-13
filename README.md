# Forward-Backward Selection with Early Dropping

## Giorgos Borboudakis, Ioannis Tsamardinos; 20(8):1−39, 2019.

## Abstract

Forward-backward selection is one of the most basic and commonly-used feature selection algorithms available. It is also general and conceptually applicable to many different types of data. In this paper, we propose a heuristic that significantly improves its running time, while preserving predictive performance. The idea is to temporarily discard the variables that are conditionally independent with the outcome given the selected variable set. Depending on how those variables are reconsidered and reintroduced, this heuristic gives rise to a family of algorithms with increasingly stronger theoretical guarantees. In distributions that can be faithfully represented by Bayesian networks or maximal ancestral graphs, members of this algorithmic family are able to correctly identify the Markov blanket in the sample limit. In experiments we show that the proposed heuristic increases computational efficiency by about 1-2 orders of magnitude, while selecting fewer or the same number of variables and retaining predictive performance. Furthermore, we show that the proposed algorithm and feature selection with LASSO perform similarly when restricted to select the same number of variables, making the proposed algorithm an attractive alternative for problems where no (efficient) algorithm for LASSO exists.

## Algorithm for Forawrd Backward Selection (FBS)
``` code
Input: Dataset D, Target T 
Output: Selected Variables S 
1: S ←∅ //Set of selected variables 
2: R ← V //Set of remaining candidate variables 
3: 
4: //Forward phase: iterate until S does not change 
5: while S changes do 
6: //Identify the best variable Vbest out of all remaining variables R, according to Perf 
7: Vbest ← argmax V∈R Perf(S∪V ) 
8: //Select Vbest if it increases performance according to criterion C 
9: if Perf(S∪Vbest) > C Perf(S) then 
10: S ← S∪Vbest 
11: R ← R\Vbest 
12: end if 
13: end while 
14: 
15: //Backward phase: iterate until S does not change 
16: while S changes do 
17: //Identify the worst variable Vworst out of all selected variables S, according to Perf 
18: Vworst ← argmax V∈S Perf(S\V ) 
19: //Remove Vworst if it does not decrease performance according to criterion C 
20: if Perf(S\Vworst) ≥ C Perf(S) then 
21: S ← S\Vworst 
22: end if 
23: end while 
24: return S
```

## Algorithm for Forawrd Backward Selection with Early Dropping (FBED)
``` code
Input: Dataset D, Target T, Maximum Number of Runs K 
Output: Selected Variables S 
1: S ←∅ //Set of selected variables 
2: Kcur ← 0 //Initializing current number of runs to 0 
3: 
4: //Forward phase: iterate until (a) run limit reached, or (b) S does not change 
5: while Kcur ≤ K∧ S changes do 
6: S ← OneRun(D,T,S) 
7: Kcur ← Kcur + 1 
8: end while 
9: 
10: //Perform backward selection and return result
11: return BackwardSelection(D,T,S) 
-----------------------------------------------------------------------
12: function OneRun(D, T, S) 
13: R ← V\S //Set of remaining candidate variables 
14: 
15: //Forward phase: iterate until R is empty 
16: while |R| > 0 do 
17: //Identify best variable Vbest out of R, according to Perf 
18: Vbest ← argmax V∈R Perf(S∪V ) 
19: //Select Vbest if it increases performance according to criterion C 
20: if Perf(S∪Vbest) > C Perf(S) then 
21: S ← S∪Vbest 
22: end if 
23: //Drop all variables from R not satisfying C 
24: R ←{V : V ∈ R∧V 6= Vbest ∧Perf(S∪V ) > C Perf(S)} 
25: end while 
26: return S 
27: end function
```

## Implementation of algorithm
### Language used
- Python 3.x

### Dataset used
- Wine dataset from sklearn

### Libraries used
- Numpy
- Pandas
- sklearn
- matplotlib

### Model used
- **RandomForestClassifier** is used for data Classification
