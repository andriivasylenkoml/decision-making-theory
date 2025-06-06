Okay, this is a comprehensive lab on expert evaluation methods. Let's break it down step-by-step.

**Understanding the Core Concepts**

1.  **Експертне оцінювання (Expert Evaluation):** A method of gathering and processing opinions from knowledgeable individuals (experts) to assess complex systems, predict outcomes, or make decisions, especially when direct measurement is difficult or impossible.
2.  **Метод парного порівняння (Pairwise Comparison Method):** Experts compare objects in pairs, indicating preference or assigning scores. This helps in constructing a ranking. The values in Tables 2.1 and 2.2 (0, 1, 2) likely represent:
    *   0: Column object is preferred over row object / Row object is significantly worse.
    *   1: Row and column objects are equal / Row object is slightly preferred.
    *   2: Row object is preferred over column object / Row object is significantly better.
    (The exact meaning of 0, 1, 2 should be defined by the methodology, but summing them for each row object gives a measure of its overall "strength" or "preference".)
3.  **Ранжування (Ranking):** Ordering objects based on some criterion.
    *   **Абсолютне рангове місце (Absolute Rank):** The direct position after sorting (e.g., 1st, 2nd, 3rd). Ties are usually handled by assigning the average rank (`РАНГ.СР` or `RANK.AVG` in Excel).
    *   **Відносний ранг (Relative Rank):** Often the same as absolute rank if using average for ties.
    *   **Ранговий показник (%) (Rank Percentage):** (Rank / Max Possible Rank) * 100%. This normalizes ranks.
