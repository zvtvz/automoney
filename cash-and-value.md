阅读下面文章前，请保证[**zvt**](https://github.com/zvtvz/zvt)的环境已经准备好。

>源码:  
>https://github.com/zvtvz/zvt  
>https://gitee.com/null_071_4607/zvt

>文档:  
>https://zvtvz.github.io/zvt/  
>http://zvt.foolcage.com

## 1. 现金

没有现金，流动性出问题了，灾难就发生了。
值此血雨腥风之际，决定用zvt的5行代码来选出"现金比市值高的个股",以纪念这必将载入史册的时刻。

## 2. 5行代码

```python
#导入data schema
In [1]: from zvt.domain import *

#查询市值
In [2]: df_valuation = StockValuation.query_data(start_timestamp='2020-03-18',columns=[StockValuation.code,StockValuation.market_cap])

#查询现金
In [3]: df_finance = BalanceSheet.query_data(start_timestamp='2019-09-30',columns=[BalanceSheet.cash_and_cash_equivalents,BalanceSheet.code]).drop_duplicates(subset='code', keep='last')

#取都有数据的交集
In [4]: codes=list(set(df_valuation.code.tolist()) & set(df_finance.code.tolist()))
In [5]: df1=df_valuation[df_valuation.code.isin(codes)].set_index('code',drop=True).sort_index()
In [6]: df2=df_finance[df_finance.code.isin(codes)].set_index('code',drop=True).sort_index()

#"现金比市值高的个股"
In [7]: codes = df1[df1.market_cap<df2.cash_and_cash_equivalents].index.tolist()
```

## 3. show me the code
```
In [8]: df_stock = Stock.query_data(codes=codes,columns=[Stock.code,Stock.name])

In [9]: df_stock[['code','name']]
Out[9]:
      code   name
0   600606   绿地控股
1   600657   信达地产
2   600665    天地源
3   000521   长虹美菱
4   600691   阳煤化工
5   000564   供销大集
6   600827   百联股份
7   600839   四川长虹
8   600858   银座股份
9   600859    王府井
10  000036   华联控股
11  000040   东旭蓝天
12  600705   中航资本
13  600708   光明地产
14  000404   长虹华意
15  600710    苏美达
16  000422   ST宜化
17  600743   华远地产
18  600751   海航科技
19  000413   东旭光电
20  000617   中油资本
21  000631   顺发恒业
22  000671    阳光城
23  000701   厦门信达
24  600051   宁波联合
25  000736   中交地产
26  600077   宋都股份
27  000732   泰禾集团
28  600096    云天化
29  600121   郑州煤电
30  000761   本钢板材
31  600153   建发股份
32  600170   上海建工
33  000090   天健集团
34  000933   神火股份
35  600221   海航控股
36  600277   亿利洁能
37  600280   中央商场
38  600290   ST华仪
39  000488   晨鸣纸业
40  600339   中油工程
41  600466   蓝光发展
42  600376   首开股份
43  600569   安阳钢铁
44  600361   华联综超
45  600375   华菱星马
46  600502   安徽建工
47  600325   华发股份
48  601001   大同煤业
49  601699   潞安环能
50  601588   北辰实业
51  601666   平煤股份
52  002183    怡亚通
53  601668   中国建筑
54  601788   光大证券
55  002450  *ST康得
56  601669   中国电建
57  002769    普路通
58  002889   东方嘉盛

```
## 6. 小结

好了，祝大家赚钱。

个人微信:foolcage 添加暗号:zvt  
<img src="https://raw.githubusercontent.com/zvtvz/zvt/master/docs/imgs/wechat.jpeg" width="25%" alt="Wechat">

---
**知乎专栏**:  
https://zhuanlan.zhihu.com/automoney  

**github**:  
https://github.com/zvtvz/automoney

**公众号**:  
<img src="./imgs/gongzhonghao.jpg" width="25%" alt="公众号">
