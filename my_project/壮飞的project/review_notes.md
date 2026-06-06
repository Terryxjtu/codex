# review_notes.md

# Overleaf 源码审阅意见  
**项目文件：** `波动率风险溢价__马壮飞_英文.zip`  
**源码文件：** `main.tex`, `main_V2.tex`, `ref.bib`, `r2.png`, `eng r2.png`, `correlation.png`, `image.png`  
**审阅原则：** 不直接修改论文源码，仅生成修改意见。  
**建议主稿：** 优先以 `main_V2.tex` 为当前修改基础；`main.tex` 更像早期版本或备份稿。

---

## 0. 源码读取情况与总体判断

本次已经实际读取了你上传的 Overleaf 压缩包。文件结构较简单：

```text
main.tex
main_V2.tex
ref.bib
eng r2.png
correlation.png
r2.png
image.png
```

两份 tex 文件内容有明显差异：

1. `main.tex`  
   - 题目为：**VRP in the Chinese Market: An Empirical Analysis Based on CSI 300 ETF Options**  
   - 使用 `\usepackage[numbers, sort&compress]{natbib}`，参考文献为手写 `thebibliography`。
   - 引文系统不完整，正文没有规范使用 `ref.bib`。
   - 含有较多中文注释、中文 label 和较粗糙的早期表述。
   - 更像早期初稿或备份版本。

2. `main_V2.tex`  
   - 题目为：**Pricing Volatility Risk in China: A HAR-RV Refined Approach to Term Structure and Asymmetric Predictability**
   - 使用 `\usepackage[authoryear, round]{natbib}` 和 `\bibliographystyle{apalike}`。
   - 正文结构更完整，包含 Introduction、Literature Review、Data and Methodology、Empirical Analysis、HAR-VRP Model、Conclusions。
   - 文献引用相对规范，并调用 `ref.bib`。
   - 更像当前应继续完善的投稿版本。

因此，建议后续以 `main_V2.tex` 为主稿，`main.tex` 只作为参考或旧版备份，避免两个版本同时继续修改导致内容不一致。

---

## 1. 论文结构

### 1.1 当前结构评价

`main_V2.tex` 的结构如下：

```text
Introduction
Literature Review
  Theoretical Foundations and International Evidence
  The VRP Landscape in China’s Emerging Market
  Methodological Refinement and Research Gaps
Data and Methodology
  Sample Selection and Data Sources
  Construction of Volatility Metrics
  Definition of the Volatility Risk Premium
  Predictive Regression Framework
Empirical Analysis
  Descriptive Statistics and Correlation Analysis
  Univariate Predictive Regressions
  Multivariate Predictive Regressions
Empirical Refinement via the HAR-VRP Model
  The HAR-RV Specification
  Predictive Performance of the Univariate HAR-VRP Model
  Multivariate Analysis and Comparative Predictive Power
Conclusions
```

总体结构是可用的，基本符合实证金融论文常见写法。主线是：

> 先构造 VRP，再考察 VRP 对 CSI 300 ETF 未来收益的期限结构预测能力，最后用 HAR-RV 提炼 realized volatility，从而构造 HAR-refined VRP 并比较其预测能力。

这个主线是清楚的。

### 1.2 主要结构问题

目前最主要的问题是：**论文标题、摘要、模型和实证结果之间的重点略有错位。**

题目强调：

```text
Pricing Volatility Risk in China
HAR-RV Refined Approach
Term Structure and Asymmetric Predictability
```

正文中确实研究了 term structure 和 HAR-refined VRP，但 “asymmetric predictability” 的理论和实证展开还不够充分。现在的非对称性主要体现在 `VRP+`、`VRP-`、`HVRP+`、`HVRP-` 的回归结果比较，但缺少一个单独的小节系统讨论：

1. 为什么要分解正负 VRP；
2. 正负 VRP 的经济含义分别是什么；
3. 正负 VRP 的预测差异如何体现 “asymmetric predictability”；
4. 这种非对称性与中国市场制度、投资者结构、ETF option 市场流动性有什么关系。

建议在 Empirical Analysis 中增加一个小节：

