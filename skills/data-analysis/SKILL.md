---
name: data-analysis
description: 数据分析和可视化。使用场景：(1) Excel/CSV数据处理，(2) 数据清洗和转换，(3) 统计分析，(4) 数据可视化（图表生成），(5) 业务数据分析（销售/用户/运营），(6) A/B 测试分析，(7) 数据报告生成，(8) Python 数据分析脚本编写。
---

# 数据分析与可视化

## 概述

本技能提供完整的数据分析能力，从基础的数据处理到高级的统计分析，帮助用户从数据中提取洞察并生成可视化报告。

## 核心能力

### 1. 数据处理

- Excel/CSV 文件读取和写入
- 数据清洗（缺失值、异常值）
- 数据转换（格式、类型）
- 数据合并和连接
- 数据透视表

### 2. 统计分析

- 描述性统计（均值、中位数、标准差）
- 相关性分析
- 回归分析
- 假设检验
- 方差分析

### 3. 数据可视化

- 柱状图、折线图、饼图
- 散点图、热力图
- 箱线图、直方图
- 仪表盘设计
- 交互式图表

### 4. 业务分析

- 销售数据分析
- 用户行为分析
- 运营指标分析
- 财务数据分析
- 市场数据分析

### 5. 报告生成

- 数据分析报告
- 可视化仪表板
- 自动化报表
- PPT 报告导出

## 快速开始

### 数据处理流程

```
1. 数据导入 → 2. 数据清洗 → 3. 数据探索 → 4. 数据分析 → 5. 可视化 → 6. 报告输出
```

### 常用工具

**Python 库**

- `pandas` - 数据处理
- `numpy` - 数值计算
- `matplotlib` - 基础绘图
- `seaborn` - 统计绘图
- `plotly` - 交互式图表
- `openpyxl` - Excel 操作

**命令行工具**

- `csvkit` - CSV 处理
- `jq` - JSON 处理
- `sqlite3` - 数据查询

## 工作流

### 工作流 1: 销售数据分析

**步骤 1: 数据导入**

```python
import pandas as pd

# 读取 Excel/CSV
df = pd.read_excel('sales_data.xlsx')
# 或
df = pd.read_csv('sales_data.csv')
```

**步骤 2: 数据清洗**

```python
# 检查缺失值
df.isnull().sum()

# 填充或删除缺失值
df.fillna(0, inplace=True)
# 或
df.dropna(inplace=True)

# 删除重复值
df.drop_duplicates(inplace=True)

# 数据类型转换
df['date'] = pd.to_datetime(df['date'])
df['amount'] = pd.to_numeric(df['amount'])
```

**步骤 3: 数据探索**

```python
# 基本统计
df.describe()

# 销售总额
total_sales = df['amount'].sum()

# 平均订单金额
avg_order = df['amount'].mean()

# 订单数量
order_count = len(df)
```

**步骤 4: 维度分析**

```python
# 按产品类别分析
category_sales = df.groupby('category')['amount'].sum()

# 按地区分析
region_sales = df.groupby('region')['amount'].sum()

# 按时间分析（月度）
df['month'] = df['date'].dt.to_period('M')
monthly_sales = df.groupby('month')['amount'].sum()
```

**步骤 5: 可视化**

```python
import matplotlib.pyplot as plt

# 月度销售趋势
plt.figure(figsize=(12, 6))
monthly_sales.plot(kind='line')
plt.title('月度销售趋势')
plt.xlabel('月份')
plt.ylabel('销售额')
plt.grid(True)
plt.savefig('monthly_sales.png')
```

**步骤 6: 输出报告**

```python
# 导出分析结果
category_sales.to_excel('category_analysis.xlsx')

# 生成摘要报告
summary = {
    '总销售额': total_sales,
    '平均订单': avg_order,
    '订单数量': order_count,
    '最畅销品类': category_sales.idxmax()
}
pd.DataFrame([summary]).to_excel('summary.xlsx')
```

### 工作流 2: 用户行为分析

**步骤 1: 定义指标**

- DAU/MAU（日/月活跃用户）
- 留存率（次日、7 日、30 日）
- 转化率（注册→付费）
- ARPU（每用户平均收入）
- LTV（用户生命周期价值）

**步骤 2: 计算活跃用户**

```python
# DAU
dau = df.groupby('date')['user_id'].nunique()

# MAU
df['month'] = df['date'].dt.to_period('M')
mau = df.groupby('month')['user_id'].nunique()
```

**步骤 3: 计算留存率**

```python
def calculate_retention(df, day):
    # 首日用户
    day0 = df[df['date'] == df['date'].min()]['user_id'].unique()

    # 第 N 日活跃用户
    target_date = df['date'].min() + pd.Timedelta(days=day)
    dayN = df[df['date'] == target_date]['user_id'].unique()

    # 留存率
    retention = len(set(day0) & set(dayN)) / len(day0)
    return retention

retention_1day = calculate_retention(df, 1)
retention_7day = calculate_retention(df, 7)
retention_30day = calculate_retention(df, 30)
```

**步骤 4: 漏斗分析**

```python
# 各阶段用户数
funnel = df.groupby('stage')['user_id'].nunique()

# 转化率
conversion_rate = funnel / funnel.iloc[0] * 100
```

