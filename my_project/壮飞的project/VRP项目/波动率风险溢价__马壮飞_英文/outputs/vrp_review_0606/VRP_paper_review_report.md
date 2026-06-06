# 波动率风险溢价论文审稿报告与修改方案

审稿身份：波动率预测、衍生品信息含量与收益率可预测性方向审稿人  
审稿对象：`波动率风险溢价__马壮飞__0606.pdf`，并参考当前目录下可读源码 `main_V2.tex` 与关键表格页渲染结果  
总体判断：论文有明确经验事实和可继续打磨的研究问题，但目前应按 **Major Revision** 处理。核心原因不是选题不成立，而是“VRP 构造、预测时点、期限匹配、HAR-VRP 样本外优越性证明”四个环节还没有形成足够严密的证据闭环。

## 一、论文当前逻辑线

论文的实际逻辑可以概括为一条“期权隐含风险预期如何预测未来 ETF 收益”的经验研究线：

1. **研究背景**：CSI 300 ETF 期权于 2019 年底上市，中国官方 iVIX 已停止发布，因此需要自行构造隐含波动率和 VRP 指标。
2. **核心问题**：期权市场中的波动率风险溢价是否能预测 CSI 300 ETF 未来收益？这种预测能力是否随预测期限变化？上下行波动率风险是否具有不对称信息？
3. **变量构造**：用 5 分钟高频收益构造日度 RV、上行 RV 与下行 RV；用期权价格构造模型无关隐含波动率 MFIV、上行 IV 与下行 IV；再构造 `VRP = IV - RV` 及其分解项。
4. **基准实证**：用 1、7、30、60、90、120、180、360 日未来收益为因变量，做单变量与多变量预测回归，并用 Newey-West HAC 标准误处理重叠收益。
5. **主要发现**：VRP 在 30-90 日窗口正向预测收益，在 120 日及以上窗口转为显著负向；上行和下行 VRP 分解项在超长窗口表现不同。
6. **方法扩展**：引入 HAR-RV 模型预测物理测度下的 RV，构造 `HVRP = IV - RV_HAR`，声称 HAR-VRP 在中长期预测上优于传统 VRP。
7. **模型比较**：PDF 中额外出现了样本外 MSE、`R^2_{OOS}`、DM 和 MCS 检验，但正文解释和表注明显不足。
8. **结论**：论文将中期正向预测解释为风险补偿，将长期负向预测解释为市场情绪修正或均值回归，并强调 HAR-RV 改进 VRP 构造。

这条主线是成立的，但现在的问题是：每个关键断点都需要更严格的定义、时点说明和稳健性验证，否则“经验现象”很容易被审稿人质疑为变量构造或重叠收益带来的统计假象。

## 二、值得保留和强化的亮点

1. **选题有现实价值**：CSI 300 ETF 期权样本从 2020 到 2025，覆盖疫情冲击、市场转折和衍生品市场发展期，确实适合研究中国 ETF 期权信息含量。
2. **期限结构结果有辨识度**：VRP 在 30-90 日正向、120 日以上负向的“符号反转”是全文最有潜力的发现，应作为论文标题、摘要和引言的主结果。
3. **上下行分解是正确方向**：用 semivariance 区分 upside/downside volatility risk，有助于对接 Bollerslev, Patton, and Quaedvlieg (2014) 的好/坏波动率框架。
4. **HAR-RV 的引入合理**：如果处理好样本外预测和期限匹配，HAR 过滤物理波动率的思路可以成为方法贡献。
5. **已有模型比较雏形**：PDF 中出现 `R^2_{OOS}`、DM、MCS，说明作者已经意识到只比较样本内 `Adj. R^2` 不够；这部分应成为修改后的关键证据。

## 三、主要问题与修改方向

### 问题 1：VRP 定义与文献中的 variance risk premium 不完全一致

当前论文使用：

```tex
VRP_t = IV_t - RV_t
```

其中 `IV` 和 `RV` 都是年化波动率单位。严格来说，Bollerslev et al. (2009) 和 Carr and Wu (2009) 文献中更常见的是 **risk-neutral expected variance 减 physical expected variance**，也就是方差单位或预期未来实现方差，而不是简单的当期隐含波动率减当期实现波动率。

