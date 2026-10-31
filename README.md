# 数据清洗

将单类目或多类目电商商品表整理为可筛选的 Excel，并给出按类目的商品机会分析。

## 下载与使用

- **[快速使用说明：WorkBuddy、豆包、ChatGPT、Claude](快速使用说明.md)**
- **[下载技能 ZIP（v1.0.0）](https://github.com/submindd/data-cleaning/releases/download/v1.0.0/data-cleaning-v1.0.0.zip)**
- [下载普通对话版规则](https://github.com/submindd/data-cleaning/releases/download/v1.0.0/data-cleaning-rules.txt)
- [详细使用说明](使用说明.txt)
- [查看技能规则](skills/data-cleaning/SKILL.md)

WorkBuddy：在“专家·技能·连接器 → 技能 → 添加技能 → 上传技能”中选择上面的 ZIP，启用后上传自己的原始商品表。

普通豆包聊天：同时提供普通对话版规则和原始 Excel/CSV，要求“按规则处理整份文件，导出可下载的 Excel”。如果不能上传TXT，可复制全文到对话。上传规则文档是本次任务的上下文，不代表永久安装技能。

Codex及其他支持 Agent Skills 的工具：按照对应客户端的技能安装流程，安装本仓库的 `skills/data-cleaning` 目录或下载包中的 `data-cleaning` 目录。

## 默认规则

先应用 **体积≤2升** 的硬性限制。体积缺失、非数值或≤0的商品列为待核查，超过2升的商品另页保留。

对体积合格的商品，分别判断：

| 条件 | 默认要求 |
|---|---|
| 广告费用占比 | ≤7%，7%计为符合 |
| 商品卡创建年份 | 2026年 |
| 近28天下单件数 | >1000件，1000件不符合 |

依符合3条、2条、1条排序；同组按销量降序，再按广告费用占比升序。0条符合、超过体积或体积待核查的商品另页保留。年份默认固定2026，用户可明确修改条件。

## 结果内容

- 机会分析：商品依据、下一步及风险，不能将达标直接等同盈利。
- 类目汇总：多个类目分别统计样本数、达标分布、下单件数及金额；不是全市场排名。
- 筛选排序：支持一级/三级类目、FBO/FBS/FBO+FBS、创建月份和体积筛选，保留完整日期。
- 未符合条件：记录排除或待核查原因。
- 原始数据：保留源值，均值行不计入商品数量。

广告指标指**广告费用占比**，不是广告归因销售占比。商品卡创建日期不代表首次实际开售；下单件数不等于买断件数或包装内实物数量。

## 先用合成样例验证

[边界测试 CSV](skills/data-cleaning/assets/边界测试.csv)只有8条合成记录，不含真实商品或客户信息。

预期保留5条，顺序为 **T006、T001、T002、T007、T003**；另3条因体积超过2升、为0或未知而排除/待核查。详见[预期结果](skills/data-cleaning/references/边界测试预期结果.txt)。

下载生成的Excel后，还应检查类目、履约模式、月份及体积列是否有真实筛选按钮。

## 兼容性说明

这是流程与规则技能，不是独立的Excel处理软件。完整执行需要目标平台能读取整份表格、进行计算并生成Excel文件。

- 已完成规则、包结构及合成样例的边界核验。
- **尚未在 WorkBuddy 和豆包各客户端中实际运行，不承诺导入后所有功能完全一致。**
- WorkBuddy 官方支持上传本地技能包：[官方说明](https://www.codebuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Skills-Market)。
- 豆包工作技能商店有独立的企业认证与审核流程：[官方接入说明](https://open.doubao.com/document/doubao-developer-guide/skill-integration-guide)。普通聊天上传附件不等同于技能上架。

## 分享与更新

将本仓库链接或 Release 下载链接发给朋友即可。修改规则后，更新源码并发布新版本的ZIP；朋友应重新下载或更新技能。提交问题时只提供合成或已脱敏的最小样例，不要上传账户凭证和未经授权的业务数据。
