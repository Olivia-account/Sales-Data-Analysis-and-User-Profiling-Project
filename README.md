# Sales-Data-Analysis-and-User-Profiling-Project
# 项目介绍
这个项目旨在对销售数据进行深入分析，并通过用户画像生成来揭示销售业务中的关键信息。通过对销售额、数量、利润等指标的统计分析，以及产品销售情况、地域销售情况、客户销售情况和时间趋势的分析，帮助企业了解销售业绩表现、产品热卖情况、地域销售重点、客户贡献度以及销售额的月度趋势。同时，通过用户画像生成函数，可以根据用户姓名获取其消费数据，分析其消费习惯，包括总消费额、平均订单金额和最常购买的产品。
# 数据展示
整体数据拥有以下特征，共计10000条数据。

```python
<class 'pandas.core.frame.DataFrame'>
Int64Index: 9959 entries, 1 to 10000
Data columns (total 19 columns):
 #   Column  Non-Null Count  Dtype         
---  ------  --------------  -----         
 0   订单 ID   9959 non-null   object        
 1   订单日期    9959 non-null   datetime64[ns]
 2   发货日期    9959 non-null   datetime64[ns]
 3   邮寄方式    9959 non-null   object        
 4   客户 ID   9959 non-null   object        
 5   客户名称    9959 non-null   object        
 6   细分      9959 non-null   object        
 7   城市      9959 non-null   object        
 8   省/自治区   9959 non-null   object        
 9   国家      9959 non-null   object        
 10  地区      9959 non-null   object        
 11  产品 ID   9959 non-null   object        
 12  类别      9959 non-null   object        
 13  子类别     9959 non-null   object        
 14  产品名称    9959 non-null   object        
 15  销售额     9954 non-null   float64       
 16  数量      9950 non-null   float64       
 17  折扣      9952 non-null   float64       
 18  利润      9956 non-null   float64       
dtypes: datetime64[ns](2), float64(4), object(13)

```
# 项目思路与主要代码
### 1. 数据读取和基本信息查看
首先通过pd.read_excel()方法读取名为"data.xls"的Excel文件中的数据，然后使用set_index()方法将"行 ID"列设置为数据的索引，并通过info()方法查看数据的基本信息。
在这个部分的实现过程中，首先使用pd.read_excel()方法读取名为"data.xls"的Excel文件中的数据，并将其存储在名为data的DataFrame中。接着，使用set_index()方法将DataFrame中的索引设置为"行 ID"列，这样可以更方便地按照行 ID 进行数据检索。最后，通过调用info()方法，查看数据的基本信息，包括列名、非空值数量、数据类型等。

```python
import pandas as pd

# 读取数据
data = pd.read_excel("data.xls")

# 将行ID作为索引
data = data.set_index('行 ID')

# 查看数据的基本信息
print(data.info())

```
这段代码首先导入了Pandas库，然后通过pd.read_excel("data.xls")读取名为"data.xls"的Excel文件，并将数据存储在名为data的DataFrame中。接着，使用set_index('行 ID')将"行 ID"列设置为DataFrame的索引。最后，通过print(data.info())查看数据的基本信息，包括索引类型、列名、非空值数量和数据类型等。
### 2. 销售数据统计分析
通过describe()方法查看"销售额"、"数量"和"利润"列的统计信息，以及计算各列之间的相关性矩阵。
在销售数据统计分析部分，代码的主要逻辑如下：
1. 使用describe()方法查看"销售额"、"数量"和"利润"列的统计信息，包括平均值、标准差、最小值、最大值等。
2. 计算各列之间的相关性矩阵，可以通过corr()方法实现，它会计算不同列之间的相关性系数。

```python
# 探索销售额、数量和利润的统计信息
print(data[['销售额', '数量', '利润']].describe())

# 销售额、数量和利润之间的相关性分析
correlation_matrix = data.corr()
print(correlation_matrix)

```
通过以上代码，可以快速查看销售额、数量和利润列的统计信息，以及了解这些列之间的相关性，帮助进一步理解销售数据的特征和关联情况。
### 3. 产品销售情况分析
对产品名称进行分组，并计算每个产品的销售额总和，最后按销售额降序排列并输出前10个产品的销售情况。
在产品销售情况分析部分，代码的主要逻辑如下：
1. 使用groupby('产品名称')将数据按产品名称进行分组。
2. 计算每个产品的销售额总和，可以通过sum()方法实现。
3. 对产品销售额进行降序排列，可以通过sort_values(ascending=False)实现。
4. 最后，输出前10个产品的销售情况。

```python
# 产品销售情况分析
product_sales = data.groupby('产品名称')['销售额'].sum().sort_values(ascending=False)
print("产品销售情况：\n", product_sales.head(10))

```
通过这段代码，可以对不同产品的销售情况进行分析，了解哪些产品销售额较高，从而帮助企业了解产品的热门程度和销售贡献。
### 4. 地域分析
对地区进行分组，并计算每个地区的销售额总和，最后按销售额降序排列输出地域销售情况。
1. 使用groupby('地区')将数据按地区进行分组。
2. 计算每个地区的销售额总和，可以通过sum()方法实现。
3. 对地区销售额进行降序排列，可以通过sort_values(ascending=False)实现。
4. 最后，输出按销售额降序排列的地域销售情况。

