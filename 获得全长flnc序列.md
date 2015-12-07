
## 获得全长flnc序列

分析获得全长 ``RS_IsoSeq`` 序列的命令是 `pbtranscript.py`. 此命令包含是分析全长序列 (`classify`) 和 isoform聚类 (`cluster`) 两个子命令。

正确配置SMRT Analysis并加载环境变量后，输入: 
```
pbtranscript.py --help
```
查看 classify 相关使用说明, 输入:
```
pbtranscript.py classify --help
```

### 输入

``reads_of_insert.fasta`` 


### 运行示例


```
pbtranscript.py classify reads_of_insert.fasta isoseq_draft.fasta \
--min_seq_len 300 --cpus 24 \
--flnc isoseq_flnc.fasta --nfl isoseq_nfl.fasta 
```

如上的命令指定了24个并行进程， 最短的序列长度为300. 

如果你没有使用官方引物 (Clontech SMARTer primers) , 而使用定制引物. 你需要一个如下格式的primer文件以指定引物:


```
>F0
5' sequence here
>R0
3' sequence here (一般需要将试剂盒说明书上的引物序列取反向互补)
```

如果引物序列不止一条，可以使用 F0,R0,F1,R1,F2,R2... 的命名方式，引物序列必须 成对如上格式命名，否则任务会失败。


运行的命令行则添加了`-p`参数, 如下:

```
pbtranscript.py classify reads_of_insert.fasta isoseq_draft.fasta \
 -p custom_primers.fa --cpus 24 --min_seq_len 300 \
 --flnc isoseq_flnc.fasta --nfl isoseq_nfl.fasta
```

如果您的数据没有 3' 端的polyA尾.测需要添加`--ignore_polyA`参数


```
pbtranscript.py classify reads_of_insert.fasta isoseq_draft.fasta \
 -p custom_primers.fa --cpus 24 --min_seq_len 300 \
 --ignore_polyA \
 --flnc isoseq_flnc.fasta --nfl isoseq_nfl.fasta
```


### 输出

输出文件 `isoseq_draft.fasta`是中间获得全长reads过程中的分析文件，是可以忽略的。

结果文件 `isoseq_flnc.fasta` 包含 full-length, non-artificial-concateme reads.

结果文件 `isoseq_nfl.fasta` 包含 non-full-length reads。之所以在非全长序列中没有过滤 artificial-concateme 是为了程序的运行效率:
过滤 artificial concatemers 耗时很长， 但是大多数情况下却又包含很少的 artificial concatemers 序列. 在分析的过程中，我们更关注的是
全长序列中是否包含 artificial concatemers. 

输出序列的 IDs 是如下的格式:
```
<movie_name>/<ZMW>/<start>_<end>_CCS 外加一些其他信息.
```

例子:


```
m140121_100730_42141_c100626750070000001823119808061462_s1_p0/119/30_1067_CCS strand=+;fiveseen=1;polyAseen=1;thr
eeseen=1;fiveend=30;polyAend=1067;threeend=1096;primer=1;chimera=0
```

其中包含了 movie ID `m140121_100730_42141_c100626750070000001823119808061462_s1_p0`, ZMW 号 119, 序列 trim (ReadsOfInsert) 的区域 30-1067,
以及一些引物相关信息.

有两个输出文件包含了分析的报告信息:

*  `isoseq_draft.primer_info.csv` 包含了每条序列引物鉴定相关的信息. 
*  `isoseq_draft.classify_summary.txt` 内容格式如下:
```
Number of reads of insert=135466
Number of five prime reads=55806
Number of three prime reads=63143
Number of poly-A reads=60582
Number of filtered short reads=3601
Number of non-full-length reads=97170
Number of full-length reads=34695
Number of full-length non-chimeric reads=33857
Average full-length non-chimeric read length=2428
```

**注意**: 输出文件中 `isoseq_flnc.fasta` 的序列是具有方向的（5' - > 3'）. 而 `isoseq_nfl.fasta` 是没有方向性的.




