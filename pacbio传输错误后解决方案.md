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
