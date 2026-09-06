---
comments: true
tags: ["1 семестр"]
---

!!! info "Определение"
    **Множество – совокупность различающихся между собой объектов, которые**
    **называются элементами данного множества.**

!!! info "Определение"
    **Пусть А и В – множества. $A \subset B$, если $\forall\space x\space (x \in A \Rightarrow x \in B)$ (Читается так: при любом**
    **значении х выполняется условие: если х принадлежит А, тогда х принадлежит В).**

!!! info "Определение"
    Множества А и В равны $(A = B)$, если $A \subset B \space и\space B \subset A$, то есть $A = B \Leftrightarrow \forall \space x \space (x \in A \Leftrightarrow x \in B)$

## Операции над множествами
$A \cup B$ - Объединение, элементы которого являются хотя бы одного из множеств А или В ($x \in A$ и/или $x \in B$)
$A \cap B$ - Пересечение, элементы которого одновременно являются и А, и В
А\B - Разность, элементы которого входят в А, но не входят в В

## Основные свойства
$$
1. \space (A \cup B) \cap C = (A \cap C) \cup (B \cap C)
$$

**Доказательство**

$$
\Rightarrow : x \in (A \cup B) \cap C \rightarrow x \in A \cup B \space и \space x \in C\rightarrow (x \in A \space и \space x \in C) \space или \space (x \in B \space и \space x \in C) \rightarrow x \in (A \cap C) \cup (B \cap C).
\Leftarrow : x \in (A \cap C) \cup (B \cap C) \rightarrow x \in A \cap C \space или \space x \in B \cap C \rightarrow x \in C \space и \space (x \in A \space или \space x \in B) \rightarrow x \in (A \cup B) \cap C
$$

$$
2. \space (A \cap B) \cup C = (A \cup C) \cap (B \cup C)
$$

$$
3. \space \overline{A\cup B} = \bar{A} \cap \bar{B}
$$

**Доказательство**

$$
\Rightarrow: x \in \overline{A \cup B} \rightarrow x \in \bar{A} \space \overline{или} \space x \in \bar{B} \rightarrow x \in \bar{A} \space и \space x \in \bar{B} \rightarrow x \in \bar{A} \cap \bar{B}
\Leftarrow: x \in \bar{A} \cap \bar{B} \rightarrow x \in \bar{A} \space и \space x \in \bar{B} \rightarrow x \in \bar{A} \space \overline{или} \space x \in \bar{B} \rightarrow x \in \overline{A \cup B}
$$

$$
\overline{A\cap B} = \bar{A} \cup \bar{B}
$$
