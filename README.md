#Partitioned Bremer Support script for TNT


## Introduction
This is a script to be run in the software [TNT](http://www.zmuc.dk/public/phylogeny/tnt/) for phylogenetic inference by Goloboff et al.
This script performs a **Partitioned Bremer Support** analysis for N number of partitions.

You will find some documentation and troubleshooting here: [http://nymphalidae.utu.fi/cpena/software.html](http://nymphalidae.utu.fi/cpena/software.html)


If you have a dataset with several partitions (morphological and molecular data or several gene sequences) and you want to calculate the Partitioned Bremer Support (PBS) values for nodes using the Cladistic software TNT, you may want to try out this **pbsup.run** script.

It seems that there is more than one way to calculate Bremer support and Partitioned Bremer Support values. This script uses the methodology proposed by:  
Gatesy, J. et al. 1999. Cladistics, 15, 271-313. [doi:10.1111/j.1096-0031.1999.tb00268.x](http://dx.doi.org/10.1111/j.1096-0031.1999.tb00268.x)

This script was first used in our paper:

* **Peña, C., Wahlberg, N., Weingartner, E., Kodandaramaiah, U., Nylin, S., Freitas, A. V. L., & Brower, A. V. Z.** (2006). Higher level phylogeny of Satyrinae butterflies (Lepidoptera: Nymphalidae) based on DNA sequence data. Molecular Phylogenetics and Evolution, 40(1):, 29-49. [doi:10.1016/j.ympev.2006.02.007](http://dx.doi.org/10.1016/j.ympev.2006.02.007)

**Please cite it if you find the pbsup.run script useful.**


## How to use it
1. Your data should be in a file named ``dataset.tnt``
2. Each of your partitions should be in a different "block". It doesn't work if you have all your partitions in one continuous line. Thus, your data should be interleaved. It is important to precede each block with &[dna] or &[num] (if your partition is molecular data). Something like this:

    nstates dna  
    xread  
    'Exported by .......'  
    4494 95  

    &[dna]  
    Aus_sp1 ACTAGACAGGATTA  
    Aus_sp2 AGCAGAGCCAATAA  
    ...  
    ...  

    &[dna]  
    Aus_sp1 AACTTATATTGATAGCA  
    Aus_sp2 AAGACGATAGACAGTAT  
    ...  
    ;  
    proc/;  

3. Find all your most parsimonious trees and save them in parenthetical notation in a file named alltrees.tre -> Commands: tsave *alltrees.tre; save; tsave/;
4. Calculate a strict consensus tree and save it (Commands: nelsen*; tsave *base.tre; save/; tsave/;) in a file named base.tre
5. All files (including the script pbsup.run should be in the same folder.
6. Enter TNT and type in the command line: pbsup N;
7. N is the number of partitions you have in your dataset.
8. When the script is done, you will see a new file pbs.out which is actually the strict consensus tree with the Partitioned Bremer Support values attached to their respective node.
9. To see the tree and PBS values, enter TNT, open your dataset, load your pbs.out tree (Command proc pbs.out) and type the command ttag; or selecting Trees/MultipleTags/ShowSave if you are using the windows point and click version of TNT.


##Troubleshooting
If you have big datasets (more than 250 taxa) you will need to increase the RAM memory allocated to TNT. By default TNT uses only 16MB of your computer's RAM memory.

1. For example, you can give up to 200MB of RAM to TNT with the command:    mxram 200;
2. For 300 taxa, the pbsup.run script needs to use little more than 600 array cells (by default around 350 are available). Use this command to set up to 15 loops and 2000 array cells:    macro*15 2000;
3. You can include these commands in your scripts provided that they are executed before macros are enabled and before you read your data (otherwise the data will be lost and the macro* command will not take effect).
4. The algorithms and parameters for the tree search strategy is in the script's line 55, you can change it according to your needs:
xmult: level 10 fuse 5 drift 30 rss css xss rat 50;
5. You might notice that the sum of PBS values doesn't exactly match the Bremer values. The sum might be off by 0.1-0.3 units. The most likely reason is that the pbsup.run does a rounding to 1 decimal during the calculations. One way to fix this is to change the rounding in the script to 2 or 3 decimals. For example, in the script, look for the code:
macfloat 1 
and change it to
macfloat 3 
(it appears twice in the code).