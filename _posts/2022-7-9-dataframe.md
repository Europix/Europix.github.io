---
layout: post
title: DataFrame
categories: Blog
description: Usage of DataFrame
keywords: Dataframe
---

## 重新认识 Pandas 里面的 DataFrame
这两天写我的 [Eurobot](https://github.com/Europix/Eurobot) 时候发现一个问题: 
- 处理 Json 文件 （比对两个 json）会碰到乱序问题

之前我用了 `json_tools` 库里面的 `json.diff()`, 不过它本质就是 json 每一层的一一比对，如果出现乱序就会出现问题，比如无法一一对应，又或者是 json 里面多了新的条目，`json.diff()` 就出现问题了。

在这之后，我就首先想到双层循环，找两边相同的元素，比如：
这其中 achievements, fc, fs 可以改变， 那就比对 id 和 level_index, 让主键变成 `key = level_index*id`
```
{
            "achievements": 100.9285,
            "fc": "ap",
            "fs": "fsp",
            "id": 11226,  # uneditable
            "level": "13",
            "level_index": 3,   #uneditable
            "title": "時計の国のジェミニ",
            "type": "DX"
        }
```
我写功能是至简的原则，txt 当数据库什么的啥我没干过。 但是这次想了想：

3000个歌 x 2层循环， 加上查找比对至少5个数据， $$O(n^2)$$的查找有点难办，于是开始求助

## DataFrame 存 Json (Innovated from KevinZ)
**Pandas** 库给了直接读 json 到 DataFrame 的方法:
```
	df=pd.read_json(json_data,orient="records")
```
然后生成新的主键 index:
```
	df2['newindex']=df2['id']*10+df2['level_index']
    df2=df2.set_index(['newindex'],drop=True)
```
接下来处理df 
```
	newindexlist=(set(df2.index).difference(set(df1.index))) # 新打的歌id
    removeiindexlist=(set(df1.index).difference(set(df2.index))) # 删掉的歌id
    changeindex=df1.iloc[:,:1].eq(df2.iloc[:,:1])
    changeindex=changeindex.loc[changeindex['achievements']==False].index # 分数不相等的id
    updateindexlist=set(changeindex).difference(set(newindexlist)).difference(set(removeiindexlist)) # 扣去新打的和删掉的
```
*这里的id是散列后的id*
然后获取不同的数据
```
add_list = df2.loc[list(newindexlist)]
update_list = df3.loc[list(updateindexlist)]
```
最后输出 完美 
```
更新曲信息
          achievements   fc  fs     id level  level_index                    title type  newachievements newfc newfs
newindex
3842          100.4409  fcp  fs    384   10+            2                   VERTeX   SD         100.7347   fcp    fs
5892           98.2881             589   11+            2               Panopticon   SD          99.3817
7173           96.1060             717   12+            3  ラブって♡ジュエリー♪えんじぇる☆ブレイク！！   SD          98.8920
110213         98.8758           11021   13+            3                    魔ジョ狩リ   DX          99.0011
2443           96.3730             244    12            3                  おても☆Yan   SD          97.5183
...                ...  ...  ..    ...   ...          ...                      ...  ...              ...   ...   ...
111223         98.8774           11122   12+            3                アンドロイドガール   DX         100.0328
110073         97.6939           11007    12            3                  超常マイマイン   DX          99.4957
110203         96.4419           11020    13            3         Technicians High   DX          97.3704
111613         98.3960           11161    13            3                    オリフィス   DX          99.4142    fc   fsp
7934           97.7659             793    13            4                       ロキ   SD          98.6442

[61 rows x 11 columns]
```
## 总结
DataFrame 是 Pandas 的用来分析数据的很有用的数据结构(?)， 这次用完感觉和之前单纯拿 DF 来存数据时候使用的完全是两个东西。 DataFrame 既有行索引也有列索引，它可以被看做由series组成的字典，但是比单纯的 json 拿来处理数据舒服很多。

吃了安利，也感谢半夜四点半还在干活的 [KevinZ](https://github.com/SaigyoujiShizuka)：你不得给我磕一个

Edited: 这人第二天晚上六点十七才起床的 ↑
