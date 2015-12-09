# Trimmomatic做碱基质量剪切

`Trimmomatic`是一个针对 Illumina高通量测序的reads trim的工具。

- 对于single-ended，一个输入文件和一个输出文件，加上参数。
- 对于paired-end数据，两个输入文件，4个输出文件，分别为2个是'paired'，2个是'unpaired'（一个为forward的,一个为reverse的）

## 程序路径

`/bins/Trimmomatic-0.35/trimmomatic-0.35.jar`

## 命令示例

```shell
java -jar /bins/Trimmomatic-0.35/trimmomatic-0.35.jar PE -threads 12 \
-phred64 -trimlog logfile input_forward.fq input_reverse.fq \
output_forward_paired.fq output_forward_unpaired.fq \
output_reverse_paired.fq output_reverse_unpaired.fq \
ILLUMINACLIP:1.adapter.list:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

同一个指令下的不同参数可以用`':'`来界定

- `phred33`  设置碱基的质量格式,如果不设置，默认的是-phred64
- `trimlog file` 就是产生日志，包括如下部分内容：read的名字，留下来的序列的长度，第一个碱基的起始位置，从开始trimmed的长度，最后的一个碱基位于初始read的位置，最后trimmed的数量，其他步骤产生的日志
- `LEADING:3`   切除首端碱基质量小于3的碱基或者N
- `TRAILING:3`   切除末端碱基质量小于3的碱基或者
- `ILLUMINACLIP: 1.adapter.lis:2:30:10` 1.adapter.list为adapter文件，允许的最大mismatch 数，palindrome模式下匹配碱基数阈值：simple模式下的匹配碱基数阈值
- `SLIDINGWINDOW:4:15`  Windows的size是4个碱基，其平均碱基质量小于15，则切除
- `MINLEN:36`  最低reads长度为36
- `CROP:`  保留的reads长度
- `HEADCROP` 在reads的首端切除指定的长度
