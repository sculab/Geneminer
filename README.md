
# 1. Overview
 
**Note:** We have developed **GeneMiner2**, a comprehensive analysis tool for the entire workflow from NGS raw data to phylogenetic tree construction, featuring a sophisticated graphical interface (currently supporting **Windows** and **macOS**). Please visit: [GitHub](https://github.com/sculab/GeneMiner2) or [Gitee](https://gitee.com/sculab/GeneMiner2) for details.

GeneMiner is a software for extracting phylogenetic markers from next-generation sequencing (NGS) data. (i) With GeneMiner, users can accurately and efficiently obtain a large number of phylogenetic markers  from NGS data at an economical cost. For example, extract all or part of mitochondrial/chloroplast genes and highly repetitive regions (e.g., nrDNA) in the nuclear genome from genome skimming data. Extract single-to-low-copy genes from transcriptome sequencing data, etc. GeneMiner broadens the choice of phylogenetic markers from the most basic data level. (ii) GeneMiner proposes a novel verification method based on the base substitution model and repetitive resampling, which can statistically evaluate the impact of the reference on the assembly. (iii) GeneMiner provides a cross-platform graphical interface and can be easily docked to downstream phylogenetic analysis processes. In addition, GeneMiner can be applied to the research of DNA barcode extraction, customs quarantine, specific functional gene exploration, and other research, which has broad application prospects.

## **Cite** 
- Xie PL, Guo YL, Teng Y, Zhou WB, Yu Y. 2024. GeneMiner: A tool for extracting phylogenetic markers from next-generation sequencing data. Molecular Ecology Resources: e13924. [doi:10.1111/1755-0998.13924](https://www.researchgate.net/publication/377298770_GeneMiner_A_tool_for_extracting_phylogenetic_markers_from_next-generation_sequencing_data)


For lightweight usage on **macOS**, you may also refer to Section 3.2, 'Running GeneMiner-GUI'.

## Dependencies

- [Python](https://www.python.org/downloads/) 3.6 or later, along with the Pysthon libraries.

- [biopython](http://biopython.org/wiki/Main_Page) 1.79 or later



# 2. Download and install

GeneMiner is an easy-to-use software written in python3, which is provided for x86-64 systems running GNU/Linux, macOS (version 10.13 or higher), and Windows (64-bit, version 7 or higher).

Users on Windows, macOS, and Linux can run GeneMiner directly from the command line. We also offer a more convenient GUI version for Windows and macOS users.

## 2.1 GeneMiner with Graphical User Interface (GUI)

For individuals who are not accustomed to utilizing the command line, we advise using the GUI version. 
We have specifically developed a enhanced **GeneMiner2**, for **Windows** and **macOS**. Please visit: [GitHub](https://github.com/sculab/GeneMiner2) or [Gitee](https://gitee.com/sculab/GeneMiner2) for details.

## 2.2 GeneMiner with command (cmd)

- option1  **Cloning the GitHub/Gitee repository**
- option2  **Source code installation**
- option3  **Flexible construction**



**Cloning the GitHub repository** 

Clone GeneMiner's repository from github or gitee:

```
git clone https://github.com/sculab/GeneMiner.git
```
or

```
git clone https://gitee.com/sculab/GeneMiner.git
```

```
cd GeneMiner
python setup.py install --record logName --user #Add 'geneminer.py' to the '$PATH' 
geneminer.py -h
```

If you want to remove `geneminer.py` from the `$PATH`, you can:

```
cat logName | xargs rm -rf  
```



**Source code installation**

 Download the source distribution from the [Github](https://github.com/sculab/Geneminer/releases/latest) and install dependencies:

```shell
wget -c https://github.com/sculab/GeneMiner/archive/refs/tags/geneminer_v1.1.tar.gz
tar geneminer_v1.1.tar.gz
cd  geneminer_v1.1
python setup.py install --record logName --user
geneminer.py -h
```

If you want to remove `geneminer.py` from the `$PATH`, you can:

```
cat logName | xargs rm -rf  
```



**Flexible construction**

If both of the above methods fail or you want to have a deeper control of GeneMiner, you can use a more flexible method.

- Download the GeneMiner's distribution from [Github](https://github.com/sculab/Geneminer/releases/latest) or [Gitee].

```shell
wget -c https://github.com/sculab/GeneMiner/archive/refs/tags/geneminer_v1.1.tar.gz
tar geneminer_v1.1.tar.gz
```

- Use the following commands to make `geneminer.py`  executable.

```shell
cd  geneminer_v1.1
chmod 755 geneminer.py
```

- Install python library `biopython` using pip or conda.

```shell
# install required libs
pip install biopython --user
```

- Add `geneminer.py` to the `$PATH`. The following is an example for linux users:

```shell
echo "export PATH=\$PATH:$(pwd)" >> ~/.bashrc  
source ~/.bashrc
geneminer.py -h
```



# 3. Running GeneMiner

GeneMiner takes the reference sequences and FASTQ format sequencing files as input and the recovered phylogenetic markers as output. We have prepared a simulated dataset of `Arabidopsis thaliana` to help you quickly grasp the main usage of GeneMiner. This dataset can be found in the **DEMO** folder.


## 3.1 Running GeneMiner command-line

- Mining single target gene from genome skimming data

```shell
geneminer.py -1 skimming_data1.fq.gz -2 skimming_data2.fq.gz -rtfa shallow_ref/ITS.fasta -o ITS_out
```

`-1`,`-2`:  the input files with paired-end reads, given in FASTQ format

`-rtfa`:  reference sequences in FASTA format

`-o`:        the output directory



- Mining multiple target genes using from genome skimming data

```shell
#for FASTA format
geneminer.py -1 skimming_data1.fq.gz -2 skimming_data2.fq.gz -rtfa shallow_ref/ -o out1 
#for GenBank format
geneminer.py -1 skimming_data1.fq.gz -2 skimming_data2.fq.gz -rtgb shallow_ref.gb -o out2
```

`-rtfa`:  reference sequences in FASTA format

`-rtgb`:   reference sequences in GenBank format



- Mining multiple target genes from genome skimming data and evaluating the assembly results.

```shell
geneminer.py -1 skimming_data1.fq.gz  -2 skimming_data2.fq.gz -rtfa shallow_ref/ -min 300 -max 5000 -t 4 -bn 20 -o out3
```

`min`: The minimum length of recovered target genes to be retained [default = 0]

`max`: The maximum length of recovered target genes to be retained [default = 5000]

`limit_count`: The minimum number of times a k-mer should appear in reads, used to remove likely erroneous and low-abundance k-mers [default = 2]

`-t`:   The number of threads

`-bn`:   Specify the bootstrap number. Evaluate the assembly results based on repeated resampling and base substitution model.



- Mining Angiosperms353 genes from  transcriptome data

```shell
geneminer.py -1 Arabidopsis_thaliana_sim1.fq.gz -2 Arabidopsis_thaliana_sim2.fq.gz -rtfa ref_Arabidopsis_353 -k1 31 -k2 41 -t 4 -rfi 1 -b 10000 -o Angiosperm353 
```

`k1`:  Length of kmer for filtering reads [default = 31]

`k2`:  Length of kmer for assembling reads [default = 41]

`rfi`: Enable (1) or disable (0) re-filtering [default = 1].

`-b`:  Length of the extension along both sides of the recovered target gene. Set to a large value (e.g. 10000) if you want to retain the complete assembly. Recommended length is 0.5 * reads length [default = 75]



## 3.2 Running GeneMiner-GUI

GeneMiner-GUI is straightforward, simple to use, and ideal for lightweight users. Considering the memory limit and the excessive time overhead, we recommend you utilize GeneMiner-cmd when you run large-scale data

For GeneMiner-GUI, you must set `Data1`, `Data2` or `Single reads` under the `Data` module, `Ref.(fasta)` or `Ref.(gb)` under the `Reference` module and  `Output Folder` under the `Outout` module as below:

![图片](https://github.com/sculab/GeneMiner/raw/main/imgs/gui_working.png)

**Run**

Set the parameters, then click on the `Run` botton

**View Results**

Click on the `Results` button to view the results after the GeneMiner has completed. 

Don't worry if you notice that the output text box in the GUI appears to be "paused." It's because some steps are time-consuming (e.g., filtering reads and assembly). You can click on the `Results` button to check the exact progress . To avoid interrupting the program, do not attempt to read or write the existing files. 

According to the processing order of GeneMiner, it will generate: `reference_database <folder>  `，`filtered_out <folder>`，`assembled_out <folder>`，`GM_results <folder>`，`bootstrap_out <folder> `（if using the `-bn` parameter),`results.csv <file>` in order.

**Clear the screen**

Click on the `Clear` botton

**Reset parameters**

Click on the `Reset` botton and all parameters will be reset to default parameters.

WorkFlow: Reset ==>> Enter new parameters ==>> Run

Note: Do not click on the  `Reset` botton while the program is running

**Close GeneMiner**

Click on the `Close` botton

Note: If closing GeneMiner's GUI does not kill the scripts in the background, you can open Task Manager to close GeneMiner's tasks.



## View results

Now, you can view the results of GeneMiner in the output directory.  

The output directory contains  `reference_database`、`filtered_out `、`assembled_out`、 `GM_results`、`bootstrap_out `  and `results.csv` 

`reference_database <folder>  `: Used to store reference sequences

`filtered_out <folder>`：Used to store filtered reads

`assembled_out <folder>`: Used to store assembled contigs 

`GM_results <folder>`:Used to store all the target genes mined by GeneMiner

`bootstrap_out <folder> ` :Used to store assessment of assembly results, If you have used the `-bn` parameter

`results.csv <file>` :Record various information about the recovered target genes



# 4. User manual

A more complete manual is here: https://github.com/sculab/GeneMiner/blob/main/GeneMiner_User_Guide.md

# 5. Contact

Please check [Geneminer's homepage](https://github.com/sculab/GeneMiner) first. If something is wrong when running GeneMiner, please do not be surprised and report it to us. We usually have quick response to bugs.

- Find Questions & Answers at [GeneMiner Discussions](https://github.com/sculab/GeneMiner/discussions/categories/q-a): **Recommended**
- Report Bugs & Issues at [GeneMiner Issues](https://github.com/sculab/GeneMiner/issues): Please avoid repetitive or irrelevant issues

**DO NOT** directly write to us with your questions, instead please post the questions **publicly**, using above platforms  Our emails (yyu@scu.edu.cn, 1791173948@qq.com) are only for receiving public question alert and private data (if applied) associated with those public questions. When you send your private data to us, enclose the email with a link where you posted the question. Our only reply emails will be a receiving confirmation, while our answers will be posted in a public place.



# 6. Citation

When you use GeneMiner please cite:

**GeneMiner : a tool for extracting phylogenetic markers from next-generation sequencing data**