```text
Asymmetric Predictability of Positive and Negative VRP Components
```

或在 HAR-VRP 部分增加：

```text
Asymmetric Predictability after HAR-RV Refinement
```

这样题目中的 “Asymmetric Predictability” 会更有支撑。

### 1.3 建议结构微调

建议将当前第四、第五部分调整为更清晰的逻辑：

```text
4. Empirical Analysis
   4.1 Descriptive Statistics and Correlation Analysis
   4.2 Term-Structure Predictability of VRP
   4.3 Asymmetric Predictability of Positive and Negative VRP
   4.4 Multivariate Robustness

5. HAR-RV Refinement and Predictive Performance
   5.1 HAR-RV Forecasting of Physical Volatility
   5.2 Construction of HAR-Refined VRP
   5.3 Term-Structure Predictability of HVRP
   5.4 Asymmetric Predictability of HVRP+ and HVRP-
   5.5 Comparative Model Fit
```

这样能避免现在 `Empirical Analysis` 和 `Empirical Refinement via the HAR-VRP Model` 两部分之间有些割裂。

### 1.4 引言中的路线图需要更清楚

Introduction 很长，信息量充分，但建议末尾增加一段非常清楚的 roadmap，例如：

```text
The empirical analysis proceeds in four steps. First, we construct model-free implied volatility, realized volatility, and the corresponding VRP measures for the CSI 300 ETF options market. Second, we examine the term structure of VRP-based return predictability over horizons from 1 to 360 trading days. Third, we decompose the VRP into positive and negative components to test asymmetric predictability. Finally, we use the HAR-RV framework to refine the physical-volatility component and evaluate whether the HAR-refined VRP improves predictive performance.
```

---

## 2. 理论贡献

### 2.1 当前理论贡献

论文目前的理论贡献可以概括为：

1. 将 VRP 的资产收益预测框架应用到中国 CSI 300 ETF options 市场；
2. 考察 VRP 预测能力的期限结构；
3. 将 realized volatility 的 HAR-RV 预测值用于改进 VRP 构造；
4. 比较整体 VRP、正向/负向 VRP、HAR-refined VRP 的预测能力。

这个贡献方向是有价值的，特别是 CSI 300 ETF options 于 2019 年底推出，2020–2025 样本覆盖疫情、市场调整和中国衍生品市场发展阶段，具有中国市场背景。

### 2.2 需要进一步凸显的理论问题

当前稿件中 “Pricing Volatility Risk” 的理论解释还可以更深入。现在更多是在说 VRP 有预测力，但对以下问题解释不足：

1. VRP 为什么应当预测未来收益？
2. 为什么预测方向会随 horizon 改变？
3. 为什么中期为正、长期为负？
4. 为什么 HAR-refined VRP 可以更好地反映 physical expectation？
5. 正向 VRP 和负向 VRP 的经济含义有何不同？

建议明确建立一个理论逻辑：

```text
VRP = risk-neutral expected volatility - physical expected volatility
```

其中：

- risk-neutral expected volatility 反映期权市场对未来风险的定价；
- physical expected volatility 反映实际市场波动预期；
- 二者差额反映投资者为承担波动风险所要求的补偿；
- 当 VRP 较高时，短中期可能代表风险补偿，因此对未来收益为正；
- 但在长期，如果高 VRP 主要来自过度恐慌或情绪性定价，则可能预示未来收益反转，因此系数转为负。

### 2.3 建议补充一段机制解释

可在 Introduction 或 Literature Review 末尾加入：

```text
Theoretically, the sign and horizon of VRP-based predictability depend on the source of volatility compensation. Over short and medium horizons, a high VRP may reflect compensation required by investors for bearing variance risk, leading to positive return predictability. Over longer horizons, however, an abnormally high VRP may capture sentiment-driven overpricing of downside protection or excessive demand for volatility hedging, which can be followed by return reversal. The horizon-dependent sign reversal therefore provides information about the transition from risk compensation to sentiment correction in China’s option market.
```

这会显著增强论文对 “term structure” 和 “sign reversal” 的理论解释。

### 2.4 HAR-RV 的理论贡献需要更聚焦

