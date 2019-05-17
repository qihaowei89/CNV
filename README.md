# 介绍
本脚本基于R语言的exomCopy包,它主要是用来分析WGS和WES数据的CNV(拷贝数变异).根据脚本实际使用情况,修改了软件包中内置函数exomCopy.

修改部分:注释exomCopy函数代码中的24~27行
```
25  # if (length(seqlevelsInUse(gr)) != 1) {
26  #   stop("The current gr argument has ranges over the following chromosomes: ",paste(seqlevelsInUse(gr),collapse=", "),".\n The gr argument should contain ranges over a single chromosome.\n You can pass exomeCopy a GRanges object with ranges from a single chromosome with this syntax: gr[seqnames(gr) == 'chr1'].\n See vignette for example of running on multiple chromosomes.")
27  # }

```



# 分析脚本

## １．构建control数据集

**`Rscript scr/cnv_controlset_ctDNA.R  [options]`**

**`Usage: 
    Rscript scr/cnv_controlset_ctDNA.R \   
    -B control_set/bam_list \   
    -b target_range_bed/ctDNA_103genes_20180702.bed \   
    -r human_g1k_v37.fasta \   
    -o test `**
    

```
    Options:
        -B BAMLIST, --bamlist=BAMLIST
                control bam file list

        -b BED, --bed=BED
                target region bed file

        -r REF, --ref=REF
                reference genome xxx.fasta file

        -o OUTDIR, --outdir=OUTDIR
                output directory

        -h, --help
                Show this help message and exit
        
```
**准备bamlist文件，构建control集的bam文件路径，格式示例如下**
```
/home/wqh/workdir/bam/control_bam_/ct_1013_bai_1.target.bam
/home/wqh/workdir/bam/control_bam_/ct_1013_bai_2.target.bam
/home/wqh/workdir/bam/control_bam_/ct_1013_bai_3.target.bam
/home/wqh/workdir/bam/control_bam_/ct_1013_bai_4.target.bam
．．．
```



## ２．样本分析

**`Rscript scr/cnv_ctDNA.R  [options]`**

**`Usage: 
    Rscript scr/cnv_controlset_ctDNA.R \   
    -B sample_ID.target.bam \   
    -b target_range_bed/ctDNA_103genes_20180702.bed \   
    -s sample_ID \   
    -c control_set/cnv_controlset_ctDNA.RData \    
    -t 2.5 \   
    -o test `**
    
```
Options:
        -B BAM, --bam=BAM
                input bam file

        -b BED, --bed=BED
                target region bed file

        -s SAMPLE, --sample=SAMPLE
                sample name

        -c CONTROLSET, --controlset=CONTROLSET
                control set

        -o OUTDIR, --outdir=OUTDIR
                output directory

        -t THRESHOLD, --threshold=THRESHOLD
                copy number threshold to output [default 3]

        -h, --help
                Show this help message and exit
```



# 结果文件示例 
## xxx.copy_num.txt   
文件各列分别表示样本名称,bin区域所在的染色体号,bin起始位置,bin终止位置,bin长度,bin所在的基因,in_house算法计算CNV,exomeCopy计算CNV,HMM预测CNV
```
sample                      chromosome  ranges.start  ranges.end  ranges.width  gene  simple_CNV  exomeCopy_CNV  HMM_CNV
Ct19020067_1021_ZZ19010043  1           11169302      11169421    120           MTOR  0.5         0.8            1
Ct19020067_1021_ZZ19010043  1           11169668      11169787    120           MTOR  0.7         1              1
Ct19020067_1021_ZZ19010043  1           11174331      11174450    120           MTOR  0.7         0.9            1
Ct19020067_1021_ZZ19010043  1           11174360      11174479    120           MTOR  0.7         1              1
Ct19020067_1021_ZZ19010043  1           11177037      11177156    120           MTOR  0.5         0.7            1
Ct19020067_1021_ZZ19010043  1           11181314      11181433    120           MTOR  0.7         0.8            1
Ct19020067_1021_ZZ19010043  1           11181359      11181478    120           MTOR  0.5         0.8            1
Ct19020067_1021_ZZ19010043  1           11182099      11182218    120           MTOR  0.6         0.7            1
Ct19020067_1021_ZZ19010043  1           11184514      11184633    120           MTOR  0.6         0.9            1

```

## xxx.ExomeCopy_CNV.xls

文件各列分别表示样本名称,染色体号,基因坐标区间,基因名称,in_house算法计算CNV,exomeCopy计算CNV,HMM预测CNV
```
#Sample     Chromosome      Range   Gene    Simple_cnv      Exomecopy_cnv   Bin_count
Ct19020067_1021_ZZ19010043      4       [1803517, 1808850]      FGFR3   8.25    5.7     12
Ct19020067_1021_ZZ19010043      7       [92247382, 92463269]    CDK6    5.3     3       13
Ct19020067_1021_ZZ19010043      9       [21968151, 21975127]    CDKN2A  5.45    3.05    10
Ct19020067_1021_ZZ19010043      10      [43601910, 43617489]    RET     2.9     2.8     62
Ct19020067_1021_ZZ19010043      11      [69456078, 69467055]    CCND1   4.65    4.45    12
Ct19020067_1021_ZZ19010043      11      [533408, 534348]        HRAS    7.4     7       5
Ct19020067_1021_ZZ19010043      14      [105239307, 105246611]  AKT1    4.1     3.9     15
Ct19020067_1021_ZZ19010043      16      [2106659, 2136901]      TSC2    4       4.2     11
Ct19020067_1021_ZZ19010043      19      [1220311, 1226590]      STK11   7.3     4.7     9
```

##  xx.CNVs.plots/文件夹 
文件夹下是基因CNV的散点图



# CNV
