# Homework 20201210

## Threshold 0.3 及 0.8 圖
### Threshold 0.8
![](https://i.imgur.com/DGx9iUv.png)
### Threshold 0.3
![](https://i.imgur.com/n7zSoNH.png)
Threshold 0.8 跟0.3 的差別就是相似度的閥值,0.8閥值高,所以連接線少,反之0.3閥值高,所以連接線多。閥值最好別設太高或太低,太高連接線太少根本找不到相關的惡意程式,太低連接線太多,變成每個惡意程式都相關,不是我們想要的結果。

* * *
## 分析程式
### jaccard
```
def jaccard(set1,set2):
    intersection = set1.intersection(set2)
    intersection_length = float(len(intersection))
    union = set1.union(set2)
    union_length = float(len(union))
    return intersection_length / union_length
```
jaccard similarity function 是用來兩個東西的相似度, 先求set1及set2的交集長度,再求set1及set2的聯集長度,最後交集長度除以聯集長度就是 jaccard similarity # jaccard similarity 絕對會<= 1

### pecheck
```
def pecheck(fullpath):

    return open(fullpath).read(2) == "MZ"
    
malware_paths = filter(pecheck, malware_paths)    
```
用於過濾掉不是PE檔的資料


### networkx

```
graph = networkx.Graph()
graph.add_node(path,label=os.path.split(path)[-1][:10])
graph.add_edge(malware1,malware2,penwidth=1+(jaccard_index-args.threshold)*10)

```
networks 畫圖 
* Graph():產生圖

* add_node():畫出節點

* add_edge():畫出連接線

### Threshold
```
if jaccard_index > args.threshold:
            print malware1,malware2,jaccard_index
            graph.add_edge(malware1,malware2,penwidth=1+(jaccard_index-args.threshold)*10)

```
相似度結果如果大於 Threshold ,就畫出兩個資料間的連接線,因此 Threshold 設越大, 代表越嚴謹,圖內的線也就越少

### write_dot
```
write_dot(graph,args.output_dot_file)
```
產生出dot file

