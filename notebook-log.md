Data will come from GenBank from the paper Liu and Smith from 2021.

For homework due 2-20-2024, I am still unsure what alignment method to use for my data. I believe that ClustalW would be best, but my laptop's operating system is too new to work with it. I have downloaded 25 nucleotides from GenBank all from the paper Liu and Smith. I do not plan on using T-Coffee because of Dr. Claudia's suggestion (since it would likely be the slowest or most complex). M-Coffee intrigued me but that seems too complex for right now. I plan on going to office hours next week when Dr. Claudia is back because I am confused on how I would know how to write a reproducible script based on an alignment method from the Alignathon paper.

<<<<<<< HEAD

Code used to make distance and parsimony trees using my data:
Below are the codes for both parsimony and distance methods. Neither are the most common used today (lieklihood methods). In these methods, you cannot infer timing as a measure of branch length, without further analysis and data. Parsimony is particularly complex and takes a lot more time than other methods.

install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)

install.packages("igraph", type="binary")

library(ape)
library(adegenet)
library(phangorn)


dna <- fasta2DNAbin(file="/Users/jwalston/Desktop/myproject/HeliamphoraNucleotides2.19.24")

D <- dist.dna(dna, model="TN93")
tre <- nj(D)
tre <- ladderize(tre)
plot(tre, cex=.6)
title("A simple NJ tree")

#second activity
dna <- fasta2DNAbin(file="/Users/jwalston/Desktop/myproject/HeliamphoraNucleotides2.19.24")
dna2 <- as.phyDat(dna)

tre.ini <- nj(dist.dna(dna,model="raw"))
parsimony(tre.ini, dna2)

tre.pars <- optim.parsimony(tre.ini, dna2)

plot(tre.pars, cex=0.6)
=======
(clustalw_env) Josephs-MacBook-Pro-2:data jwalston$ clustalw2 -ALIGN -INFILE=primatesAA.fasta -OUTFILE=primatesAA-aligned.fasta -OUTPUT=FASTA
>>>>>>> d96ba2c758810ec99983f5b7b96332fafad01807


(clustalw_env) Josephs-MacBook-Pro-2:Desktop jwalston$ clustalw2 -ALIGN -INFILE=data/HeliamphoraNucleotides2.19.24 -OUTFILE=HeliamphoraNucleotidesAligned.fasta -OUTPUT=FASTA

./raxml-ng --check --msa Heliamphoranucelotides2.19.24.fa --model GTR+G
./raxml-ng --check --msa Heliamphoranucelotides2.19.24.fa/bad.fa.raxml.reduced.phy --model GTR+G


3/21/24- I am continuing to have difficulty with getting this code to work. No matter where I put my nucelotide text file, I keep getting an error message that clustalw cannot find it; I think the error comes from the miniconda/clustalw environment. I had to do this step because I needed to align my data correctly. I chose RAXml because it is faster than IQtree according to some literature.
