# PacBio传输错误（分析错误）后解决流程

## 步骤

+ putty登录pap01
+ 进入homer

`pbi@pap01-4220:homer`

进入homer后显示一个小人,还有几个大于号.

查看pacbioRS 是否报故障

+ homer下输入 `dumpAllAlarms()` 

无故障输出如下：
```
Alarms (verbose): 
  
 No alarms 
 
```



```python
pc=PipelineController.Instance 
pc.GetFailedOrAborted() 
```

会输出错误的run信息如下：

```
Dictionary[str, List[str]]({'Run139_20141207' : List[str](['m141207_110036_42208_c100715102550000001823154504301560_s1_p0']), 'Run126_20141117' : List[str](['m141118_060416_42208_c100724582550000001823142304301556_s1_p0'])}) 
```

## 初级分析任务

如果是提交分析任务用下面的命令：

```
>>> pc.SubmitPapJob('m141207_110036_42208_c100715102550000001823154504301560_s1_p0') 
```

结果显示：

```
<PacBio.Instrument.Jobs.PrimaryAnalysisJob object at 0x000000000000002F [PacBio.Instrument.Jobs.PrimaryAnalysisJob]> 
```


## 传输任务

如果提交传输任务：

```
pc.SubmitTransJob('m141118_060416_42208_c100724582550000001823142304301556_s1_p0') 
```

结果显示：
```
<PacBio.Instrument.Jobs.TransferJob object at 0x0000000000000030 [PacBio.Instrument.Jobs.TransferJob]>
```

## 查看已提交任务

下面的python命令行可以在homer中查看已提交的任务

```python
for i in pc.GetJobs(): print str(i.JobType)+" job has status " +str(i.Status)+" on cell "+str(i.MovieContextStr)+"\n" 
```

结果如下：
```
Primary job has status Running on cell m141207_110036_42208_c100715102550000001823154504301560_s1_p0 
  
Transfer job has status Ready on cell m141207_110036_42208_c100715102550000001823154504301560_s1_p0 
  
Transfer job has status Ready on cell m141118_060416_42208_c100724582550000001823142304301556_s1_p0 
```

## 退出

`crtl+]` 后出来`telnet>`然后按`q`退出。
## 命令行运行ReadsOfInsert

命令行运行 ReadsOfInser 的命令是`ConsensusTools` . 可以到smrt安装穆棱 `<smrtanalysis_directory>/doc/bioinformatics-tools/ConsensusTools/doc/index.html`下面查看相关文档。


需要的输入文件：

```
input.fofn --- A plain file containing the list of .bax.h5 file locations.
```

下面是一个 `input.fofn` 文件的例子.一共有两个movie，每个包含三个 .bax.h5 文件 (.1.bax.h5, .2.bax.h5, and .3.bax.h5).

```
/MYHOME/runs/m140121_100730_42141_c100626750070000001823119808061462_s1_p0.1.bax.h5
/MYHOME/runs/m140121_100730_42141_c100626750070000001823119808061462_s1_p0.2.bax.h5
/MYHOME/runs/m140121_100730_42141_c100626750070000001823119808061462_s1_p0.3.bax.h5
/MYHOME/runs/m140121_132657_42141_c100626060070000001823118408061490_s1_p0.1.bax.h5
/MYHOME/runs/m140121_132657_42141_c100626060070000001823118408061490_s1_p0.2.bax.h5
/MYHOME/runs/m140121_132657_42141_c100626060070000001823118408061490_s1_p0.3.bax.h5
```


经过训练的 ReadsOfInsert 参数一般保存在 `<smrtanalysis_directory>/analysis/etc/algorithm_parameters/`下. 如果要进行后续的 Iso-Seq 分析，例如 ICE 和 Quiver,
我们建议设置 minimum number of full passes 为 ``0``, the minimum predicted accuracy 为 ``75``.

下面是一个运行的示例命令：

```
ConsensusTools.sh CircularConsensus  --minFullPasses 0  --minPredictedAccuracy 75 \
    --parameters <smrtanalysis_directory>/analysis/etc/algorithm_parameters/2014-03 \
    --numThreads 24 --fofn /MYHOME/test_dir/input.fofn \
    -o /MYHOME/test_dir/data
```

运行结束, 输出结果为 `/MYHOME/test_dir/data/<movie>.ccs.[fasta|fastq|h5]`. 需要将结果合并，以进行后续 Iso-Seq 分析:

```
cat /MYHOME/test_dir/data/*.ccs.fasta > reads_of_insert.fasta
cat /MYHOME/test_dir/data/*.ccs.fastq > reads_of_insert.fastq
cat /MYHOME/test_dir/data/*.ccs.h5 > reads_of_insert.fofn
```

注意：FOFN文件中一定要使用绝对路径 ！！（`reads_of_insert.fofn` and `input.fofn`）

下一步的分析就是要进行 Iso-Seq 的 "classify" 步骤，从而获得全长和非全长的转录本序列. 


