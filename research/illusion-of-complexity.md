# 复杂性的幻觉：我用 AI 做了四组研究，朴素基线却一次次提醒我

*The Illusion of Complexity · 2026-11-26 · 鲜唯佳 / Weijia Xian*

过去我一直在找一个问题的答案：AI 能不能从大量数据里挖出人类没有发现的东西？

我让 AI 研究过股票、比特币、地震和城市收入。今天重新翻看这些旧实验，发现一个共同点：最值得保留下来的，有时不是模型的漂亮结果，而是它遇到朴素基线时的表现。

## 01｜AI 分析了半天，「永远猜涨」反而赢了

我有一份历史交易预测台账，固定保存了 66 条 AI 预测，其中 48 条有独立行情复核记录。本次只选取已给出明确上涨或下跌方向、且已有期末行情方向比较的 26 条记录。按「资产 × 原依赖/重复组」合并，每组选取第一条有方向评分的记录，剩下 19 组。

| 口径 | AI 方向命中 | 始终预测上涨 |
| --- | ---: | ---: |
| 原始 26 条 | 17/26（65.4%） | 20/26（76.9%） |
| 合并重复后的 19 组 | 12/19（63.2%） | 14/19（73.7%） |

使用最后一条可评分记录替代第一条，结论数字不变。2025 年 4 月 17 日的五条比特币看多判断均与期末方向一致，但不能当成五次独立成功。

去重后的 19 组中，AI 给出 17 个看涨判断（其中 12 个上涨），以及 2 个看跌判断（两个实际均上涨）。所以这个样本里的差距来自两次错误看跌，不是复杂的收益模型。

这不是「AI 不会炒股」的证明。样本很小，资产和预测窗口仍可能重叠，记录不是随机抽样；未评分的 Hold、区间预测和缺行情记录不计入。方向命中也不等于真实收益，未重建仓位、成交、止损和成本。

但它至少让我明白：如果连一个不需要 AI 的基准都没超过，就暂时别把复杂分析称为预测优势。

## 02｜一个地震预测信号，在两项检验中失去了优势

2026 年 10 月 19 日，我尝试利用小震活动改善未来 24 小时地震概率预报。历史研究使用了 14,927 条源区域地震事件，2020–2024 年的评价区间包含 29,232 个格日。

最初，候选模型相对日尺度地震聚集近似基线的对数评分增加了 14.22 nats。然后我在同一研究里进行了两项不同的检验：

| 检验 | 候选模型相对该检验的参照模型的对数评分差（越大越好） |
| --- | ---: |
| 原始回测：日尺度聚集近似 | +14.22 |
| 相同冻结参数，输入额外滞后一天 | −0.23 |
| 换成开发期校准的连续时空 ETAS 变体 | −0.75 |

后两项不是依次叠加的调整。一天滞后是敏感性实验，不是已核实的真实发布延迟。强基线比较的 180 天分块 bootstrap 95% 区间为 [−13.13, 8.91] nats，跨零；Brier 评分下候选略好，因此不能说它在所有指标上都输了。

更关键的是，历史事件目录使用的是后来下载的修订快照，无法证明每个输入值在预报发行时就已经可用。没有真正的前瞻验证，也没有地震预测突破。

这让我想到股票市场的大投资者交易披露。知道别人买了什么，和及时知道别人买了什么，可能是两回事。

## 03｜预测城市收入，复杂模型也没赢

在[中国城市机会环境图谱](https://github.com/Ericnotme/china-opportunity-atlas)中，我比较了香港 18 区家庭收入的历史预测模型：2022–2023 年拟合、2024 年选择超参数、2025 年留出检验。

| 模型 | 2025 年平均绝对误差（港元/月） |
| --- | ---: |
| 汇总趋势模型 | 649 |
| 调参后的收缩模型 | 722 |
| 持续性基线（上一期值） | 739 |

结果很有意思：最好的不是更复杂的收缩模型，而是汇总趋势模型。

这里只涉及一个测试年份、18 个区的家庭收入指标。它不证明搬家可以提高收入，更不预测孩子未来的收入。代码、方法和复现步骤见[实验说明](https://github.com/Ericnotme/china-opportunity-atlas/blob/main/EXPERIMENTS.md)。本轮核对了仓库记录，未重新运行该项目。

## 04｜比特币的底部时钟，没有找到

我也曾想找到一个可以预测比特币周期底部的规律。后来做了四项历史机制检验，逐项对照简单基准；四项均未达到事先设定的采用门槛。因此没有发布一个未经验证的本轮底部日期。

详见原研究：[我们没有找到比特币的底部时钟](https://weijia-lab.com/blog/bitcoin-bottom/)。这里引用原研究的结论，不声称本轮重新执行了四项检验。

## 我现在更想让 AI 帮我做什么

这四个案例不是独立随机抽取的科学样本，也不构成复杂模型普遍不如简单模型的统计证明。它们是我自己的研究档案中的失败或未获支持的案例。

但它们让我改变了一个习惯：

1. 开始研究之前，先写下一个很笨、很便宜的基准。
2. 明确哪一刻能够知道哪些信息；冻结规则，在未使用的数据上检验。
3. 不只展示成功，也保留失败、修订和停止条件。

AI 的价值不在于每次都给我一个漂亮的新模型。它也可以帮助我更快发现：我以为自己发现了什么，其实还没有。

**生成一个复杂的答案越来越便宜，证明它比简单答案更好，依然需要证据。**

---

### 复核说明 / Evidence and limits

- 交易：依据私人固定样本复核工作簿（2026-11-07，原始预测 66 条及行情复核 48 条），本轮从已有方向比较字段复算；未独立重新下载行情。仅公开聚合统计，不公开原聊天、账户或交易隐私。合并键为资产与原依赖组；按表中记录顺序保留第一条符合条件的记录。可从上面的 19 组汇总列联关系复算：看涨/实际涨 12，看涨/实际跌 5，看跌/实际涨 2，看跌/实际跌 0。原始 26 条对应四项为 17、6、3、0。
- 地震：依据 2026-10-19 两份历史研究报告（eq-v0.1 与 ETAS 扩展），本轮比较报告内已保存的结果；未重跑拟合。原始目录来源：[USGS FDSN](https://earthquake.usgs.gov/fdsnws/event/1/)。完整原始研究工件尚未在此公开，不能声称第三方已独立复现。
- 城市：[公开仓库及实验说明](https://github.com/Ericnotme/china-opportunity-atlas)；2025 年留出，18 个地区，不做因果解释。
- 比特币：[公开研究记录](https://weijia-lab.com/blog/bitcoin-bottom/)。
- 本文由 AI 协助检索、复算及撰写，属于个人研究札记，不是同行评议论文、投资建议或公众地震预警。

### English abstract

Four retrospective projects in my own research archive illustrate why a strong baseline matters: after grouping duplicate directional forecasts, an always-up baseline scored 14/19 against the AI's 12/19; an apparent earthquake forecasting gain disappeared under a one-day input lag or a stronger calibrated ETAS comparison; a pooled trend outperformed tuned shrinkage on a small Hong Kong household-income holdout; and four Bitcoin-bottom tests failed their adoption criteria. These are selected, heterogeneous negative or inconclusive cases—not a general statistical claim against complexity. The practical lesson is to define baselines, respect information availability, preserve failed tests and verify incremental value before deploying a model.
