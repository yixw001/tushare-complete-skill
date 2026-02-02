# Tushare Complete 技能

## 项目简介

Tushare Complete 是一个完整的 Tushare Pro API 技能包，包含了 Tushare Pro 的所有 266 个接口文档，为用户提供全面的金融数据获取能力。

## 功能特点

### 完整的接口覆盖
- 包含 Tushare Pro 全部 266 个接口的详细文档
- 覆盖股票、基金、债券、期货、外汇、宏观经济等多个领域
- 每个接口都有完整的输入/输出参数说明

### 核心财务接口详细文档
以下 5 个核心财务接口提供了详细的参数说明和代码示例：

1. **资产负债表** (`balancesheet`)
   - 包含在建工程 (`cip`) 等关键财务字段
   - 支持多种报告类型和公司类型

2. **现金流量表** (`cashflow`)
   - 包含资本开支 (`c_pay_acq_const_fiolta`) 等重要指标
   - 提供完整的现金流量分析能力

3. **利润表** (`income`)
   - 包含扣非净利润 (`n_income_attr_p`) 等核心财务指标
   - 支持多维度财务分析

4. **财务指标数据** (`fina_indicator`)
   - 包含净资产收益率 (`roe`) 等关键财务比率
   - 提供全面的财务分析指标

5. **历史日线** (`daily`)
   - 提供完整的股票交易数据
   - 支持多市场、多周期数据获取

### 技术特性
- **完整的文档结构**：每个接口都有详细的参数说明、代码示例和数据样例
- **标准化的数据格式**：统一的参数命名和数据结构
- **易于集成**：可直接导入到 Trae IDE 中使用
- **定期更新**：保持与 Tushare Pro API 的同步更新

## 安装方法

1. 下载 `tushare-complete_20260202_1000.zip` 文件
2. 打开 Trae IDE
3. 点击左侧菜单栏的 "技能" 选项
4. 点击 "安装技能" 按钮
5. 选择下载的压缩文件进行安装
6. 等待安装完成即可使用

## 使用示例

### 1. 获取资产负债表数据
```python
pro = ts.pro_api()
df = pro.balancesheet(
    ts_code='600000.SH',
    start_date='20180101',
    end_date='20180730',
    fields='ts_code,ann_date,f_ann_date,end_date,report_type,comp_type,cap_rese,cip'
)
```

### 2. 获取现金流量表数据
```python
pro = ts.pro_api()
df = pro.cashflow(
    ts_code='600000.SH',
    start_date='20180101',
    end_date='20180730',
    fields='ts_code,ann_date,f_ann_date,end_date,c_pay_acq_const_fiolta'
)
```

### 3. 获取利润表数据
```python
pro = ts.pro_api()
df = pro.income(
    ts_code='600000.SH',
    start_date='20180101',
    end_date='20180730',
    fields='ts_code,ann_date,f_ann_date,end_date,n_income_attr_p'
)
```

### 4. 获取财务指标数据
```python
pro = ts.pro_api()
df = pro.fina_indicator(
    ts_code='600000.SH',
    start_date='20180101',
    end_date='20180730',
    fields='ts_code,ann_date,f_ann_date,end_date,roe,asset_liab_ratio'
)
```

### 5. 获取历史日线数据
```python
pro = ts.pro_api()
df = pro.daily(
    ts_code='600000.SH',
    start_date='20180101',
    end_date='20180131'
)
```

## 注意事项

1. **API 认证**：使用前需要在 Tushare Pro 官网注册账号并获取 API Key
2. **积分要求**：不同接口有不同的积分要求，详情请参考接口文档
3. **数据更新**：部分接口数据有更新频率限制，请合理安排数据获取时间
4. **错误处理**：使用时请注意捕获可能的 API 错误，确保代码的健壮性

## 版本信息

- **版本**：20260202_1000
- **Tushare Pro API 版本**：包含全部 266 个接口
- **更新时间**：2026-02-02

## 联系方式

如果您在使用过程中遇到问题，或有任何建议，请联系我们。

---

**© 2026 Tushare Complete 技能团队**
**致力于为用户提供最完整、最专业的金融数据获取能力**