现在 HAR-RV 的引入主要是方法改进，但还可以说得更清楚：

- 传统 VRP 用 ex-post realized volatility 作为 physical volatility realization；
- 但 ex-post RV 包含噪声和短期冲击；
- HAR-RV 可以利用 daily/weekly/monthly volatility components 提取更稳定的 physical volatility expectation；
- 因此 HVRP 更接近 option-implied risk-neutral expectation 与 physical expected volatility 之间的差额。

建议明确：

```text
The HAR-RV refinement is not merely a forecasting device; it is intended to construct a more economically meaningful proxy for the physical expectation of future volatility.
```

---

## 3. 文献综述

### 3.1 当前文献综述的优点

`main_V2.tex` 的 Literature Review 比 `main.tex` 更规范，已分为三条线：

1. Theoretical Foundations and International Evidence；
2. The VRP Landscape in China’s Emerging Market；
3. Methodological Refinement and Research Gaps。

这个结构是合理的。

### 3.2 主要问题：文献与本文贡献的连接还需加强

目前每个小节基本能说明相关文献，但每个小节结尾应更明确地指出 “本文如何回应这个缺口”。建议每个小节最后增加一句：

- 国际 VRP 文献之后：  
  “However, whether these findings extend to China’s newly established ETF options market remains unclear.”

- 中国市场文献之后：  
  “Existing Chinese studies primarily examine the existence of VRP, while less attention has been paid to its horizon-dependent and asymmetric predictive content.”

- HAR-RV 方法文献之后：  
  “This motivates our use of HAR-RV to construct a refined physical-volatility benchmark for VRP measurement.”

### 3.3 文献综述中需要避免的问题

1. 不要把 HAR-RV 仅表述为 “解决 realized volatility noise” 的万能工具。HAR-RV 主要刻画 long memory 和多期限市场参与者结构，它可以改善 physical volatility 预测，但不自动消除所有噪声。
2. 中国市场文献需要更具体地区分：
   - 股指期货；
   - ETF 期权；
   - 沪深300指数期权；
   - 50ETF 期权；
   - CSI 300 ETF option。
3. 需要核查 2024 年文献 `qiao2024variance` 和 `qiao2024` 是否为同一文献或不同文献。目前 `ref.bib` 中同时出现：
   - `qiao2024variance`
   - `qiao2024`

   如果是同一篇，应合并；如果不同，应确保正文引用区分清楚。

### 3.4 参考文献格式问题

`ref.bib` 目前有若干条目不完整或格式不统一。例如：

- `carr2001volatilitytrading` 没有 journal；
- `corsi2012harmodeling` 信息不完整；
- `londono2011variance` 信息不完整；
- `hansen2010realizedgarch` 信息不完整；
- `li2022weak` 信息不完整；
- `qiao2024` 标为 Working Paper/Forthcoming，投稿前需确认状态；
- 部分中文文献英文翻译需要统一格式，并注明 “in Chinese”。

投稿前必须补全所有 BibTeX 信息，包括 journal、volume、issue、pages、publisher 或 working paper 信息。

---

## 4. 模型设定

### 4.1 Realized Volatility 定义

当前源码中 RV 定义为：

```text
RV_t = 100 × sqrt(252 × sum r_{t,i}^2)
```

这实际上是年化 realized volatility，而不是 realized variance。建议全文统一称为：

```text
annualized realized volatility
```

或明确：

```text
Although the notation RV is used, the variable is annualized realized volatility rather than realized variance.
```

否则读者会认为 RV 应该是 `sum r^2`，而不是开方年化后的波动率。

### 4.2 Semi-variances 的命名问题

当前定义：

```text
RV_t^+ = 100 × sqrt(252 × sum positive intraday squared returns)
RV_t^- = 100 × sqrt(252 × sum negative intraday squared returns)
```

这更准确地说是 positive/negative realized semivolatility，而非 semivariance。若文中使用 “semi-variances”，则公式不应开方；若保留开方公式，则建议改成：

```text
positive realized semi-volatility
negative realized semi-volatility
```

至少需要在文中说明为 annualized semivolatility。

### 4.3 Implied Volatility / MFIV 定义需更严格

