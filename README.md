# An-analysis-of-NBA-players-salaries-and-on-court-performance
221549141-吴家建-NBA球员薪资与赛场表现之分析-python数据分析项目实战

# NBA球员薪资与赛场表现分析  
基于皮尔逊相关系数的分位置量化研究  


## 📌 项目概述  
本项目通过分析2023-2024赛季NBA球员薪资与赛场表现数据，利用皮尔逊相关系数揭示不同位置球员的薪资驱动因素。项目包含数据清洗、相关性分析及可视化全流程，代码开源可复现。


## 🛠️ 环境配置  
### 依赖环境  
- **Python版本**：3.8+  
- **关键库**：  
  ```bash  
  pandas>=1.3.3    # 数据处理  
  numpy>=1.21.2    # 数值计算  
  matplotlib>=3.4.3 # 基础可视化  
  seaborn>=0.11.2  # 统计可视化  
  scipy>=1.7.1     # 统计分析（皮尔逊相关系数）  
  ```  
  **安装命令**：  
  ```bash  
  pip install <库名> 
  ```

### 数据依赖  
1. **薪资数据**：`2021-2024PlayersSalaries.csv`  
   - 字段：`Player`（球员名）、`Salary(2023-2024)`（薪资，美元）  
2. **表现数据**：`2023-2024 NBA Player Stats - Regular.csv`  
   - 核心字段：`Player`、`Pos`（位置）、`PTS`（得分）、`DRB`（防守篮板）、`AST`（助攻）等32项指标  
3. **数据来源**：GitHub项目 [BusinessDataMiningProject](https://github.com/KristWangCY/BusinessDataMiningProject)（已整合至本仓库目录）


## 🚀 使用说明  
### 1.数据准备  
#### 数据获取  
1. **原始数据**：  
   - 薪资数据：`2021-2024PlayersSalaries.csv`（含2023-2024赛季薪资）  
   - 表现数据：`2023-2024 NBA Player Stats - Regular.csv`（含32项赛场指标）  
   - **下载地址**：从 [BusinessDataMiningProject](https://github.com/KristWangCY/BusinessDataMiningProject) 下载，或直接使用本仓库的样例数据。  

2. **数据格式要求**：  
   - 编码：UTF-8（原始数据若为Windows-1252编码需手动转换，脚本已内置转换逻辑）  
   - 字段匹配：确保两表均包含`Player`字段用于合并  
### 2. 克隆项目  
```bash  
git clone https://github.com/Kyrie907/An-analysis-of-NBA-players-salaries-and-on-court-performance.git  
cd An-analysis-of-NBA-players-salaries-and-on-court-performance  
```
或者直接下载.ipynb文件即可

### 3. 运行流程  
运行Jupyter Notebook或VScode，运行.ipynb文件，依次执行代码块



## 🔍 关键分析步骤  
1. **数据清洗**：  
   - 过滤非单一位置球员（如`Pos`含“/”的样本）  
   - 薪资列转换为数值型：`Salary(2023-2024) → float`  
2. **分位置分组**：  
   - 划分位置：`C`（中锋）、`SF`（小前锋）、`PF`（大前锋）、`SG`（得分后卫）、`PG`（控球后卫）  
 
## 🔍 核心技术细节  
### 🔢 数据预处理逻辑  


1. **缺失值处理策略**：  
   - 数值型（如PTS、DRB）：均值填充  
   - 字符型（如Pos）：众数填充   

2. **异常值处理**：  
   - 方法：IQR（四分位距）法，替换异常值为`[Q1-1.5IQR, Q3+1.5IQR]`  
   - 应用场景：薪资列（Salary）检测到18个异常值，经人工验证后保留合理值（如明星球员高薪）  


## 📊 结果示例  
### 全局相关性热力图  
![image](https://github.com/user-attachments/assets/a3ca856a-ee0c-40d1-8951-97df7a2dd54f)

*薪资与得分（PTS, r=0.68）、投篮命中数（FG, r=0.67）呈强正相关*

### 中锋位置关键指标散点图  
![image](https://github.com/user-attachments/assets/7b257c7a-83ba-4aa8-935a-efb958099a44)

*2分命中数每增加1次，薪资平均提升180万美元（r=0.79）*

## 📈 结果解读示例  
### 🌟 全局关键发现  
| 指标         | 相关系数(r) | 说明                          |  
|--------------|-------------|-------------------------------|  
| 得分（PTS）  | 0.68        | 薪资最核心驱动指标            |  
| 出场时间（MP）| 0.63        | 主力球员薪资溢价明显          |  
| 防守篮板（DRB）| 0.38       | 防守价值对薪资影响有限        |  
| 三分命中率（3P%）| 0.17      | 效率型指标与薪资关联较弱      |  

### 🏀 分位置核心指标对比  
| 位置 | 关键指标（r值）                | 典型球员案例          |  
|------|-------------------------------|-----------------------|  
| C    | 2P(0.79)、DRB(0.73)、BLK(0.66) | 约基奇（场均2P=12.5, DRB=13.0）|  
| PG   | MP(0.70)、DRB(0.63)、3P(0.57)  | 库里（MP=34.5, DRB=5.2）       |  
| SF   | 2PA(0.61)、AST(0.47)           | 詹姆斯（2PA=10.8, AST=8.3）    |  

## ❓ 常见问题（FAQ）  
1. **Q：图表显示中文乱码怎么办？**  
   - A：在可视化脚本中添加字体设置：  
     ```python  
     plt.rcParams['font.sans-serif'] = ['SimHei']  # 设置黑体  
     plt.rcParams['axes.unicode_minus'] = False     # 显示负号  
     ```  
2. **Q：如何添加其他赛季数据？**  
   - A：将数据按原始格式放入.ipynb文件所在目录，确保字段名一致，重新运行预处理脚本即可。

## 🤝 贡献方式  
1. **提出问题**：在 [Issues](https://github.com/Kyrie907/An-analysis-of-NBA-players-salaries-and-on-court-performance/issues) 中反馈疑问或建议  
2. **代码贡献**：提交Pull Request，需注明改进点及测试说明  
  
**更新时间**：2025年6月  
**作者**：吴家建  










  