**为什么重要**：如果论文一方面引用 variance risk premium 文献，一方面又用 volatility spread，审稿人会质疑变量命名、单位、经济含义和符号解释是否混在一起。

**建议修改**：

1. 若保持现有构造，应把核心变量称为 **volatility premium measure** 或 **volatility risk premium in volatility units**，并明确这是一种经验代理变量。
2. 作为稳健性实验，必须补充方差单位版本：

```tex
VRP^{var}_t = IV^2_t - \widehat{E}^P_t(RV_{t,t+30})
```

3. 如果仍写“pricing volatility risk”，需要在方法部分说明：本文的基准变量是波动率单位，稳健性部分用方差单位复核结论。

### 问题 2：IV 的期限与 RV/HAR-RV 的期限没有完全匹配

论文中 MFIV 被线性插值到 30 天，但 RV 是日度 5 分钟实现波动率；HAR-RV 也是一日 ahead 形式：

```tex
RV_{t+1} = beta_0 + beta_d RV_t + beta_w RV_{t,w} + beta_m RV_{t,m} + epsilon_{t+1}
```

**为什么重要**：30 日隐含波动率对应的是未来约 30 日风险中性预期，而日度 RV 或一日 HAR forecast 与之期限不同。期限错配会削弱 VRP 的经济含义。

**建议修改**：

1. 基准 VRP 至少构造成 30 日物理期望：

```tex
\widehat{RV}^{(30)}_{t} =
\frac{1}{30}\sum_{j=1}^{30}\widehat{RV}_{t+j|t}
```

2. 如果数据和代码暂时只能用日度 RV，则应在正文中承认它是 **short-run realized volatility proxy**，并把“风险溢价”表述降级为“option-implied volatility minus recent physical volatility”。
3. HAR 部分应改成预测 30 日聚合 RV，而不是只用一日预测值。

### 问题 3：HAR-RV 是否存在全样本估计造成的未来信息泄露

源码没有交代 HAR-RV 的估计方式：是全样本回归后生成 fitted RV，还是滚动/递归预测？

**为什么重要**：如果用 2020-2025 全样本估计 HAR 系数，再把 fitted value 用于早期样本的 HVRP，那么 HAR-VRP 的“改进”包含未来数据的信息，不能声称预测表现更好。

**建议修改**：

1. 样本内描述可以保留全样本 HAR 拟合，但不能把它作为预测证据。
2. 样本外检验必须使用 rolling 或 expanding window：

```tex
For each day t, the HAR-RV model is estimated using information available up to t only. The resulting forecast is then used to construct HVRP_t.
```

3. 论文中所有 “outperforms” 表述必须基于样本外 `R^2_{OOS}`、DM/Clark-West 或 MCS，而不是样本内 `Adj. R^2`。

### 问题 4：长期重叠收益的统计推断仍然偏乐观

论文已经使用 Newey-West HAC，lag length = h-1，这是必要的。但当 h=180 或 360 时，重叠收益极其严重，1409 个日度样本的有效独立观测远低于 1409。

**为什么重要**：收益预测文献中，长期 horizon 的 t 值经常被重叠收益和持久预测变量放大。

**建议修改**：

1. 保留 Newey-West，但补充至少两种推断稳健性：
   - 非重叠月度/季度抽样回归；
   - moving-block bootstrap 或 stationary bootstrap；
   - fixed-b / Hodrick 型标准误。
2. 对 8 个预测期限和多组变量做多重检验调整，例如 Benjamini-Hochberg FDR 或 Romano-Wolf stepdown。
3. 把 360 日结论写得更谨慎，避免“highly robust”。

### 问题 5：经济机制解释超出当前证据

论文将 30-90 日正向关系解释为风险补偿，将 120 日以上负向关系解释为情绪修正和均值回归。这是合理猜想，但目前没有机制检验。

**建议修改**：

1. 增加状态依赖回归：

```tex
Ret_{t,t+h} = alpha + beta_1 VRP_t + beta_2 VRP_t \times HighSentiment_t
              + beta_3 VRP_t \times HighLiquidity_t + controls + epsilon
```