当前源码中 “MFIV” 部分使用 model-free implied volatility 的思路，但需要检查以下问题：

1. 公式是否完整展示了 OTM call/put 的积分或离散化加总？
2. 是否说明如何处理不同到期日？
3. 是否使用最近月、次近月还是插值得到固定 maturity？
4. `IV^+` 和 `IV^-` 如何从 option prices 中定义？
5. 正向/负向 implied volatility 是否对应 upside/downside variance swap decomposition？
6. 是否区分 call-based upside variation 和 put-based downside variation？

如果不能严格定义 `IV^+` 和 `IV^-`，则 `VRP^+ = IV^+ - RV^+`、`VRP^- = IV^- - RV^-` 的经济解释会不够稳固。

建议增加更清楚的公式和文字说明。

### 4.4 VRP 定义问题

当前定义：

```text
VRP_t = IV_t - RV_t
```

这在直觉上可以接受，但学术文献中 VRP 常用 variance risk premium：

```text
VRP_t = IV_t^2 - E_t^P(RV_{t,t+h})
```

或 variance swap rate minus expected realized variance。你的公式使用 volatility difference 而非 variance difference，应明确说明：

```text
In this paper, VRP is measured as a volatility spread rather than a variance spread.
```

或改名为：

```text
volatility premium
```

如果目标期刊较严格，建议考虑使用 variance-based VRP 作为主结果或稳健性检验。

### 4.5 Predictive Regression Framework

当前模型：

```text
Ret_{t,t+h} = alpha_0 + alpha_1 X_t + beta' Control_t + epsilon_{t,t+h}
```

问题不大，但建议补充：

1. `Ret_{t,t+h}` 是累计简单收益还是对数收益？
2. h 的单位是 trading days 还是 calendar days？
3. h = 1, 7, 30, 60, 90, 120, 180, 360 是否对应交易日？
4. Newey-West lag length `h-1` 对 h=360 是否合适？
5. 是否对预测变量做标准化？
6. 是否控制 market return lag、realized volatility lag、momentum/reversal 等基础变量？

### 4.6 HAR-RV 模型设定

当前 HAR-RV：

```text
RV_{t+1} = beta_0 + beta_d RV_t + beta_w RV_{t,w} + beta_m RV_{t,m} + epsilon_{t+1}
```

建议明确：

1. HAR-RV 是用 expanding window、rolling window 还是 full sample 估计？
2. HVRP 中的 physical volatility 是 out-of-sample HAR forecast，还是 full-sample fitted value？
3. 如果使用 full-sample fitted value 构造 HVRP，再预测未来收益，可能存在 look-ahead bias。
4. 如果是滚动预测，应说明初始窗口、预测窗口和参数更新频率。
5. `RV_{t,w}` 和 `RV_{t,m}` 是否包含当天 `RV_t`，当前公式是包含的，需说明。

这是模型设定中最重要的问题之一：**HVRP 的构造必须避免未来信息泄露。**

---

## 5. 实证设计

### 5.1 样本选择

样本为 2020 年 1 月至 2025 年 10 月，研究对象为华泰柏瑞沪深300ETF及对应期权。

需要注意：

1. CSI 300 ETF options 2019 年底推出，样本开始于 2020 年 1 月是合理的；
2. 样本包含 COVID-19、2021–2022 结构调整、2024–2025 市场波动， regime 变化较大；
3. 应考虑是否做子样本稳健性，例如：
   - 2020–2021；
   - 2022–2023；
   - 2024–2025；
   - 或疫情前后/市场下跌期与恢复期。

### 5.2 5分钟高频数据

需要补充数据清洗细节：

- 是否删除开盘前后异常交易？
- 午休断点如何处理？
- 是否剔除停牌/半日交易？
- 5分钟收益如何从价格构造？
- 是否使用复权价格？
- ETF 分红除权是否处理？
- 日内缺失值如何补齐或删除？
- 是否 winsorize 极端 intraday returns？

现在只写 “handled outliers and contract rollover effects” 不够。对 ETF 来说不应写 “contract rollover effects”，因为 ETF 本身不是期货合约。这里应改为：