4.  **Коефіцієнт рангової кореляції Спірмена (Spearman's Rank Correlation Coefficient, $r_s$ or $\rho$):**
    *   **Purpose:** Measures the strength and direction of a monotonic relationship between two ranked variables. It tells you how well the relationship between two variables can be described using a monotonic function.
    *   **Values:** Ranges from -1 to +1.
        *   +1: Perfect positive correlation (as one rank increases, the other also increases).
        *   -1: Perfect negative correlation (as one rank increases, the other decreases).
        *   0: No linear correlation between ranks.
    *   **Formula (2.1):** $r s=1-\frac{6 \cdot \sum\left(X_{i}-Y_{i}\right)^{2}}{N \cdot\left(N^{2}-1\right)}$, where $X_i$ and $Y_i$ are the ranks of the $i$-th object by two experts (or criteria), and $N$ is the number of objects.
    *   **Significance:** To determine if the calculated $r_s$ is statistically significant (not due to chance), it's compared against a critical value or a p-value is calculated. Formula (2.2) $\varepsilon=\frac{1}{\sqrt{m-1}} \psi\left(\frac{1-\beta}{2}\right)$ helps find a threshold.
        *   $m$ is the number of objects ($N$ in formula 2.1).
        *   $\beta$ is the significance level (e.g., 0.05 for 5% significance).
        *   $\psi(x)$ is the inverse of the standard normal cumulative distribution function (CDF). For $\beta=0.05$, $\frac{1-\beta}{2} = 0.475$. The Z-value for which the area from 0 to Z is 0.475 (or cumulative P(Z) = 0.975) is approximately 1.96.
        *   If $|r_s| > \varepsilon$, the correlation is considered statistically significant.
5.  **Коефіцієнт конкордації Кендалла (Kendall's Coefficient of Concordance, W):**
    *   **Purpose:** Measures the degree of agreement (concordance) among several experts (d experts) who have ranked several objects (m objects).
    *   **Values:** Ranges from 0 to 1.
        *   1: Perfect agreement among experts.
        *   0: No agreement (rankings are random).
    *   **Key Calculations:**
        *   $\bar{r}=\frac{1}{m} \sum_{i=1}^{m} \sum_{s=1}^{d} r_{i s}$ (average sum of ranks for an object across all experts, but it's actually simpler: it's the sum of all ranks assigned by all experts divided by the number of objects, *or more simply, the expected sum of ranks if there was no agreement*: $\bar{R} = d(m+1)/2$. The $\bar{r}$ in formula 2.4 is the sum of ranks for object $i$ across all experts, divided by $m$, then used as the average of these *sums of ranks*. The formula for $S$ uses $\bar{r}$ as the average of the *sums of ranks per object*: $\bar{r}_{\text{sum_of_ranks}} = \frac{\sum_{i=1}^{m} (\sum_{s=1}^{d} r_{is})}{m}$).
            Let's clarify $\bar{r}$ in formula 2.4. It means the average of the *sums of ranks for each object*.
            If $R_i = \sum_{s=1}^{d} r_{is}$ is the sum of ranks for object $i$, then $\bar{R} = \frac{\sum_{i=1}^{m} R_i}{m}$.
        *   $S=\sum_{i=1}^{m}\left(\sum_{S=1}^{d} r_{iS}-\bar{r}\right)^{2}$ (sum of squared deviations of object rank sums from the mean object rank sum).
        *   $T_s=\sum_{k=1}^{H_{s}}\left(h_{k}^{3}-h_{k}\right)$ (correction factor for tied ranks for expert $s$, where $h_k$ is the number of items in the $k$-th group of ties for that expert).
        *   **Formula (2.5) for W (with ties):** $W=\frac{12 S}{d^{2}\left(m^{3}-m\right)-d \sum_{s=1}^{d} T_{S}}$
    *   **Significance (Chi-squared test):** To check if the observed agreement W is significant.
        *   **Formula (2.7):** $\chi^{2}=12 S /\left[d m(m+1)-\frac{1}{m-1} \sum_{s=1}^{d} T_{s}\right]$ (This is a specific formula given in your text. A more common one is $\chi^2 = d(m-1)W$ for $m>7$). We will use the one provided.
        *   Degrees of freedom ($df$ or $n$): $m-1$.
        *   Compare calculated $\chi^2$ with the critical value from $\chi^2$ distribution table (Table 2.6) for the given $df$ and significance level (e.g., p=0.05). If calculated $\chi^2 >$ critical $\chi^2$, the agreement is significant.

**Common Mistakes to Avoid:**

*   **Incorrectly calculating sums or ranks**, especially with ties. Use `RANK.AVG` (or `РАНГ.СР`) for ties.
*   **Errors in arithmetic** for Spearman's $d_i^2$ or Kendall's $S$ and $T_s$.
*   **Misinterpreting Spearman's $r_s$:** Correlation does not imply causation. A high $r_s$ only means ranks tend to move together (or oppositely).
*   **Misinterpreting Kendall's W:** A low W doesn't mean experts are "wrong," just that they don't agree. There might be subgroups of experts with different opinions.
*   **Forgetting the correction for ties ($T_s$)** when calculating W, if ties are present. This will inflate W.
*   **Using the wrong degrees of freedom** for the chi-squared test.
*   **Incorrectly looking up critical values** in statistical tables.

Let's proceed with the calculations.

---

**Step-by-Step Solution**

**Part 1: Calculating Sums and Ranks for Experts X and Y (Tables 2.1, 2.2, 2.3)**

**Employees (Об'єкти):**
1.  Бадикін
2.  Бабкін
3.  Богданова
4.  Грудік
5.  Макогон
6.  Пархоменко
7.  Пащенко
8.  Скляр
9.  Ціндин
Number of employees (N or m) = 9.

**1.1. Calculate Sum of Scores for Expert X (from Table 2.1)**

| Прізвище    | Бадикін | Бабкін | Богданова | Грудік | Макогон | Пархоменко | Пащенко | Скляр | Ціндин | **Сума балів (X)** |
| :---------- | :------ | :----- | :-------- | :----- | :------ | :--------- | :------ | :---- | :----- | :----------------- |
| Бадикін     | X       | 1      | 0         | 1      | 0       | 1          | 0       | 0     | 1      | 4                  |
| Бабкін      | 1       | X      | 0         | 1      | 0       | 0          | 0       | 1     | 1      | 4                  |
| Богданова   | 2       | 2      | X         | 2      | 0       | 0          | 0       | 1     | 1      | 8                  |
| Грудік      | 1       | 1      | 0         | X      | 0       | 0          | 1       | 0     | 0      | 3                  |
| Макогон     | 2       | 2      | 2         | 2      | X       | 0          | 1       | 1     | 1      | 11                 |
| Пархоменко  | 1       | 2      | 2         | 2      | 2       | X          | 1       | 1     | 2      | 13                 |
| Пащенко     | 2       | 2      | 2         | 1      | 1       | 1          | X       | 1     | 2      | 12                 |
| Скляр       | 2       | 1      | 1         | 2      | 1       | 1          | 1       | X     | 1      | 10                 |
| Ціндин      | 1       | 1      | 1         | 2      | 1       | 0          | 0       | 1     | X      | 7                  |

**1.2. Calculate Sum of Scores for Expert Y (from Table 2.2)**

| Прізвище    | Бадикін | Бабкін | Богданова | Грудік | Макогон | Пархоменко | Пащенко | Скляр | Ціндин | **Сума балів (Y)** |
| :---------- | :------ | :----- | :-------- | :----- | :------ | :--------- | :------ | :---- | :----- | :----------------- |
| Бадикін     | X       | 0      | 0         | 1      | 0       | 2          | 0       | 1     | 0      | 4                  |
| Бабкін      | 2       | X      | 0         | 1      | 0       | 0          | 1       | 1     | 1      | 6                  |
| Богданова   | 0       | 1      | X         | 2      | 1       | 1          | 0       | 0     | 2      | 7                  |
| Грудік      | 1       | 1      | 0         | X      | 0       | 1          | 1       | 1     | 1      | 6                  |
| Макогон     | 1       | 2      | 2         | 2      | X       | 0          | 0       | 1     | 1      | 9                  |
| Пархоменко  | 2       | 1      | 2         | 2      | 2       | X          | 1       | 0     | 1      | 11                 |
| Пащенко     | 2       | 2      | 2         | 1      | 1       | 1          | X       | 1     | 1      | 11                 |
| Скляр       | 1       | 0      | 1         | 2      | 0       | 0          | 1       | X     | 0      | 5                  |
| Ціндин      | 1       | 1      | 1         | 2      | 1       | 0          | 2       | 0     | X      | 8                  |

**1.3. Create Ranking Tables (Table 2.3 Format)**

For ranking, we sort by "Сума балів" in *ascending* order as requested by "Відсортуйте за допомогою фільтрів поле «Сума балів» у порядку зростання та відповідно надайте кожному експерту абсолютний ранг." This means lower sums get better (lower) ranks. However, typically higher scores mean better performance, leading to higher ranks for higher scores. Let's assume the standard: **higher sum = better rank (lower rank number)**. If the instruction means "sort sums ascendingly, then assign rank 1 to the smallest sum", we'll adjust. The example table for Spearman (Table 2.4) starts with Grudik, who has the lowest sum for X, implying rank 1 for the lowest sum. Let's follow this: **lowest sum = rank 1**.

Excel function `RANK.AVG(number, ref, [order])` or `РАНГ.СР(число;ссылка;[порядок])`.
Order = 1 for ascending (lowest value = rank 1).
Order = 0 for descending (highest value = rank 1).
Given "у порядку зростання" (in ascending order) for sums implies rank 1 for the smallest sum. So, `order = 1`.

**Table 2.3 for Expert X (Scores from 1.1)**
N = 9. Max rank = 9.

| Прізвище    | Сума балів (X) | Абсолютне рангове місце (Ранг X) (RANK.AVG, ascending) | Ранговий показник (%) (Ранг X / 9 * 100) |
| :---------- | :------------- | :----------------------------------------------------- | :--------------------------------------- |
| Грудік      | 3              | 1                                                      | (1/9)*100 = 11.11%                       |
| Бадикін     | 4              | 2.5                                                    | (2.5/9)*100 = 27.78%                     |
| Бабкін      | 4              | 2.5                                                    | (2.5/9)*100 = 27.78%                     |
| Ціндин      | 7              | 4                                                      | (4/9)*100 = 44.44%                       |
| Богданова   | 8              | 5                                                      | (5/9)*100 = 55.56%                       |
| Скляр       | 10             | 6                                                      | (6/9)*100 = 66.67%                       |
| Макогон     | 11             | 7                                                      | (7/9)*100 = 77.78%                       |
| Пащенко     | 12             | 8                                                      | (8/9)*100 = 88.89%                       |
| Пархоменко  | 13             | 9                                                      | (9/9)*100 = 100.00%                      |

**Table 2.3 for Expert Y (Scores from 1.2)**
N = 9. Max rank = 9.

| Прізвище    | Сума балів (Y) | Абсолютне рангове місце (Ранг Y) (RANK.AVG, ascending) | Ранговий показник (%) (Ранг Y / 9 * 100) |
| :---------- | :------------- | :----------------------------------------------------- | :--------------------------------------- |
| Бадикін     | 4              | 1                                                      | (1/9)*100 = 11.11%                       |
| Скляр       | 5              | 2                                                      | (2/9)*100 = 22.22%                       |
| Бабкін      | 6              | 3.5                                                    | (3.5/9)*100 = 38.89%                     |
| Грудік      | 6              | 3.5                                                    | (3.5/9)*100 = 38.89%                     |
| Богданова   | 7              | 5                                                      | (5/9)*100 = 55.56%                       |
| Ціндин      | 8              | 6                                                      | (6/9)*100 = 66.67%                       |
| Макогон     | 9              | 7                                                      | (7/9)*100 = 77.78%                       |
| Пархоменко  | 11             | 8.5                                                    | (8.5/9)*100 = 94.44%                     |
| Пащенко     | 11             | 8.5                                                    | (8.5/9)*100 = 94.44%                     |

---

**Part 2: Spearman's Rank Correlation Coefficient (Table 2.4)**

We need the ranks $X_i$ and $Y_i$ from the tables above.
$N = 9$.
$N(N^2-1) = 9(9^2-1) = 9(81-1) = 9(80) = 720$.
Formula: $r_s = 1 - \frac{6 \sum (X_i - Y_i)^2}{N(N^2-1)}$

**Table 2.4 Calculation**
The table is presorted by Rank X.

| Прізвище    | Ранг $X_i$ | Ранг $Y_i$ | $d_i = X_i - Y_i$ | $(X_i - Y_i)^2$ |
| :---------- | :--------- | :--------- | :---------------- | :-------------- |
| Грудік      | 1          | 3.5        | -2.5              | 6.25            |
| Бадикін     | 2.5        | 1          | 1.5               | 2.25            |
| Бабкін      | 2.5        | 3.5        | -1.0              | 1.00            |
| Ціндин      | 4          | 6          | -2.0              | 4.00            |
| Богданова   | 5          | 5          | 0.0               | 0.00            |
| Скляр       | 6          | 2          | 4.0               | 16.00           |
| Макогон     | 7          | 7          | 0.0               | 0.00            |
| Пащенко     | 8          | 8.5        | -0.5              | 0.25            |
| Пархоменко  | 9          | 8.5        | 0.5               | 0.25            |
| **Сума**    |            |            |                   | **$\sum d_i^2 = 30.00$** |

Calculation of $r_s$:
$r_s = 1 - \frac{6 \times 30.00}{720}$
$r_s = 1 - \frac{180}{720}$
$r_s = 1 - 0.25$
$r_s = 0.75$

**Interpretation of Spearman's $r_s$:**
An $r_s$ of 0.75 indicates a strong positive correlation between the rankings of the employees by Expert X and Expert Y. This means that, generally, employees ranked highly by Expert X also tend to be ranked highly by Expert Y, and vice-versa.

**Significance Test for $r_s$ (using formula 2.2):**
$\varepsilon=\frac{1}{\sqrt{m-1}} \psi\left(\frac{1-\beta}{2}\right)$
Here, $m=N=9$. Let's choose a significance level $\beta = 0.05$.
Then $\frac{1-\beta}{2} = \frac{1-0.05}{2} = \frac{0.95}{2} = 0.475$.
$\psi(0.475)$ is the value $z$ from the standard normal distribution table such that the area under the curve from 0 to $z$ is 0.475. This corresponds to a cumulative probability of $0.5 + 0.475 = 0.975$. The $z$-value is 1.96.
So, $\psi(0.475) \approx 1.96$.

$\varepsilon = \frac{1}{\sqrt{9-1}} \times 1.96 = \frac{1}{\sqrt{8}} \times 1.96 = \frac{1}{2.8284} \times 1.96 \approx 0.3535 \times 1.96 \approx 0.6929$

Since $|r_s| = |0.75| = 0.75$, and $0.75 > \varepsilon \approx 0.6929$, the correlation is statistically significant at the 0.05 level.

**Conclusion for Spearman's Correlation:**
The calculated Spearman's rank correlation coefficient is $r_s = 0.75$. The critical value for significance at $\alpha=0.05$ is $\varepsilon \approx 0.693$. Since $|0.75| > 0.693$, we reject the null hypothesis ($H_0$) that there is no correlation. The correlation between the rankings of Expert X and Expert Y is statistically significant.

---

**Part 3: Kendall's Coefficient of Concordance (W) (Table 2.5)**

This part uses a different dataset from Table 2.5.
Number of objects (employees/candidates) $m = 11$.
Number of experts $d = 5$.

**Data from Table 2.5:**
| Експерти Об'єкти   | Бах (E1) | Чугай (E2) | Гумен (E3) | Молек (E4) | Спірман (E5) | $\sum_{s=1}^{d} r_{is}$ (Sum of ranks for object i, $R_i$) |
| :----------------- | :------- | :--------- | :--------- | :--------- | :----------- | :----------------------------------------------------------- |
| Сундутов Сергій    | 1.5      | 4          | 3.5        | 5.5        | 1.5          | 16.0                                                         |
| Чижевський Віталій | 1.5      | 1          | 1          | 3.5        | 1.5          | 8.5                                                          |
| Мукоєд Олег        | 3        | 2          | 3.5        | 1.5        | 3.5          | 13.5                                                         |
| Ткачук Сергій      | 4        | 3          | 9          | 3.5        | 7            | 26.5                                                         |
| Сьох Анастасія     | 5        | 8          | 2          | 7          | 9.5          | 31.5                                                         |
| Мартинюк Антон     | 6.5      | 6          | 7.5        | 10.5       | 9.5          | 40.0                                                         |
| Мельник Анастасія  | 6.5      | 7          | 5.5        | 5.5        | 3.5          | 28.0                                                         |
| Стрілець Наталя    | 8        | 5          | 5.5        | 1.5        | 9.5          | 29.5                                                         |
| Прокопець Віталій  | 9.5      | 10.5       | 10.5       | 8.5        | 5.5          | 44.5                                                         |
| Чугай Максим       | 9.5      | 10.5       | 10.5       | 8.5        | 5.5          | 44.5                                                         |
| Мовчанець Андрій   | 11       | 9          | 7.5        | 10.5       | 9.5          | 47.5                                                         |
| **Total Sum of $R_i$** |          |            |            |            |              | **330.0**                                                    |

**3.1. Calculate $\bar{r}$ (Mean of the sums of ranks $R_i$)**
$\bar{R} = \frac{\sum R_i}{m} = \frac{330.0}{11} = 30.0$.
(Note: The formula (2.3) $\bar{r}=\frac{1}{m} \sum_{i=1}^{m} \sum_{s=1}^{d} r_{i s}$ is exactly this).
Alternatively, the expected sum of ranks for an object if there's no agreement is $d(m+1)/2 = 5(11+1)/2 = 5 \times 12 / 2 = 30$. This matches.

**3.2. Calculate $S = \sum_{i=1}^{m}\left(R_i - \bar{R}\right)^{2}$**
(This is the column $\left(\sum_{s=1}^{\mathrm{d}} \mathrm{r}_{\text {is }}-\overline{\mathrm{r}}\right)^{2}$ in Table 2.5)

| Об'єкт             | $R_i$ | $R_i - \bar{R}$ ($R_i - 30$) | $(R_i - \bar{R})^2$ |
| :----------------- | :---- | :--------------------------- | :------------------ |
| Сундутов Сергій    | 16.0  | -14.0                        | 196.00              |
| Чижевський Віталій | 8.5   | -21.5                        | 462.25              |
| Мукоєд Олег        | 13.5  | -16.5                        | 272.25              |
| Ткачук Сергій      | 26.5  | -3.5                         | 12.25               |
| Сьох Анастасія     | 31.5  | 1.5                          | 2.25                |
| Мартинюк Антон     | 40.0  | 10.0                         | 100.00              |
| Мельник Анастасія  | 28.0  | -2.0                         | 4.00                |
| Стрілець Наталя    | 29.5  | -0.5                         | 0.25                |
| Прокопець Віталій  | 44.5  | 14.5                         | 210.25              |
| Чугай Максим       | 44.5  | 14.5                         | 210.25              |
| Мовчанець Андрій   | 47.5  | 17.5                         | 306.25              |
|                    |       | **Сума $S$**                 | **1776.00**         |

So, $S = 1776.00$.

**3.3. Calculate $T_s$ for each expert (Formula 2.6: $T_{s}=\sum_{k=1}^{H_{s}}\left(h_{k}^{3}-h_{k}\right)$)**

*   **Expert 1 (Бах):**
    *   Ranks: 1.5, 1.5 (2 tied); 3; 4; 5; 6.5, 6.5 (2 tied); 8; 9.5, 9.5 (2 tied); 11.
    *   Ties: (1.5, 1.5) -> $h_1=2 \implies 2^3-2 = 6$
    *   Ties: (6.5, 6.5) -> $h_2=2 \implies 2^3-2 = 6$
    *   Ties: (9.5, 9.5) -> $h_3=2 \implies 2^3-2 = 6$
    *   $T_1 = 6 + 6 + 6 = 18$. (Matches example in prompt)

*   **Expert 2 (Чугай):**
    *   Ranks: 1; 2; 3; 4; 5; 6; 7; 8; 9; 10.5, 10.5 (2 tied).
    *   Ties: (10.5, 10.5) -> $h_1=2 \implies 2^3-2 = 6$
    *   $T_2 = 6$.

*   **Expert 3 (Гумен):**
    *   Ranks: 1; 2; 3.5, 3.5 (2 tied); 5.5, 5.5 (2 tied); 7.5, 7.5 (2 tied); 9; 10.5, 10.5 (2 tied).
    *   Ties: (3.5, 3.5) -> $h_1=2 \implies 6$
    *   Ties: (5.5, 5.5) -> $h_2=2 \implies 6$
    *   Ties: (7.5, 7.5) -> $h_3=2 \implies 6$
    *   Ties: (10.5, 10.5) -> $h_4=2 \implies 6$
    *   $T_3 = 6 + 6 + 6 + 6 = 24$.

*   **Expert 4 (Молек):**
    *   Ranks: 1.5, 1.5 (2 tied); 3.5, 3.5 (2 tied); 5.5, 5.5 (2 tied); 7; 8.5, 8.5 (2 tied); 10.5, 10.5 (2 tied).
    *   Ties: (1.5, 1.5) -> $h_1=2 \implies 6$
    *   Ties: (3.5, 3.5) -> $h_2=2 \implies 6$
    *   Ties: (5.5, 5.5) -> $h_3=2 \implies 6$
    *   Ties: (8.5, 8.5) -> $h_4=2 \implies 6$
    *   Ties: (10.5, 10.5) -> $h_5=2 \implies 6$
    *   $T_4 = 6 + 6 + 6 + 6 + 6 = 30$.

*   **Expert 5 (Спірман):**
    *   Ranks: 1.5, 1.5 (2 tied); 3.5, 3.5 (2 tied); 5.5, 5.5 (2 tied); 7; 9.5, 9.5, 9.5, 9.5 (4 tied - Сьох, Мартинюк, Стрілець, Мовчанець).
        *Checking the table carefully for Spirman: Sundutov 1.5, Chizhevsky 1.5 (2 tied). Mukoed 3.5, Melnyk 3.5 (2 tied). Prokopets 5.5, Chugai M 5.5 (2 tied). Tkachuk 7. Syokh 9.5, Martyniuk 9.5, Strilec 9.5, Movchanets 9.5 (4 tied). This list is correct based on the provided table for expert Spirman.*
    *   Ties: (1.5, 1.5) -> $h_1=2 \implies 6$
    *   Ties: (3.5, 3.5) -> $h_2=2 \implies 6$
    *   Ties: (5.5, 5.5) -> $h_3=2 \implies 6$
    *   Ties: (9.5, 9.5, 9.5, 9.5) -> $h_4=4 \implies 4^3-4 = 64-4 = 60$
    *   $T_5 = 6 + 6 + 6 + 60 = 78$.

Sum of $T_s$: $\sum T_s = T_1 + T_2 + T_3 + T_4 + T_5 = 18 + 6 + 24 + 30 + 78 = 156$.

**3.4. Calculate Kendall's W (Formula 2.5)**
$W=\frac{12 S}{d^{2}\left(m^{3}-m\right)-d \sum T_{S}}$
$m=11, d=5, S=1776, \sum T_s = 156$.
$m^3-m = 11^3-11 = 1331-11 = 1320$.
$d^2(m^3-m) = 5^2 \times 1320 = 25 \times 1320 = 33000$.
$d \sum T_s = 5 \times 156 = 780$.

$W = \frac{12 \times 1776}{33000 - 780} = \frac{21312}{32220}$
$W \approx 0.6614$

**Interpretation of W:**
A Kendall's W of approximately 0.6614 indicates a moderately high level of agreement among the 5 experts in ranking the 11 objects. Perfect agreement would be W=1, and no agreement would be W=0.

**3.5. Evaluate Significance of W using Chi-Squared Test (Formula 2.7)**
$\chi^{2}=12 S /\left[d m(m+1)-\frac{1}{m-1} \sum T_{s}\right]$
$d m(m+1) = 5 \times 11 \times (11+1) = 5 \times 11 \times 12 = 5 \times 132 = 660$.
$\frac{1}{m-1} \sum T_s = \frac{1}{11-1} \times 156 = \frac{1}{10} \times 156 = 15.6$.

$\chi^2_{calc} = \frac{12 \times 1776}{660 - 15.6} = \frac{21312}{644.4}$
$\chi^2_{calc} \approx 33.0726$

Degrees of freedom $n = m-1 = 11-1 = 10$.
Significance level, let's use $p=0.05$.
From Table 2.6 (Значення $x^{2}$): for $n=10$ and $p=0.05$, the critical $\chi^2_{crit} = 18.3$.

**Comparison and Conclusion for Concordance:**
Calculated $\chi^2_{calc} \approx 33.07$.
Critical $\chi^2_{crit}(df=10, p=0.05) = 18.3$.
Since $\chi^2_{calc} (33.07) > \chi^2_{crit} (18.3)$, we reject the null hypothesis that the experts' rankings are unrelated. The agreement among the experts is statistically significant.

The hypothesis about the convergence of expert ranking estimates is accepted.

---

**Summary of Results**

1.  **Pairwise Comparison & Ranking:** Scores and ranks for 9 employees were determined based on evaluations by Expert X and Expert Y.
2.  **Spearman's Rank Correlation:** $r_s = 0.75$. This indicates a strong, statistically significant positive correlation between the rankings of Expert X and Expert Y. They generally agree on the relative order of the employees.
3.  **Kendall's Coefficient of Concordance:** For a separate dataset of 11 objects and 5 experts, $W \approx 0.6614$. This indicates a moderately high and statistically significant level of agreement among the 5 experts.

---

**Similar Example Problem with Solution**

Let's consider a simpler scenario with 3 experts evaluating 5 new software products (A, B, C, D, E).

**Experts' Rankings:**

| Product | Expert 1 (Ranks) | Expert 2 (Ranks) | Expert 3 (Ranks) |
| :------ | :--------------- | :--------------- | :--------------- |
| A       | 1                | 2                | 1                |
| B       | 2                | 1                | 3                |
| C       | 3                | 4                | 2                |
| D       | 5                | 3                | 5                |
| E       | 4                | 5                | 4                |

**Task 1: Calculate Spearman's Rank Correlation between Expert 1 and Expert 2.**
$N=5$. $N(N^2-1) = 5(25-1) = 5(24) = 120$.

| Product | Rank $X_1$ | Rank $X_2$ | $d_i = X_1 - X_2$ | $d_i^2$ |
| :------ | :--------- | :--------- | :---------------- | :------ |
| A       | 1          | 2          | -1                | 1       |
| B       | 2          | 1          | 1                 | 1       |
| C       | 3          | 4          | -1                | 1       |
| D       | 5          | 3          | 2                 | 4       |
| E       | 4          | 5          | -1                | 1       |
|         |            |            | **Sum $d_i^2$**   | **8**   |

$r_s = 1 - \frac{6 \times 8}{120} = 1 - \frac{48}{120} = 1 - 0.4 = 0.6$.
**Interpretation:** A Spearman's $r_s$ of 0.6 suggests a positive moderate correlation between Expert 1 and Expert 2.

**Task 2: Calculate Kendall's Coefficient of Concordance (W) for all 3 experts.**
$m=5$ (products), $d=3$ (experts).

1.  **Sum of ranks for each product ($R_i$):**
    *   A: $1+2+1 = 4$
    *   B: $2+1+3 = 6$
    *   C: $3+4+2 = 9$
    *   D: $5+3+5 = 13$
    *   E: $4+5+4 = 13$
    Total sum of $R_i = 4+6+9+13+13 = 45$.

2.  **Calculate mean of $R_i$ ($\bar{R}$):**
    $\bar{R} = \frac{45}{5} = 9$.
    (Alternatively: $d(m+1)/2 = 3(5+1)/2 = 3 \times 3 = 9$).

3.  **Calculate $S = \sum (R_i - \bar{R})^2$:**
    *   A: $(4-9)^2 = (-5)^2 = 25$
    *   B: $(6-9)^2 = (-3)^2 = 9$
    *   C: $(9-9)^2 = (0)^2 = 0$
    *   D: $(13-9)^2 = (4)^2 = 16$
    *   E: $(13-9)^2 = (4)^2 = 16$
    $S = 25+9+0+16+16 = 66$.

4.  **Calculate $T_s$ (correction for ties):**
    *   Expert 1: No ties. $T_1 = 0$.
    *   Expert 2: No ties. $T_2 = 0$.
    *   Expert 3: No ties. $T_3 = 0$.
    $\sum T_s = 0$.

5.  **Calculate W (using formula for no ties or with $T_s=0$):**
    $W=\frac{12 S}{d^{2}(m^{3}-m)-d \sum T_{S}}$
    $m^3-m = 5^3-5 = 125-5 = 120$.
    $d^2(m^3-m) = 3^2(120) = 9 \times 120 = 1080$.
    $d \sum T_s = 3 \times 0 = 0$.
    $W = \frac{12 \times 66}{1080 - 0} = \frac{792}{1080} \approx 0.733$.

**Interpretation of W:** A W of 0.733 indicates a good level of agreement among the 3 experts.

**Significance of W (using common formula $\chi^2 = d(m-1)W$ as $m \le 7$ is okay, or using the lab's formula (2.7)):**
Let's use lab's formula (2.7): $\chi^{2}=12 S /\left[d m(m+1)-\frac{1}{m-1} \sum T_{s}\right]$
$d m(m+1) = 3 \times 5 \times (5+1) = 3 \times 5 \times 6 = 90$.
$\frac{1}{m-1} \sum T_s = \frac{1}{4} \times 0 = 0$.
$\chi^2_{calc} = \frac{12 \times 66}{90 - 0} = \frac{792}{90} = 8.8$.

Degrees of freedom $n = m-1 = 5-1 = 4$.
From Table 2.6, for $n=4$ and $p=0.05$, $\chi^2_{crit} = 9.49$.
Since $\chi^2_{calc} (8.8) < \chi^2_{crit} (9.49)$, the agreement is **not statistically significant** at the 0.05 level using this specific formula.
(If we used the more common $\chi^2 = d(m-1)W = 3(5-1)(0.733) = 12 \times 0.733 = 8.796 \approx 8.8$. The result is the same).
This shows that even with a W of 0.733, with a small number of objects and experts, it might not be statistically significant.

This detailed breakdown should help you understand and complete your laboratory work. Remember to use Excel for calculations where appropriate, as suggested.