2. 分组检验：高/低 VIX、高/低换手率、牛市/熊市、疫情期/非疫情期。
3. 如果暂时不做机制检验，结论中应把“reflects”改为“is consistent with”。

### 问题 6：模型比较部分需要重写

PDF 第 21-24 页显示有样本外 MSE、`R^2_{OOS}`、DM 和 MCS，但表注和文字解释不够完整。表 11 中模型 1-4 的含义不清；表 12 DM 检验的正负方向、p 值含义、benchmark 都没有讲清。

**建议修改**：

1. 给出模型编号：
   - Model 1: baseline VRP model
   - Model 2: decomposed VRP model with upside/downside components
   - Model 3: HAR-VRP model
   - Model 4: decomposed HAR-VRP model
2. 明确样本外切分。例如：

```tex
The first 80% of the sample is used for initial estimation and the remaining 20% for recursive out-of-sample evaluation.
```

3. 明确 `R^2_{OOS}` 的 benchmark：

```tex
R^2_{OOS}=1-\frac{MSPE_{model}}{MSPE_{historical\ mean}}.
```

4. 对 DM 检验补充方向说明：正的 DM 是否表示前者 loss 更大？p 值是否在括号中？粗体是否表示 5% 显著？
5. 对嵌套模型比较，优先使用 Clark-West；DM 更适合非嵌套模型。

### 问题 7：上下行 VRP 的命名和经济含义需要更清楚

`VRP^+` 和 `VRP^-` 容易被误解为“正的/负的 VRP 数值”，但实际是上行和下行 semivariance premium。

**建议修改**：

1. 全文改成 `VRP^{up}` 与 `VRP^{down}`，或者至少第一次出现时写：

```tex
We use the superscripts + and - to denote upside and downside semivariance components, rather than positive and negative numerical values.
```

2. 在表格中把 `Positive VRP` 改为 `Upside VRP`，`Negative VRP` 改为 `Downside VRP`。
3. 对 `HVRP^+` 在 180/360 日系数绝对值很大的结果，要检查尺度是否与 `VRP^+` 一致，并补充标准化系数。

### 问题 8：缺少经济显著性

现在表格只报告系数和 t 值。读者不知道 `VRP` 增加一个标准差，会导致未来收益变化多少。

**建议修改**：

1. 所有核心变量增加标准化回归：

```tex
Ret_{t,t+h} = alpha + beta z(VRP_t) + controls + epsilon
```

2. 报告一标准差 VRP 冲击对未来 30、90、180 日累计收益的影响，单位用百分点。
3. 加一张核心图：横轴为 h，纵轴为标准化 beta，阴影为 95% CI。它会比大表更有说服力。

## 四、建议重构后的论文结构

建议把论文改成“主问题 - 主事实 - 方法改进 - 样本外验证”的结构：

1. Introduction  
   直接提出 puzzle：CSI 300 ETF options 中的 VRP 是否预测收益？为何预测符号会随期限反转？
2. Hypotheses  
   H1: VRP predicts future returns.  
   H2: predictability is horizon-dependent.  
   H3: upside/downside semivariance premia have asymmetric predictive content.  
   H4: HAR-filtered physical volatility improves out-of-sample prediction.
3. Data and Variable Construction  
   详细说明期权过滤、MFIV、RV、semivariance、HAR forecast timing。
4. In-Sample Predictive Evidence  
   单变量、多变量回归，突出 sign reversal。
5. Asymmetry and Economic Interpretation  
   上下行 VRP 同时进入模型，做 Wald test。
6. Out-of-Sample Forecasting and Model Comparison  
   rolling/expanding window，`R^2_{OOS}`，Clark-West/DM，MCS。
7. Robustness and Mechanism  
   方差单位、不同 RV、子样本、非重叠、block bootstrap、状态依赖。
8. Conclusion  
   更克制地总结。

## 五、可直接替换的英文改写稿

### 1. 摘要改写稿