```text
we handled missing observations, abnormal quotes, and non-trading intervals
```

如果指的是 option contracts 的到期切换，则应明确说：

```text
option maturity selection and rollover
```

### 5.3 期权数据

当前写到 strike prices、open interest、mid-quote prices 来自 Wind。建议进一步说明：

1. 使用 call 和 put 还是只用 OTM options？
2. 如何处理无成交或报价异常的期权？
3. mid-quote 是 bid-ask midpoint 还是成交价？
4. 是否剔除深度虚值/深度实值期权？
5. 是否剔除到期日过近的期权？
6. 无风险利率和股息率如何设定？
7. 如何插值得到固定期限 implied volatility 或 MFIV？
8. 是否考虑交易成本和 bid-ask spread？

这些细节对 option-implied 指标非常关键。

### 5.4 控制变量定义

当前控制变量表包括：

- `IV_MEAN`
- `VOLD`
- `IVD`
- `VIX`

但需要进一步统一：

1. `VIX` 是中国市场哪一个 VIX-like 指标？如果是自己构造的 model-free implied volatility，不宜直接叫 VIX。
2. `VOLD` 的定义为 call-put volume spread，需要说明是否标准化。
3. `IVD` 是 call-put implied volatility difference，还是 skewness measure？建议称为 implied-volatility skew。
4. 控制变量是否滞后一期？必须避免使用与未来收益窗口重叠的信息。

### 5.5 预测期限设计

当前 horizon 包括：

```text
1, 7, 30, 60, 90, 120, 180, 360
```

这是一个很宽的期限结构。建议解释为何选择这些 horizons：

- 1 天：短期；
- 7 天：周度；
- 30/60/90 天：月度/季度；
- 120/180 天：中长期；
- 360 天：一年左右。

但如果 h 是 trading days，360 trading days 超过一年；如果是 calendar days，则需要明确。

### 5.6 Newey-West 标准误

使用 overlapping returns 时 Newey-West 是正确方向。但对于 h=360，lag length = h-1 可能非常大，而样本只有约 1409 个观测，可能导致推断不稳定。建议：

1. 说明为什么使用 `h-1`；
2. 报告稳健性：如 Andrews automatic bandwidth 或固定较短 lag；
3. 对超长期 horizon 的结果谨慎解释。

---

## 6. 结果呈现

### 6.1 描述性统计

描述性统计表有用，但建议增加：

1. N 的解释：为什么 Count = 1409；
2. VRP 为正的比例；
3. VRP 极端负值的来源；
4. 是否存在 outlier winsorization；
5. `VRP+` 和 `VRP-` 的均值差异是否有经济解释。

### 6.2 图表问题

源码中图片包括：

- `r2.png`
- `eng r2.png`
- `correlation.png`
- `image.png`

`main_V2.tex` 只使用了 `r2.png`。建议：

1. 删除或移出未使用图片，避免 Overleaf 项目混乱；
2. 图片文件名避免空格，例如 `eng_r2.png`；
3. 增加 correlation heatmap 或 scatter plot 时，要在正文中解释；
4. `r2.png` 应确保分辨率足够，字体可读，横纵坐标清楚；
5. 图题中说明 baseline VRP 与 HAR-VRP 的比较含义。

### 6.3 表格 label 重复

`main_V2.tex` 中 `\label{result1}` 出现两次：

- 第一个表：Results of the univariate baseline regression model；
- 第二个表：Baseline Univariate Regression Results。

这是必须修复的问题。否则 `Table \ref{result1}` 的引用会混乱。

建议改为：

```text
\label{tab:univariate_vrp}
\label{tab:univariate_vrp_short}
```

或者删除重复表，保留其中一个。

### 6.4 中文 label

`main_V2.tex` 中存在：

```text
\label{描述性统计}
```

虽然某些环境可编译，但不建议投稿源码中使用中文 label。建议改为：

```text
\label{tab:desc_stats}
```

同时正文引用改成：

```text
Table \ref{tab:desc_stats}
```

### 6.5 表格标题和变量名

建议统一标题风格：

