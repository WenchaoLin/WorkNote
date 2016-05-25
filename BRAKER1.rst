
基因组注释程序BRAKER１使用说明
=============================



.. toctree::
   :maxdepth: 2



简介
-----

BRAKER1 属于无参的、基于RNAseq的基因组注释软件流程。软件集合了GeneMark-ET和AUGUSTUS。输入文件是一个基因组的FASTA序列和一个转录组的map的bam结果文件。


软件依赖
--------


至少需要安装配置下列软件:

* AUGUSTUS 3.1.0
* GeneMark-ET 4.29
* BAMTOOLS 2.3.0
* SAMTOOLS 1.2 (using htslib 1.2.1)

依赖于但不限于下列的Perl模块:

::

 File::Spec::Functions
 Hash::Merge
 List::Util
 Logger::Simple
 Module::Load::Conditional
 Parallel::ForkManager
 POSIX
 Scalar::Util::Numeric
 YAML
 
 
安装步骤
--------

1. Download GeneMark-ET from http://exon.gatech.edu/GeneMark/license_download.cgi
2. Download AUGUSTUS from http://bioinf.uni-greifswald.de/augustus/downloads/index.php
3. Download BAMTOOLS (e.g. e.g. git clone https://github.com/pezmaster31/bamtools.git)
4. Install BAMTOOLS by typing the following in your shell:


进入bamtools路径后编译bamtools

::
  
 mkdir build
 cd build
 cmake ..
 make


    
5. 将 braker.pl, AUGUSTUS bin文件夹, AUGUSTUS scripts文件夹添加到系统$PATH。

::

   PATH=/my_path_to_braker/:/my_path_to_augustus/bin/:/my_path_to_augustus/scripts/:$PATH
   export PATH


6. 可选的配置

配置环境变量 $AUGUSTUS_CONFIG_PATH, $GENEMARK_PATH, $BAMTOOLS_PATH 以及 $SAMTOOLS_PATH 到指定的位置


如果不将对应的路径写到环境变量里，可以在运行的时候使用下列参数指定相应软件的目录位置：

::

    --AUGUSTUS_CONFIG_PATH=/my_path_to_AUGUSTUS/augustus/config/ 
    --GENEMARK_PATH=/my_path_to_GeneMark-ET/gmes_petap/
    --BAMTOOLS_PATH=/my_path_to_bamtools/bin/
    --SAMTOOLS_PATH=/my/path_to_samtools/


7. 其实你看了这么一大堆没有用，我已经帮你装好了，只要加载一下环境变量就可以了

::
  
  source /work/lwc/workFlow/BRAKER/setup.BRAKER



执行BRAKER1
-----------

BRAKER1 有两个必选的参数：

1. **fasta** 格式的基因组序列
2. **bam** 格式的RNA-Seq的map结果

另外，你可以用gff格式的 "intron hints"结果代替bam格式的map结果，格式如下：
::

  contig_name	TopHat2	intron	2740	2888	25	+	.	.

主要要有如下几列内容:
::

    Column <seqname>     序列名字
    Column <source>      任何值貌似都可以
    Column <feature>     "intron" ( 必须是intron !)
    Columns <start><end> 起始，终止坐标 
                         
    Column <score>       对TopHat2结果来说是reads数目，别的软件可能是别的值，不重要
    Column <strand>     + or -
    
    
    gff的后面几列BREAKER（GENEMARK)用不着。

但是这个gff还是需要map结果生成的～～～

::

  --hints=gff-hintsfilename




运行方法:

加载环境变量
^^^^^^^^^^^^
加载环境变量是必须滴，否则会找不到命令

::

  /work/lwc/workFlow/BRAKER/setup.BRAKER

运行BRAKER1
^^^^^^^^^^^

运行BRAKER1很简单，跟我一起无脑敲:

::
  
  perl braker.pl [OPTIONS] --genome=genome.fa --bam=RNAseq.bam

或者
::

  perl braker.pl [OPTIONS] --genome=genome.fa --hints=RNAseq.gff


其他参数
^^^^^^^^

::

  --alternatives-from-evidence=true/false

如果一个转录本有多种情况，都输出，默认是true

::

  --AUGUSTUS_CONFIG_PATH=/my_path_to_AUGUSTUS/augustus/config/ 
  --GENEMARK_PATH=/my_path_to_GeneMark-ET/gmes_petap/
  --BAMTOOLS_PATH=/path/to/bamtools/
  --SAMTOOLS_PATH=/path/to/samtools/

如果没有在环境变量里面指定对应路径，或者想用自己的指定路径，可以使用上面的几个参数

::

  --cores=n             计算CPU核心数
  --fungus              使用专门的真菌预测模型，预测真菌时请选用

  --hints=hints.gff     与--optCfgFile联合使用，提供额外的转录本信息给AUGUSTUS
  --optCfgFile=extrinsic.cfg  AUGUSTUS配置文件, 可与 --hints 联合使用，能改变BRAKER1的默认参数
  --overwrite           你懂得
  --skipGeneMark-ET
  
  --skipOptimize        跳过优化参数步骤（强烈不建议使用本参数！！)
  --softmasking
  --useexisting         使用已有的AUGUSTUS模型
  --UTR=on/off          预测UTR区，仅支持 human, galdieria, toxoplasma 和 caenorhabditis

  --workingdir=/path/to/wd/   默认当前路径

  --species=speciesname  参考AUGUSTUS，一般在 ${AUGUSTUS_CONFIG_PATH}/species下

  --version              还是你懂得