### 工作流 3: A/B 测试分析

**步骤 1: 数据准备**

```python
# 分组
group_a = df[df['group'] == 'A']['conversion']
group_b = df[df['group'] == 'B']['conversion']
```

**步骤 2: 统计检验**

```python
from scipy import stats

# T 检验
t_stat, p_value = stats.ttest_ind(group_a, group_b)

# 判断显著性
if p_value < 0.05:
    print("差异显著")
else:
    print("差异不显著")
```

**步骤 3: 效果评估**

```python
# 提升比例
lift = (group_b.mean() - group_a.mean()) / group_a.mean() * 100
print(f"B 组相对 A 组提升：{lift:.2f}%")
```

## 常用分析模型

### 1. RFM 模型（用户价值分析）

```python
# Recency: 最近一次消费
# Frequency: 消费频率
# Monetary: 消费金额

rfm = df.groupby('user_id').agg({
    'date': lambda x: (df['date'].max() - x.max()).days,
    'order_id': 'count',
    'amount': 'sum'
}).rename(columns={
    'date': 'R',
    'order_id': 'F',
    'amount': 'M'
})

# 分群
rfm['R_score'] = pd.qcut(rfm['R'], 4, labels=[4, 3, 2, 1])
rfm['F_score'] = pd.qcut(rfm['F'].rank(method='first'), 4, labels=[1, 2, 3, 4])
rfm['M_score'] = pd.qcut(rfm['M'], 4, labels=[1, 2, 3, 4])

rfm['RFM_score'] = rfm['R_score'].astype(int) + rfm['F_score'].astype(int) + rfm['M_score'].astype(int)
```

### 2. 同期群分析（Cohort Analysis）

```python
# 按用户首次购买月份分组
df['first_purchase'] = df.groupby('user_id')['date'].transform('min')
df['cohort'] = df['first_purchase'].dt.to_period('M')
df['period'] = (df['date'] - df['first_purchase']).dt.days // 30

cohort_data = df.groupby(['cohort', 'period'])['user_id'].nunique().unstack()
```

### 3. 帕累托分析（80/20 法则）

```python
# 按销售额排序
sorted_df = df.groupby('product')['amount'].sum().sort_values(ascending=False)

# 计算累计百分比
cumulative = sorted_df.cumsum() / sorted_df.sum() * 100

# 找出贡献 80% 销售额的产品
top_products = cumulative[cumulative <= 80].index
```

## 可视化图表选择指南

| 分析目的 | 推荐图表               |
| -------- | ---------------------- |
| 趋势分析 | 折线图、面积图         |
| 对比分析 | 柱状图、条形图         |
| 构成分析 | 饼图、环形图、堆积图   |
| 分布分析 | 直方图、箱线图         |
| 关系分析 | 散点图、气泡图、热力图 |
| 地理分析 | 地图、热力地图         |
| 漏斗分析 | 漏斗图、桑基图         |

## 图表美化技巧

### 配色方案

```python
# 专业配色
colors = ['#2E86AB', '#A23B72', '#F18F01', '#C73E1D', '#6A994E']

# 渐变色
cmap = 'viridis'  # 或 'plasma', 'inferno', 'magma'
```

### 字体设置

```python
plt.rcParams['font.sans-serif'] = ['SimHei']  # 中文
plt.rcParams['axes.unicode_minus'] = False  # 负号
```

### 图表保存

```python
plt.savefig('chart.png', dpi=300, bbox_inches='tight')
```

## 脚本工具

### scripts/data_cleaner.py

自动化数据清洗脚本（待创建）

### scripts/chart_generator.py

批量生成常用图表（待创建）

### scripts/report_builder.py

自动生成分析报告（待创建）

### scripts/excel_processor.py

Excel 批量处理工具（待创建）

## 参考资料

### references/statistics-guide.md

统计学基础指南（待创建）

### references/pandas-cheatsheet.md

Pandas 常用操作速查（待创建）

### references/visualization-best-practices.md

可视化最佳实践（待创建）

## 输出模板

### 数据分析报告模板

```markdown
# [分析主题] 数据分析报告

## 执行摘要

- 核心发现（3-5 点）
- 关键建议

## 数据概况

- 数据来源
- 时间范围
- 样本量
- 数据质量

## 分析方法

- 使用的指标
- 分析模型
- 工具和技术

## 分析结果

### 发现 1

- 数据支撑
- 图表展示
- 解读说明

### 发现 2

- ...

## 结论与建议

- 主要结论
- 行动建议
- 后续计划

## 附录

- 详细数据表
- 技术细节
- 参考资料
```

## 注意事项

1. **数据质量** - 分析前务必检查数据质量
2. **异常值处理** - 识别并合理处理异常值
3. **样本代表性** - 确保样本具有代表性
4. **因果关系** - 相关性不等于因果性
5. **数据隐私** - 保护用户隐私和数据安全
6. **结果验证** - 重要结论需要交叉验证
7. **可复现性** - 保留分析代码和过程

## 相关技能

- `web-research` - 网络数据采集
- `content-creation` - 报告内容撰写
- `analytics` - 业务数据分析（扩展）

---

**最后更新：** 2026-03-14  
**维护者：** 小灵 🧭