- `Results of the univariate baseline regression model`
- `Baseline Univariate Regression Results`
- `Multivariate Predictive Regression: Pricing Volatility Risk`
- `Predictive Regression Results for the HAR-Refined VRP`

建议改为更一致的标题：

```text
Table 1. Variable Definitions
Table 2. Descriptive Statistics
Table 3. Univariate Predictive Regressions Using VRP
Table 4. Multivariate Predictive Regressions Using VRP
Table 5. Univariate Predictive Regressions Using HAR-Refined VRP
Table 6. Multivariate Predictive Regressions Using HAR-Refined VRP
```

### 6.6 结果解释

当前结果解释中有一些较强表述，例如：

- “dual-role hypothesis”
- “sentiment-driven overreaction”
- “forward-looking expectations embedded in option prices are the primary drivers”

这些解释有启发性，但需要小心，因为回归本身主要证明预测相关性，不一定能识别情绪过度反应机制。建议使用更谨慎表达：

```text
This pattern is consistent with a transition from risk compensation at medium horizons to possible sentiment correction at longer horizons.
```

---

## 7. 英文表达

### 7.1 标题建议

当前标题：

```text
Pricing Volatility Risk in China: A HAR-RV Refined Approach to Term Structure and Asymmetric Predictability
```

优点是覆盖全面，但略长，且 “A HAR-RV Refined Approach to Term Structure” 英语不够自然。

建议题目选项：

1. **Volatility Risk Premium and Return Predictability in China: Evidence from CSI 300 ETF Options**

2. **Pricing Volatility Risk in China: Term-Structure and Asymmetric Evidence from CSI 300 ETF Options**

3. **A HAR-RV Refined Volatility Risk Premium and Return Predictability in China**

4. **Volatility Risk Premium, Term Structure, and Asymmetric Return Predictability in China’s ETF Options Market**

如果突出方法创新，选 3；如果突出市场和实证发现，选 4。

### 7.2 摘要问题

摘要目前较完整，但有几个问题：

1. “pricing of volatility risk” 与 “return predictability” 之间需要更清楚连接；
2. “HAR-RV model to optimize VRP construction” 中 optimize 可能过强，建议用 refine；
3. 应明确 `VRP = IV - RV` 是 volatility spread；
4. 应说明样本是 CSI 300 ETF options，而不是泛泛的 Chinese market；
5. 应突出核心发现：term-structure sign reversal、positive/negative asymmetry、HAR-refined improvement。

建议摘要结构：

```text
This paper studies the volatility risk premium (VRP) in China’s CSI 300 ETF options market and examines its term-structure and asymmetric return predictability. Using 5-minute intraday ETF returns and daily option data from January 2020 to October 2025, we construct model-free implied volatility, realized volatility, and positive and negative VRP components. The baseline predictive regressions show that the VRP exhibits horizon-dependent predictability: it is positively associated with future returns at medium horizons but becomes significantly negative at longer horizons. We further refine the physical-volatility component using a HAR-RV model and show that the HAR-refined VRP improves explanatory power, especially at long horizons. Decomposed results reveal that upside volatility-risk components contain more persistent predictive information than downside components. These findings provide new evidence on volatility-risk pricing and option-implied information in China’s emerging derivatives market.
```

### 7.3 常见表达修改

建议替换以下表达：

| 原表达 | 建议表达 |
|---|---|
| optimize VRP construction | refine VRP measurement |
| pricing volatility risk in the Chinese market | volatility-risk pricing in China’s ETF options market |
| ultra-long horizon | long horizon |
| high-fidelity laboratory | useful setting / natural setting |
| radical shifts in risk-neutral expectations | sharp changes in risk-neutral expectations |
| term structure characteristics | term-structure properties |
| asymmetric effects | asymmetric predictive effects |
| the primary drivers | important drivers / key sources |
| realized price fluctuations far exceed hedging costs | realized volatility exceeds the volatility compensation priced in options |

### 7.4 语气问题

当前部分语句偏宣传式，例如：

```text
high-fidelity laboratory
profound global instability
sophisticated hedging and pricing mechanisms
exceptional stability
```

建议改为更克制的学术表达。实证金融论文通常越克制越有说服力。

---