```tex
This paper examines whether the volatility premium embedded in CSI 300 ETF options predicts future ETF returns in China's equity market. Using 5-minute intraday returns and daily option quotes from January 2020 to October 2025, we construct model-free implied volatility, realized volatility, upside and downside semivariance measures, and a HAR-RV-filtered volatility premium. The evidence reveals a pronounced horizon-dependent pattern: the VRP is positively associated with future returns over 30- to 90-day horizons, but the sign reverses and becomes significantly negative at horizons of 120 days and beyond. Decomposing the premium shows that upside and downside volatility components contain different information, with downside-premium predictability decaying more rapidly at ultra-long horizons. The HAR-RV refinement improves medium- to long-horizon fit and should be evaluated further through recursive out-of-sample tests. Overall, the results suggest that option-implied volatility information in China's ETF option market contains both risk-compensation and mean-reversion components, but the strength of this interpretation depends on careful treatment of horizon matching and forecast timing.
```

如果补完样本外检验，并且 Clark-West/DM/MCS 支持 HAR-VRP，可把最后一句改为：

```tex
Recursive out-of-sample tests further show that the HAR-RV-filtered premium delivers lower forecast errors than the conventional VRP benchmark at medium and long horizons.
```

### 2. 引言研究缺口段落改写稿

```tex
The key unresolved issue is not simply whether volatility risk is priced in China, but whether the information contained in ETF options predicts the underlying return at economically relevant horizons. Existing studies mainly focus on SSE 50 ETF options or broad index-level volatility measures, and they rarely distinguish between horizon-specific predictability, upside and downside volatility components, and the physical-volatility forecast used to construct the premium. This distinction is important because a 30-day option-implied volatility measure should be compared with a physical expectation over a comparable horizon; otherwise, the resulting premium may mix option-market expectations with short-run realized-volatility noise. This paper addresses this gap by studying the CSI 300 ETF option market from its inception, decomposing the VRP into upside and downside components, and evaluating whether a HAR-RV-filtered physical-volatility forecast improves return predictability.
```

### 3. 贡献段落改写稿

```tex
This paper contributes to the literature in three ways. First, it provides new evidence on the term structure of return predictability in China's CSI 300 ETF option market, documenting a sign reversal from positive predictability at 30- to 90-day horizons to negative predictability beyond 120 days. Second, it extends the good-volatility/bad-volatility framework to China's ETF option market by separating upside and downside semivariance premia and comparing their predictive content across horizons. Third, it evaluates whether replacing noisy realized volatility with a HAR-RV-based physical-volatility forecast produces a cleaner volatility-premium signal. The last contribution is primarily empirical and requires formal out-of-sample validation, which we implement through rolling forecasts, out-of-sample R-squared, and forecast comparison tests.
```

### 4. 变量构造段落改写稿

```tex
To align the option-implied and physical components of the premium, the baseline implied-volatility measure is interpolated to a constant 30-day maturity. The physical component is constructed from high-frequency realized volatility and, in the HAR-refined specification, from forecasts generated using only information available up to day t. Throughout the paper, predictors are dated at the end of day t and future returns are measured over the interval from t+1 to t+h, thereby avoiding mechanical look-ahead bias.
```

如果暂时没有 30 日 HAR 聚合预测，应写得更保守：

```tex
Because the current physical component is based on recent realized volatility rather than a full 30-day physical expectation, the resulting measure should be interpreted as an empirical volatility-premium proxy. We therefore report robustness checks based on variance-unit and horizon-matched alternatives.
```

### 5. 预测回归与推断段落改写稿

```tex
Because the dependent variable is an overlapping h-day cumulative return, conventional OLS standard errors are inappropriate. We therefore report Newey-West HAC t-statistics with h-1 lags in the baseline specification. For long horizons, where overlap is severe, we further verify the results using non-overlapping returns, block-bootstrap confidence intervals, and alternative HAC lag choices. These additional checks are important because long-horizon return predictability is sensitive to persistence in the predictor and to the effective number of independent observations.
```

### 6. HAR-VRP 段落改写稿

```tex
The HAR-RV model is used to extract the persistent component of physical volatility rather than to mechanically increase in-sample fit. For each forecast origin t, the HAR-RV coefficients are estimated recursively using data available up to t, and the resulting forecast is used to construct HVRP_t. This recursive design ensures that the HAR-refined premium can be interpreted as a feasible real-time predictor.
```

