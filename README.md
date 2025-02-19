# some little tools for bioinformatics

python在服务器上画图，开头添加
```python
import matplotlib
matplotlib.use('agg')
```

### cal-RNA-file.py
Statistical RNA-Seq data alignment results(hisat2/star/magic-blast/tophat2)<br/>
python ~/cal-RNA-file.py input.sam/bam

### flt_bam.py
split hisat2 alignment file to un/uniq/multi-map file
python flt_bam.py input.bam/sam output_prefix

### overlap.pl
Calculate the distance between two intervals(chr1   start1  end1    chr2    start2  end2)<br/>
perl overlap.pl bedpe.bed

### compare_exon_intron_coverage.py
if we don't know what type of file(DNA or RNA),wo can detect it through comparing exon and intron coverage,if it RNA file,the exon have higher coverage <br/>
python compare_exon_intron_coverage.py

### reptile.136book.py
using urllib and bs4 to get the story from 136book.com<br/>
python reptile.136book.py

### word_cloud_test.py
using jieba and pyecharts to generate the word cloud<br/>
python word_cloud_test.py

### count2FPKM.py
using htseq-count output and gene exon length to calculator the [FPKM and FPKM-UQ](https://docs.gdc.cancer.gov/Data/Bioinformatics_Pipelines/Expression_mRNA_Pipeline/)<br/>
python count2FPKM htseq.count exon_gene_length.txt out.txt<br/>
different with TCGA:<br/>
- the total reads just use protein_coding gene to calculate the total reads and Q75 reads<br/>
count2FPKMforMatrix.py<br/>
针对多样本矩阵形式的count转化成FPKM<br/>
python count2FPKMforMatrix.py combine.gene.flt.count exon_gene_cg_length combine.gene.flt.count.fpkm

### annopeak.py
using bedtools to annotation peak<br/>
python annopeak.py peak/allrich-06.DNA.promoter.overlap.bed peak/allrich-06.DNA.genebody.overlap.bed peak/allrich-06.DNA.exon.overlap.bed peak/allrich-06.DNA.terminal.overlap.bed > peak/allrich-06.DNA.anno.result.txt<br>
future word<br/>
- using pybedtools to do this work.

### parsegtf.py
if use stringtie or cufflinks to assemble,the output gtf file don't contain the gene line.this script is used to add it.<br/>
python parsegtf.py MH63.seedling.nove.annotated.gtf > MH63.seedling.nove.cg.gtf

### combine_fastq.py
对于RNA-Seq,如果是两个重复数据(生物学重复,两个 library)。在第一个文件里的某个 reads 它会比对到基因组的某个位置,然后再将两个文件直接合并,这个同样的 reads 有一定的概率比对到其它的位置,可能这些 RNA-Seq 比对工具(tophat/hisat/star,已验证,有数据集在百度云)会根据 insert size 评估比对的结果导致的。所以合并两个RNA-Seq的时候建议 A-B-A-B-A-B 的形式合并而不是A-A-...-A-A-B-B-...-B的合并。<br/>
python combine.py test1.fq test2.fq test.fq<br/>
待做的<br/>
- 没有考虑数据大小的差异，如果A文件时B文件的n倍大,那么应该时 nA-B-nA-B 的形式。

### bam2fastq.py
对按照name排序的bam文件转化成fastq文件 <br/>
$ `samtools sort -n -o file.sorted.bam file.bam` <br/>
$ `python bam2fq.py ORP-113.multimap.bam ORP-113.multimap`<br/>
将bam/sam文件转化成fastq文件，支持单端比对/双端比对。如果输入的是双端数据,双端数据中成对的结果保存在ORP-113.multimap.N.fq中,如果是单端数据则在ORP-113.multimap.fq中,混合数据也同理

### divide_bam.py
python divide_bam.py -i test.bam -t bwamem -f bam<br/>
将bam/sam文件分成uniq,multi,unmap,lowuniq四类，并且统计<br/>

### UniqFileBam.py
python UniqFileBam.py -t bowtie2 -i mappings.bowtie2.sam -o test2.sam -q 20 -f True<br/>
提取bam/sam文件中的unique比对的结果，支持 bwa-aln bwa-mem bowtie2 hisat/hisat2 STAR (后续用过别的软件持续更新)<br/>

### UniqFileBamAsPair.py
python UniqFileBamAsPair.py -t bowtie2 -i mappings.bowtie2.sam -o test2.sam -q 20 -f True<br/>
提取bam/sam文件中的unique比对的结果（双端都成对的unique map），支持 bwa-aln bwa-mem bowtie2 hisat/hisat2 STAR (后续用过别的软件持续更新)<br/>

### htseq.py
python htseq.py<br/>
修改自htseq-count,可以同时计算sense(yes),no-sense(no),anti-sense(reverse)的结果,目前只适合单端数据,日后也没有添加双端的计划,STAR的count的结果默认就有三种<br/>

### split_bam_SE.py
python split_bam_SE.py input.bam input <br/>
根据指定的文件行数分割文件，暂时没有提供接口，目前都是切分1000000左右<br/>

### split_bam_PE.py
python split_bam_SE.py input.bam input <br/>
类上的split_bam_SE.py，根据指定的文件行数分割文件，暂时没有提供接口，目前都是切分1000000左右<br/>

### add_count.py
python add_count.py input 1 39 > total.count <br/>
通过上面的 split_bam 将文件分割，然后用 htseq-count 统计基因的表达数量，通过该软件合并起来<br/>

### CalculateCoveragePerBinsize.py
python CalculateCoveragePerBinsize.py hg38.100K.bin 100000 RUNX1.bedpe RUNX1.bedpe.bedgraph <br/>
对bed文件计算给定binsize的bedgraph结果<br/>

### SplitNLineBam.py
python SplitNLineBam.py input.bam 100000 outprefix <br/>
将给定的sam/bam文件拆分成多个bam文件，如果中间涉及了多位点比对，则文件必须按照name排序，然后有些文件会稍微多余给定的值<br/>

### downSample.py
python downSample.py -i ../7_reads_annotation/A549.CTCF.DNARNA.bedpe -n 15013571 -o A549.CTCF.DNARNA.bedpe -s 123456789 <br/>
对按行的文件取子样本<br/>

### fingBigData.sh
bash fingBigData.sh > user.text.size.txt <br/>
寻找大文件，代码中是寻找大于4G以上的文件，可以自定义 <br/>

### kmer.py
python kmer.py 100 10 full.fa genome.fastq.gz <br/>
将基因组文件以10bp为步长，提取100bp长度的序列片段，生成fastq文件<br/>

### codelibrary.java
java的常用工具集<br/>

### pair2single.py
python pair2single.py input.bam R1.bam R2.bam <br/>
将双端比对的结果拆分成单端比对的结果<br/>

### getRegionCoverage.py
python getRegionCoverage.py peak.file alignfile.bw peak.coverage <br/>
计算指定区域的平均测序深度<br/>

### remove_duplicated_reads.py/remove_duplicated_reads_SE.py
python remove_duplicated_reads.py infile1.fastq.gz infile2.fastq.gz uniq1.fastq.gz uniq2.fastq.gz <br/>
python remove_duplicated_reads_SE.py infile.fastq.gz uniq.fastq.gz <br/>
对测序数据去除PCR/光学重复<br/>

### bedpe2matrix_normal.all.py
python bedpe2matrix_normal.all.py input.1M.bed input.1M.mat output.1M.pdf <br/>
python cal.py -p input.uniq.bedpe -g genome.size -b 1000000 -o output.1M <br/>
计算HiC或者类似数据的交互矩阵，并画图 <br/>
```
input.1M.bed
chr1	0	1000000	1
chr1	1000000	2000000	2
chr1	2000000	3000000	3
chr1	3000000	4000000	4
chr1	4000000	5000000	5
chr1	5000000	6000000	6

input.1M.mat
1	1	241
1	2	10
1	3	24
1	4	4
1	5	21
1	6	8
1	7	4
1	8	10
```

### mergeGeneCountFile.py
python mergeGeneCountFile.py combine.count sample1.gene.count sample2.gene.count sample2.gene.count <br/>
合并多个样本的gene表达情况 <br/>
```
MH01g0010000	119
MH01g0010100	882
MH01g0010200	26
MH01g0010300	32
MH01g0010400	184
MH01g0010500	302
MH01g0010600	422
MH01g0010700	416
MH01g0010900	19
MH01g0011000	3387
```

### CombinationPermutations.java
计算排列组合 <br/>

### RandomSampling.java
随机抽样，用区间树来代替超大型列表，节省内存 <br/>

### RandomSampling.py
numpy自带的 <br/>

### RNA-seq-pipeline.sh

## 绘图
#### Box Plot
###### boxplot.1.r
要绘制离群点，请注释掉outlier.colour=NA。特别在绘制基因表达量的时候，因为需要取log，所以请绘制两边，用原始值的wilcoxon test的结果替换取对数后的结果。<br/>
[R中均值方法比较的详细描述](http://www.sthda.com/english/wiki/comparing-means-in-r)。<br/>
![just plot box plot](img/box.plot.1.png)<br/>
```
compare_means(len ~ supp, data = ToothGrowth)
.y: 用于检验的y变量
p: p值
p.adj: 矫正p值，默认矫正方法p.adjust.method="holm"
p.format: 格式化p值
p.signif: 显著性水平
方法： 用于比较组别的统计检验
```

#### Pie Plot
###### pieplot.1.r
![just plot plot](img/pie.plot.1.png)

#### Scatter Plot
###### scatterplot.1.r
![just scatter plot](img/scatter.plot.1.png)

#### Bar Plot
###### barplot.1.r
![just scatter plot](img/bar.plot.1.png)

#### Line Plot
###### lineplot.1.r
其中带宽参数，带宽越大，曲线越光滑，默认带宽为1，可以通过adjust参数进行调整。
![just line plot](img/line.plot.1.png)

#### Density Plot
###### density.plot.1.r
带柱子的密度曲线<br/>
Rscript density.plot.1.r density.plot.1.txt density.plot.1.txt.pdf 14425<br/>
该示例是我将交互随机模拟产生了500次，然后统计每次的结果得到density.plot.1.txt文件。14425是实际数据得到的结果，用于说明差异。然后以后用截断坐标轴画出来就更完美了（目前不会~~~~）<br/>
![just density plot](img/density.plot.1.png)

#### PCA Plot
###### plotPCA.1.r
三维的PCA分析图<br/>
Rscript plotPCA.1.r<br/>
![just PCA plot](img/PCA.plot.1.png)

#### Interaction Plot
###### [interactionplot.1.r](plotInteractionCurve/)
不知道为什么cyto文件不能着色，日后改进<br/>
![just interaction plot](img/interactionplot.1.png)

## 处理fasta格式的文件(所有的文件自动识别压缩后缀.gz .gzip)
#### 将大的fasta格式的文件拆分成指定大小的fasta小文件
python splitFasta.py MH63.nove.ios.ijoux.rna.fa 1000 test <br/>
输入文件，每个文件包含的序列条数，输出前缀

#### 将fasta格式的文件转化成fastq格式
python fasta2fastq.py in.fa out.fq<br/>
输入fasta文件，输出fastq文件

#### 序列比对（基于动态规划）
GlobalAlignment.py 全局比对，一般不用<br/>
LocalAlignment.py 局部比对，但是只匹配一次<br/>
LocalAlignmentFull.py 局部比对，配对到不能匹配为止<br/>
ChIAPETLinkerFilter 因为纯python跑的太慢了，用java重构了，专门为ChIA-PET数据过滤linker<br/>

## 处理bam/sam格式的文件(所有的文件自动识别后缀)
#### bam2bed.py
将bam或者sam文件转化成bed格式，方便自定义。目前只给出了一些必要的信息。 <br/>
python bam2bed.py in.bam/in.sam out.bed <br/>

## 处理bed格式的文件(所有的文件自动识别后缀)
#### recordDistance.py
对已经排过序的bed文件，查看region之间的距离（查看peak之间的距离分布） <br/>
python recordDistance.py file.peak distance.txt <br/>

#### filterMACS2nolambda.py
查看某个区域，以及该区域附近区域的覆盖度情况，用于peak过滤，和下面的处理coverage文件中的代码相似 <br/>
python filterMACS2nolambda.py sample_peaks.narrowPeak sample.bw sample_peaks.cal.narrowPeak > filterMACS2nolambda.log <br/>

#### mergePeak.py
去掉一些距离很近的不显著的peak <br/>
python mergePeak.py sample_peaks.cal.flt.narrowPeak 1000 sample_peaks.cal.flt.out.narrowPeak sample_peaks.cal.flt.merge.narrowPeak <br/>

## 处理coverage文件(所有的文件自动识别压缩后缀.gz .gzip .bigwig)
#### 将多个bedgraph/bigwig文件合并成一个（未来将改为多线程实现）
测试的每10M用时27s左右，需要用多线程来加速实现。
```
cat file1.bedgraph file2.bedgraph file3.bedgraph | sort -k1,1 -k2,2n -k3,3n > test.bed
bgzip test.bed
tabix test.bed.gz
python bed2bedgraph.py test.size test.bed.gz 1 4 combine.bedgraph
```
python bed2bedgraph.py test.size test.bed.gz 1 4 combine.bedgraph <br/>
test.size: 染色体大小 <br/>
```
chr1   1000
chr2   40000
```
test.bed.gz: bed4 format(chrom, start, end, coverage) <br/>
8: 步长，可以设置为1，会花很多时间，取决于input的步长设置的多少 <br/>
4: 计算的方式，设置为0，则每一行的记录视作coverage为1，若设置为正整数n，则计算的coverage是第n列的值 <br/>
combine.bedgraph: 输出文件

## lncRNA calling
```
step1: 除去rRNA序列
hisat2 -x rRNA.index -1 read1.fq.gz -2 read2.fq.gz -p 5 | samtools view -Sb -o out.alignrRNA.bam
python getNonrRNAFastq.py out.alignrRNA.bam read1.fq.gz read2.fq.gz nonrRNA.read1.fq nonrRNA.read2.fq && pigz -p 5 nonrRNA.read1.fq && pigz -p 5 nonrRNA.read2.fq

step2: 比对到参考基因组
hisat2 --rna-strandness RF -x genome.index -1 nonrRNA.read1.fq.gz -2 nonrRNA.read2.fq.gz -p 5 | samtools view -Sb - | samtools sort -@ 5 -o out.aligngenome.bam -

stringTie也会利用多位点比对的结果，单端的比对（唯一比对和多位点比对都行）
如果没有特殊需求，可以把这些都纳入进来进行组装，否则就过滤。（过滤步骤可省略）
# 去掉所有多位点的比对结果，因为hisat2比对结果得分只有0，1，60，60的都是唯一比对的。所以可以如下过滤
samtools view -Sb -q 20 out.aligngenome.flt.bam out.aligngenome.bam && cp out.aligngenome.flt.bam out.aligngenome.bam
# 也可以进一步过滤到只剩配对唯一比对的结果进行组装（可以对产生的九个文件分别组装看看结果）
python split_hisat2_bam.py out.aligngenome.bam out.aligngenome && cp out.aligngenome.uniq-uniq.bam out.aligngenome.bam

step3: 组装转录本
stringtie out.aligngenome.bam -G genome.gtf -o out.gtf --rf

step4: 合并转录本
stringtie --merge -G genome.gtf -o out.merge.gtf out1.gtf out2.gtf out3.gtf ...

step5: 与参考注释的转录本比较（gffcompare等价于cuffcompare）
gffcompare -r genome.gtf -s genome.fa -o out.nove -V -D out.merge.gtf

step6: 筛选一些待定转录本
awk '$3=="i"||$3=="j"||$3=="o"||$3=="u"||$3=="x"' out.nove.MH63.merge.gtf.tmap > out.nove.ios.ijoux.tmap
python get_gtf_file.py out.nove.ios.ijoux.tmap out.nove.annotated.gtf > out.nove.ios.ijoux.gtf
gffread out.nove.ios.ijoux.gtf -g genome.fa -w out.nove.ios.ijoux.fa

step7: 若为动物（植物亦可），可以用CPC2，CNCI，PLEK，LGC，PfamScan（主要是好安装）
# 过滤长度小于200nt的转录本，过滤ORF长于100aa(300nt)的转录本（可用ncbi的ORFfinder）（操作省略）
ORFfinder -in out.nove.ios.ijoux.fa -g 1 -s 0 -n yes -strand plus -out out.nove.ios.ijoux.ncbi.orf -outfmt 0 2> out.log
python ./bin/CPC2.py -i out.nove.ios.ijoux.fa -o out.nove.ios.ijoux.CPC2.txt (提取noncoding)
python CNCI/CNCI.py -f out.nove.ios.ijoux.fa -o out.nove.ios.ijoux.CNCI -m ve -p 30 (脊椎动物:ve 植物: pl) (提取 noncoding)
python PLEK.py -fasta out.nove.ios.ijoux.fa -out out.nove.ios.ijoux.PLEK.txt -thread 3 -isoutmsg 1 -isrmtempfile 0 （提取 Non-coding）
python lgc-1.0.py -p 2 out.nove.ios.ijoux.fa out.nove.ios.ijoux.LGC.txt （提取 Non-coding）
perl pfam_scan.pl -fasta out.nove.ios.ijoux.ncbi.orf -dir ~/Tools/Pfamdatabase/ -outfile out.nove.ios.ijoux.ncbi.orf.PfamScan.txt -cpu 30
虽然提到了四个预测软件：CPC2，CNCI，PLEK，LGC，但其实各自的结果重叠的情况没有那么的好。可以根据实际情况来判断。
个人比较认可CPC2+PfamScan的结果。

step8: 若为植物，可用plncpro来鉴定
python prediction.py -i out.nove.ios.ijoux.fa -p out.nove.ios.ijoux.pred_res -o ./plncpro -m $plncpro_1.1/models/monocot.model -d $plncpro_1.1/lib/blastdb/sprot -t 4 -v --min_len 200
python predstoseq.py -f out.nove.ios.ijoux.fa -o ./plncpro/out.nove.ios.ijoux.pred_res.flt.fa -p ./plncpro/out.nove.ios.ijoux.pred_res
interproscan.sh --appl Pfam -t n -dp -i out.nove.ios.ijoux.fa --iprlookup -f tsv -o ./plncpro/out.nove.ios.ijoux.interproscan -cpu 4
interproscan 跑的很慢，建议切分文件为小文件再跑，有提供切分脚本splitFasta.py
```
#### split_hisat2_bam.py
python split_hisat2_bam.py input.bam input <br/>
针对hisat2的比对结果拆分bam文件为9个文件：unmap-unmap,unmap-uniq,unmap-multi,uniq-unmap,uniq-uniq,uniq-multi,multi-unmap,multi-uniq,multi-multi <br/>

#### bwaalignclass.py
python split_hisat2_bam.py input.bam input <br/>
类似于split_hisat2_bam.py <br/>

#### get_gtf_file.py
python get_gtf_file.py out.nove.ios.ijoux.tmap out.nove.annotated.gtf > out.nove.ios.ijoux.gtf <br/>
根据列表提取gtf信息 <br/>

#### get_gtf_gene.file.py
python get_gtf_gene.file.py out.nove.ios.ijoux.rna.gtf > out.nove.ios.ijoux.rna.cg.gtf <br/>
不管是cufflinks，cuffmerge，cuffcompare，stringtie, stringtie --merge, 执行过后的gtf文件均是不含有gene那一行信息的。在执行某些软件的时候会有问题，可以添加回去。 <br/>

## HiC
#### 提取HiC中的高级交互（三个点的交互，四个点的，五个点的，甚至更多）
```
# step1 预处理，过滤掉接头和低质量的reads片段
java -jar ~/Tools/Trimmomatic-0.36/trimmomatic-0.36.jar PE -phred33 -threads 8 \
SRR1658586_1.fastq.gz SRR1658586_2.fastq.gz trim/SRR1658586_R1.clean.fq.gz trim/SRR1658586_R1.unclean.fq.gz trim/SRR1658586_R2.clean.fq.gz trim/SRR1658586_R2.unclean.fq.gz \
ILLUMINACLIP:~/Tools/Trimmomatic-0.36/adapters/TruSeq3-PE.fa:2:30:10:8:true LEADING:5 TRAILING:5 SLIDINGWINDOW:4:15 MINLEN:40
# MINLEN:40 设置40是因为SRR1658586这个数据中有很多reads压根就没那么长，只有43bp长。一般最好设置70以上。因为比对用的bwa mem。

# step2 比对
bwa mem -t 10 -S -P -5 -T 10 refdata-hg19-2.1.0/fasta/genome.fa trim/SRR1658586_R1.clean.fq.gz trim/SRR1658586_R2.clean.fq.gz | samtools view -@ 5 -Sb -o align/${i}.tmp.0.bam -
# 双端的单端比对模式。

# step3 提取含有supplementary reads含有supplementary的reads才有可能是
python classHiChighOrder.py SRR1658586.bam SRR1658586
# SRR1658586.drop.bam 没有supplementary的read且有至少一端是不唯一比对的（只有一个比对位置）（绝对没有交互）
# SRR1658586.two.bam 没有supplementary的read且双端是唯一比对的（有两个比对位置）（需要判断是不是在同一个酶切片段里面）
# SRR1658586.super.bam 含有supplementary（才有超过两个比对位点的可能）（需要判断是不是在同一个酶切片段里面）
# 从SRR1658586.two.bam和SRR1658586.super.bam是可以提取两两交互的。

# step4 将*super.bam文件进一步分成有两个以下片段数的结果，两个片段，超过两个
python getMoreInteraction.py bam和SRR1658586.super.bam bam和SRR1658586.super > bam和SRR1658586.super.log
# 后面可以直接将classHiChighOrder.py和getMoreInteraction.py合并。


```
## Peak Calling
#### 用macs2 call peak
```
# For ATAC-seq/DNase-seq/Faire-seq
macs2 callpeak -t aligndir/$i.flt.rm.bam -f BAM -g $GENOME_LEN -n peak/$i.narrow --trackline -B --verbose 3 --SPMR --shift -100 --extsize 200 --nomodel --keep-dup all
macs2 callpeak -t aligndir/$i.flt.rm.bam -f BAM -g $GENOME_LEN -n peak/$i.broad --trackline -B --verbose 3 --SPMR --broad --shift -100 --extsize 200 --nomodel --keep-dup all

# For ChIP-seq
macs2 callpeak -t align/$i.rmd.bam -c control.bam -f BAM -g 3.8E8 -n peak/$i --verbose 3 --trackline -B --SPMR --call-summits --keep-dup all (--broad)

重要参数：
-f：当是双端数据时，可以设置为bampe，影响很大(但是通用的是bam)
--keep-dup：如果已经去过PCR重复，就设置。
--broad：当peak宽度比较宽的时候(针对H3K9me2这种至少在KB级别的)
--nolambda：数据质量比较好的时候不考虑，当数据质量一般，call不出来什么peak的时候，请设置这个参数。固定的背景，不会动态调整。
-g：是指有效长度(非N的长度和非repeats长度)，影响不大，个人一般统计非N长度。
grep -v ">" genome.fa | perl -lane 'BEGIN{$sum=0}{$_=~s/[nN]+//g;$sum+=length($_)}END{print $sum}'
```
Narrow Peaks Format：bed6+4的格式，染色体、起点、终点、peak_id、score(Macs2输出的是int(-l0*log10qvalue))、strand(macs为 . )、signalvalue(macs为fold enrichment)、-log10(pvalue)、-log10(qvalue)、summit距离起点的距离<br/>
Broad Peaks Format：bed6+3的格式,剪掉了上面的最后一列<br/>

当使用--nolambda后，需要对peak稍微过滤一下。
```
python filterMACS2nolambda.py sample_peaks.narrowPeak sample.bw sample_peaks.cal.narrowPeak > filterMACS2nolambda.log
awk '($11>$14*1.5 || $11>$13*1.5 || $11>$12*1.5)' sample_peaks.cal.narrowPeak > sample_peaks.cal.flt.narrowPeak
python mergePeak.py sample_peaks.cal.flt.narrowPeak 1000 sample_peaks.cal.flt.out.narrowPeak sample_peaks.cal.flt.merge.narrowPeak
```
在这里，是指peak区域的coverage是本地背景上下游1Kb，5Kb，10Kb coverage的1.5倍以上，实际可做调整。<br/>
去掉一些距离特别近，看起来就不那么显著的peak。<br/>