## 8. 投稿准备

### 8.1 LaTeX 源码层面必须修改

1. **确定主稿文件**  
   建议明确只保留 `main_V2.tex` 为主稿，`main.tex` 改名为 `main_old.tex` 或移入 archive。

2. **恢复作者信息**  
   `main_V2.tex` 中 `\author{...}` 被 `comment` 环境注释掉了，编译时会出现 “No author given”。投稿前必须恢复作者信息，或按匿名审稿要求设置 anonymous title page。

3. **修复重复 label**  
   `\label{result1}` 重复出现，必须改。

4. **替换中文 label**  
   `\label{描述性统计}` 改为 `\label{tab:desc_stats}`。

5. **清理未使用图片**  
   如果最终只使用 `r2.png`，其余图片可移入 `figures/archive/`，避免混乱。

6. **图片文件名去空格**  
   `eng r2.png` 改为 `eng_r2.png`。

7. **统一参考文献系统**  
   `main_V2.tex` 已用 `natbib + ref.bib`，建议保留；`main.tex` 的手写 bibliography 不再作为主稿。

8. **补全 BibTeX 条目**  
   检查 working paper、中文文献、HAR-RV 文献、MFIV 文献的 volume/pages/DOI。

9. **检查引用命令风格**  
   源码中有 `{\cite{...}}`，建议改为 `\citet{...}` 或 `\citep{...}`：
   - 主语引用：`\citet{bollerslev2009} show that...`
   - 括号引用：`... risk premium \citep{bollerslev2009}.`

10. **检查编译环境**  
   我尝试编译 `main_V2.tex`，PDF 可初步生成，但由于当前环境缺少 `bibtex` 命令，参考文献未完整解析；Overleaf 中通常可正常处理。源码本身仍需修复重复 label、作者注释、中文 label 和引用格式问题。

### 8.2 投稿内容层面必须检查

1. 目标期刊是否允许 1.5 倍行距；
2. 是否需要匿名稿；
3. 是否需要 title page 单独文件；
4. 是否需要 JEL codes；
5. 是否需要 Data Availability Statement；
6. 是否需要 Conflict of Interest；
7. 是否需要 Funding Statement；
8. 是否需要 CRediT author contribution；
9. 是否需要 Declaration of generative AI use；
10. 参考文献格式是否使用 APA、Harvard 或期刊指定格式。

### 8.3 可能被审稿人追问的问题

1. **为什么使用 volatility spread 而不是 variance risk premium？**  
   需要解释或增加 variance-based robustness。

2. **HAR-RV refined VRP 是否存在 look-ahead bias？**  
   必须明确 HVRP 是滚动预测构造，不能用全样本 fitted RV。

3. **IV+ 和 IV− 如何定义？**  
   需要严格说明上行/下行 implied volatility 的 option-based 构造方法。

4. **长期 h=360 的预测结果是否稳健？**  
   因为 overlapping returns 和样本长度有限，长期预测需要谨慎。

5. **sign reversal 的经济机制是什么？**  
   需要更清楚地区分 risk compensation 和 sentiment correction。

6. **中国 ETF option 市场流动性是否足够？**  
   需要补充 option liquidity filter 或控制变量。

---

## 9. 按八个维度的修改清单

### 9.1 论文结构

- [ ] 以 `main_V2.tex` 为主稿。
- [ ] 增加 asymmetric predictability 专门小节。
- [ ] 强化 Introduction 末尾 roadmap。
- [ ] 将 HAR-RV 部分从“方法附加”改写为“VRP 构造改进”。
- [ ] 删除或归档旧版 `main.tex`，避免版本混乱。

### 9.2 理论贡献

- [ ] 明确 VRP 预测收益的理论机制。
- [ ] 解释中期正向、长期负向的 sign reversal。
- [ ] 说明 HAR-RV refinement 的经济含义，而非只说预测效果。
- [ ] 解释正向/负向 VRP 的差异和 “asymmetric predictability”。

### 9.3 文献综述