### 7. 模型比较段落改写稿

```tex
To assess whether the HAR-RV refinement improves genuine forecast performance, we conduct a recursive out-of-sample evaluation. The first 80% of the sample is used as the initial estimation window, and forecasts are produced for the remaining 20% of observations. We compare four models: the baseline VRP model, the decomposed VRP model, the HAR-VRP model, and the decomposed HAR-VRP model. Forecast accuracy is evaluated using MSPE and out-of-sample R-squared relative to the historical-mean benchmark. Pairwise differences in forecast loss are tested using Diebold-Mariano statistics for non-nested comparisons and Clark-West statistics for nested comparisons. Finally, the Model Confidence Set procedure identifies the set of models that cannot be statistically rejected as inferior at conventional confidence levels.
```

### 8. 结论改写稿

```tex
The findings should be interpreted as evidence of horizon-dependent option-market information rather than as a single monotonic volatility-risk channel. The positive VRP-return relation at medium horizons is consistent with risk compensation, whereas the negative relation at longer horizons is consistent with sentiment correction or mean reversion. Future work should distinguish these mechanisms more directly by incorporating sentiment, liquidity, margin constraints, and market-regime interactions.
```

## 六、必须补充的实验清单

| 优先级 | 实验 | 具体做法 | 作用 |
|---|---|---|---|
| 必做 | 期限匹配 VRP | 构造 30 日物理 RV 期望，或 30 日未来实现 RV 的预测值 | 修复 IV 与 RV 期限错配 |
| 必做 | 递归 HAR-VRP | rolling/expanding HAR，只用 t 日及以前数据预测 | 排除未来信息泄露 |
| 必做 | 样本外预测 | 80/20 或滚动窗口，报 MSPE、`R^2_{OOS}` | 支撑 HAR-VRP “优于” VRP |
| 必做 | Clark-West/DM/MCS | 嵌套用 Clark-West，非嵌套用 DM，最后做 MCS | 给模型优劣正式统计证据 |
| 必做 | 非重叠/Bootstrap 推断 | 非重叠月度样本、moving-block bootstrap | 修复长期重叠收益 t 值偏误 |
| 必做 | 方差单位 VRP | `IV^2 - E^P(RV)` 与现有波动率单位比较 | 对齐经典 VRP 文献 |
| 强烈建议 | 标准化经济显著性 | 一标准差 VRP 冲击对应未来收益百分点变化 | 让结果可解释 |
| 强烈建议 | 上下行同时进入回归 | `VRP^up` 与 `VRP^down` 同时进入，做 Wald test | 证明不对称性不是单独回归假象 |
| 强烈建议 | 替代 RV | 10/15/30 分钟 RV、realized kernel、bipower variation | 排除微观结构噪声 |
| 强烈建议 | 状态依赖 | 高/低 VIX、牛熊市、疫情期、流动性分组 | 支撑风险补偿 vs 情绪修正机制 |
| 可选 | 替代模型 | HARQ、HAR-CJ、Realized GARCH、GARCH-MIDAS | 强化“HAR 是合理基准”的说服力 |
| 可选 | 横向资产 | SSE 50 ETF、CSI 500 ETF、ChiNext ETF options | 拓展外部有效性 |

## 七、PPT 汇报建议

20 分钟汇报不应逐表朗读，而要围绕审稿人最关心的“可信度链条”展开：

1. 2 分钟：研究问题和一句话判断。
2. 3 分钟：论文逻辑线和当前贡献。
3. 5 分钟：核心实证事实，尤其是 VRP 期限结构反转和上下行不对称。
4. 4 分钟：HAR-VRP 改进思路及当前证据不足。
5. 4 分钟：审稿式主要问题与补充实验。
6. 2 分钟：修改路线图和结论。

汇报中最重要的一句话应是：

> 这篇论文最有价值的不是“VRP 显著”，而是发现了 CSI 300 ETF 期权市场中 VRP 预测收益的期限结构反转；但要让这个发现站得住，必须补齐 VRP 定义、期限匹配、实时预测和样本外比较四个环节。