```python
# 地域分析
region_sales = data.groupby('地区')['销售额'].sum().sort_values(ascending=False)
print("地域销售情况：\n", region_sales)

```
通过这段代码，可以对不同地区的销售情况进行分析，了解哪些地区的销售额较高，帮助企业了解销售重点地域和市场表现。
### 5. 客户分析
对客户名称进行分组，并计算每个客户的销售额总和，最后按销售额降序排列输出客户销售情况的前10名。
1. 使用groupby('客户名称')将数据按客户名称进行分组。
2. 计算每个客户的销售额总和，可以通过sum()方法实现。
3. 对客户销售额进行降序排列，可以通过sort_values(ascending=False)实现。
4. 最后，输出按销售额降序排列的客户销售情况的前10名。

```python
# 客户分析
customer_sales = data.groupby('客户名称')['销售额'].sum().sort_values(ascending=False)
print("客户销售情况：\n", customer_sales.head(10))

```
通过这段代码，可以对不同客户的销售情况进行分析，了解哪些客户贡献了较高的销售额，帮助企业了解客户贡献度和重点客户的销售表现。
### 6. 时间趋势分析
将订单日期转换为月份格式，计算每个月的销售额总和，然后绘制月度销售额趋势图表。
1. 将订单日期转换为月份格式，可以通过提取日期中的月份信息来实现。
2. 计算每个月的销售额总和，通过使用groupby('订单月份')['销售额'].sum()方法可以实现。
3. 绘制月度销售额趋势图表，可以使用数据可视化库（如Matplotlib、Seaborn等）绘制线形图来展示销售额随时间的变化趋势。

```python
import matplotlib.pyplot as plt
import seaborn as sns

# 将订单日期转换为月份格式
data['订单月份'] = data['订单日期'].dt.month

# 计算每个月的销售额总和
monthly_sales = data.groupby('订单月份')['销售额'].sum()

# 绘制月度销售额趋势图表
plt.figure(figsize=(10, 6))
sns.lineplot(x=monthly_sales.index, y=monthly_sales.values)
plt.title('月度销售额趋势')
plt.xlabel('月份')
plt.ylabel('销售额')
plt.show()

print("月度销售额趋势：\n", monthly_sales)

```
通过这段代码，可以对销售额随时间的变化趋势进行可视化展示，帮助企业了解销售业绩在不同月份的表现。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/80c98bba95c84787810d50b8409fa3c3.png)

### 7. 用户画像函数
定义了一个名为get_user_profile的函数，根据输入的用户姓名筛选出该用户的消费数据，并分析其消费习惯，包括总消费额、平均订单金额和最常购买的产品。如果找不到该用户信息，则返回相应提示。
1. 定义一个名为get_user_profile的函数，该函数接收一个用户姓名作为输入参数。
2. 在函数内部，根据输入的用户姓名筛选出该用户的消费数据，可以使用data[data['客户名称'] == user_name]来实现。
3. 如果找到了该用户的消费数据，对其进行分析，包括计算总消费额、平均订单金额和最常购买的产品。
4. 如果未找到该用户信息（即筛选出的数据为空），则返回相应提示。

```python
def get_user_profile(user_name):
    # 根据用户姓名筛选数据
    user_data = data[data['客户名称'] == user_name]

    if user_data.empty:
        return "找不到该用户信息"

    # 对用户的消费数据进行进一步分析
    total_spending = user_data['销售额'].sum()
    average_order_amount = user_data['销售额'].mean()
    most_purchased_product = user_data['产品名称'].mode().values[0]

    return f"用户：{user_name}\n总消费额：{total_spending}\n平均订单金额：{average_order_amount}\n最常购买的产品：{most_purchased_product}"

# 调用函数并输出用户画像
user_name = "某某客户"  # 输入要查询的用户姓名
user_profile = get_user_profile(user_name)
print(user_profile)

```
通过这段代码，可以根据输入的用户姓名获取该用户的消费数据，并分析其消费习惯，包括总消费额、平均订单金额和最常购买的产品。如果未找到该用户信息，则会返回相应的提示。
## 8. 用户画像生成
通过用户输入获取用户姓名，然后调用get_user_profile函数生成用户画像，并输出用户的消费习惯信息。
1. 用户输入要查询的用户姓名。
2. 调用之前定义的get_user_profile函数，传入用户输入的姓名作为参数，生成用户画像信息。
3. 输出用户的消费习惯信息，包括总消费额、平均订单金额和最常购买的产品。

```python
# 用户输入要查询的用户姓名
user_name = input("请输入要查询的用户姓名：")

# 调用get_user_profile函数生成用户画像
user_profile = get_user_profile(user_name)

# 输出用户的消费习惯信息
print(user_profile)

```
通过这段代码，用户可以输入要查询的用户姓名，然后调用get_user_profile函数生成该用户的消费画像信息，并输出用户的消费习惯信息，帮助企业了解特定客户的消费行为和偏好。
# 项目链接
CSDN：https://blog.csdn.net/m0_46573428/article/details/136092221?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22136092221%22%2C%22source%22%3A%22m0_46573428%22%7D
# 后记
如果觉得有帮助的话，求 关注、收藏、点赞、星星 哦！