Examples
^^^^^^^^

::

  perl braker --species=new_species  --genome=example.fa  --bam=example.bam



如果你要用已经存在的物种模型，请参考下面列表，并使用 --useexisting 参数，一般这些模型都存在 ${AUGUSTUS_CONFIG_PATH}/species下

::

   identifier                               | species
   -----------------------------------------|----------------------
   human                                    | Homo sapiens
   fly                                      | Drosophila melanogaster
   arabidopsis                              | Arabidopsis thaliana
   brugia                                   | Brugia malayi
   aedes                                    | Aedes aegypti
   tribolium                                | Tribolium castaneum
   schistosoma                              | Schistosoma mansoni
   tetrahymena                              | Tetrahymena thermophila
   galdieria                                | Galdieria sulphuraria
   maize                                    | Zea mays
   toxoplasma                               | Toxoplasma gondii
   caenorhabditis                           | Caenorhabditis elegans
   (elegans)                                | Caenorhabditis elegans 
   aspergillus_fumigatus                    | Aspergillus fumigatus
   aspergillus_nidulans                     | Aspergillus nidulans
   (anidulans)                              | Aspergillus nidulans
   aspergillus_oryzae                       | Aspergillus oryzae
   aspergillus_terreus                      | Aspergillus terreus
   botrytis_cinerea                         | Botrytis cinerea
   candida_albicans                         | Candida albicans
   candida_guilliermondii                   | Candida guilliermondii
   candida_tropicalis                       | Candida tropicalis
   chaetomium_globosum                      | Chaetomium globosum
   coccidioides_immitis                     | Coccidioides immitis
   coprinus                                 | Coprinus cinereus
   coprinus_cinereus                        | Coprinus cinereus
   coyote_tobacco                           | Nicotiana attenuata
   cryptococcus_neoformans_gattii           | Cryptococcus neoformans gattii
   cryptococcus_neoformans_neoformans_B     | Cryptococcus neoformans neoformans
   cryptococcus_neoformans_neoformans_JEC21 | Cryptococcus neoformans neoformans
   (cryptococcus)                           | Cryptococcus neoformans
   debaryomyces_hansenii                    | Debaryomyces hansenii
   encephalitozoon_cuniculi_GB              | Encephalitozoon cuniculi
   eremothecium_gossypii                    | Eremothecium gossypii
   fusarium_graminearum                     | Fusarium graminearum
   (fusarium)                               | Fusarium graminearium
   histoplasma_capsulatum                   | Histoplasma capsulatum
   (histoplasma)                            | Histoplasma capsulatum
   kluyveromyces_lactis                     | Kluyveromyces lactis
   laccaria_bicolor                         | Laccaria bicolor
   lamprey                                  | Petromyzon marinus
   leishmania_tarentolae                    | Leishmania tarentolae
   lodderomyces_elongisporus                | Lodderomyces elongisporus
   magnaporthe_grisea                       | Magnaporthe grisea
   neurospora_crassa                        | Neurospora crassa
   (neurospora)                             | Neurospora crassa
   phanerochaete_chrysosporium              | Phanerochaete chrysosporium
   (pchrysosporium)                         | Phanerochaete chrysosporium
   pichia_stipitis                          | Pichia stipitis
   rhizopus_oryzae                          | Rhizopus oryzae
   saccharomyces_cerevisiae_S288C           | Saccharomyces cerevisiae
   saccharomyces_cerevisiae_rm11-1a_1       | Saccharomyces cerevisiae
   (saccharomyces)                          | Saccharomyces cerevisiae
   schizosaccharomyces_pombe                | Schizosaccharomyces pombe
   thermoanaerobacter_tengcongensis         | Thermoanaerobacter tengcongensis
   trichinella                              | Trichinella spiralis
   ustilago_maydis                          | Ustilago maydis
   (ustilago)                               | Ustilago maydis
   yarrowia_lipolytica                      | Yarrowia lipolytica
   nasonia                                  | Nasonia vitripennis
   tomato                                   | Solanum lycopersicum
   chlamydomonas                            | Chlamydomonas reinhardtii
   amphimedon                               | Amphimedon queenslandica
   pneumocystis                             | Pneumocystis jirovecii
   wheat                                    | Triticum aestivum
   chicken                                  | Gallus gallus


BRAKER1的输出
--------------

BRAKER1 主要有3个重要的输出文件:

::

  introns.gff   - 从 RNAses中提取的内含子信息.这些内含子用来训练GeneMark-ET并用来做AUGUSTUS基因预测
  genemark.gtf  - GeneMark-ET 预测的基因
  augustus.gff  - AUGUSTUS 预测的基因




样例数据
---------------


BRAKER文件夹下面有个BRAKER1examples，如果正确安装了可以使用下列命令进行测试

::

  perl braker.pl --genome=/path/to/examples/genome.fa --bam=/path/to/examples/RNAseq.bam


在一个普通电脑上（2.27 GHz单核CPU），大约需要 46 小时.