- [ ] 每个文献小节结尾增加本文研究缺口。
- [ ] 区分国际 VRP、中国市场 VRP、ETF option、HAR-RV 方法。
- [ ] 合并或区分 `qiao2024variance` 和 `qiao2024`。
- [ ] 补全不完整 BibTeX 条目。
- [ ] 中文文献英文翻译格式统一。

### 9.4 模型设定

- [ ] 说明 RV 是 annualized volatility 还是 variance。
- [ ] 若使用开方公式，应避免称为 semivariance。
- [ ] 严格定义 MFIV、IV+、IV−。
- [ ] 解释为何使用 `VRP = IV - RV` 而非 variance-based VRP。
- [ ] 明确 HAR-RV refinement 是否 out-of-sample。
- [ ] 防止 HVRP 构造中的 look-ahead bias。

### 9.5 实证设计

- [ ] 补充 ETF 高频数据清洗规则。
- [ ] 删除 “contract rollover effects” 或改为 option maturity rollover。
- [ ] 说明 option filters、maturity selection、bid-ask midpoint。
- [ ] 解释 h = 1, 7, 30, 60, 90, 120, 180, 360 的选择。
- [ ] 检查 h=360 下 Newey-West lag 的合理性。
- [ ] 考虑子样本或市场状态稳健性。

### 9.6 结果呈现

- [ ] 修复重复 label `result1`。
- [ ] 将中文 label 改为英文。
- [ ] 统一表格标题风格。
- [ ] 增加或保留清晰的 R² 比较图。
- [ ] 对 sign reversal 和 asymmetry 增加经济解释。
- [ ] 避免把相关性解释成强因果。

### 9.7 英文表达

- [ ] 题目可进一步精炼。
- [ ] 摘要压缩并突出核心发现。
- [ ] 减少宣传式表达，如 “high-fidelity laboratory”。
- [ ] 将 “optimize” 改为 “refine”。
- [ ] 将 “ultra-long horizon” 改为 “long horizon”。
- [ ] 统一 “term-structure predictability”“asymmetric predictability”“HAR-refined VRP”。

### 9.8 投稿准备

- [ ] 恢复作者信息或设置匿名稿。
- [ ] 检查是否需要 title page。
- [ ] 补全 `ref.bib`。
- [ ] 检查图表分辨率。
- [ ] 删除未使用文件或放入 archive。
- [ ] 增加 JEL codes。
- [ ] 增加 Data Availability Statement。
- [ ] 增加 Conflict of Interest / Funding Statement。
- [ ] 按目标期刊格式调整引用和参考文献。

---

## 10. 最高优先级问题

如果时间有限，建议最先处理以下 10 件事：

1. 确定 `main_V2.tex` 为主稿；
2. 恢复或按匿名要求处理 author；
3. 修复重复 label `result1`；
4. 将 `\label{描述性统计}` 改为英文 label；
5. 明确 RV 是 volatility 还是 variance；
6. 严格定义 `IV+`、`IV-`、`VRP+`、`VRP-`；
7. 说明 HAR-refined VRP 是否避免 look-ahead bias；
8. 替换 ETF 数据部分中的 “contract rollover effects”；
9. 补全 `ref.bib` 中不完整条目；
10. 在 Introduction 中更明确地写出本文三点贡献。

---

## 11. 总结

这份 Overleaf 源码的 `main_V2.tex` 已经具备较完整的英文论文框架，主题也较明确：利用 CSI 300 ETF options 和 5分钟高频数据研究中国市场 VRP 的期限结构和非对称收益预测能力，并用 HAR-RV 改进 physical volatility 估计。

当前最需要加强的不是增加更多回归，而是：

1. **把理论机制说清楚**：为什么 VRP 预测收益，为什么存在期限结构反转，为什么正负 VRP 有不同预测力；
2. **把变量定义说严谨**：尤其是 RV、semi-volatility、MFIV、IV+、IV-、VRP+、VRP-；
3. **把源码问题修干净**：重复 label、中文 label、作者注释、参考文献不完整、旧版文件混杂；
4. **把投稿格式补齐**：匿名稿、JEL、声明、参考文献、图表质量。

完成这些修改后，这篇论文会更像一篇成熟的实证资产定价/衍生品市场论文，而不仅是一个 VRP 预测回归练习。
