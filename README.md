## Selection shapes synonymous stop codon use in mammals


Phylogenetic models of the evolution of protein-coding sequences can provide insights into the selection pressures that have shaped them. In the application of these models synonymous nucleotide substitutions, which do not alter the encoded amino acid, are often assumed to have limited functional consequences and used as a proxy for the neutral rate of evolution. The ratio of nonsynonymous to synonymous substitution rates is then used to categorize the selective regime that applies to the protein (e.g. purifying selection, neutral evolution, diversifying selection). Here, we extend these models to explore the extent of purifying selection acting on substitutions between synonymous stop codons. 

***

### R Scripts

We implemeted the stop-extended codon model in R and provide the following R scripts to fit and simulate from the model:
* Given a phylogenetic tree and an in-frame sequence alignment (that includes the stop codon as the last position of the alignment), obtain maximum likelihood estimates of model parameters
([stopcodon.rscript](https://github.com/cseoighe/StopEvol/blob/master/stopcodon.rscript)).
* Simulate a codon sequence alignment (including stop codon) from the stop-extended codon model, built on the Muse and Gaut codon model and the F1x4 model of codon equilibrium frequencies ([sim_mgf1x4.rscript](https://github.com/cseoighe/StopEvol/blob/master/sim_mgf1x4.rscript)). The script requires a phylogenetic tree (with branch lengths, defined as substitutions per codon site) and a textfile containing the parameters of the model,


***

### Getting Started

The R script [stopcodon.rscript](https://github.com/cseoighe/StopEvol/blob/master/stopcodon.rscript) requires a codon-aware sequence alignment file in FASTA format and a phylogenetic tree, with branch lengths, estimated from a cognate standard codon model (the default model implemented in the R script is the Muse and Gaut (1994) model with the F1x4 model of codon equilibrium frequencies).  

Note that relative branch lengths are treated as fixed and not reoptimized. The script optimizes a scale factor that applies to the overall tree length.

The sequence file should contain a codon-aware alignment in FASTA format and must include the 
stop codon as the last position of the alignment. Sequences that include a gap or a codon other than
a stop codon (i.e. sequences for which the stop codon is not positionally homologous with the last 
position in the alignment) are excluded.

The R script [sim_mgf1x4.rscript](https://github.com/cseoighe/StopEvol/blob/master/sim_mgf1x4.rscript) requires a paramter file and a phylogenetic tree file as arguments. The parameter file is a textfile containing the following 8 values on a single line: gene_name kappa omega pi_A pi_C pi_G pi_T required_alignment_length_in_codons. The treefile should contain branch lengths (in units of substitutions per codon site). Using these inputs, a sequence alignment is simulated from the stop-extended codon model, based on MG94 X HKY85 X F1x4.
***

### Prerequisites

**All Scripts:**
* Require "R" packages "ape" and "expm":

***

### Information

The following sections are broken down into:

* **Running the tests**
* **Break down into end to end tests**

For the following scripts 
* **[stopcodon.rscript](https://github.com/cseoighe/StopEvol/blob/master/stopcodon.rscript)**
<!-- * **[sim_gyempirical.rscript](https://github.com/cseoighe/StopEvol/blob/master/sim_gyempirical.rscript)** -->
* **[sim_mgf1x4.rscript](https://github.com/cseoighe/StopEvol/blob/master/sim_mgf1x4.rscript)**

***

### Running the tests
**[stopcodon.rscript](https://github.com/cseoighe/StopEvol/blob/master/stopcodon.rscript)**

The code is run on the command line using the following command:

```
Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>
```

Where "treefile.ph" contains a phylogenetic tree with branch lengths and "seqfile.fasta" is a codon-aware alignment.

***


### Break down into end to end tests

Ensure all data is in the required directory. Then enter this directory using the "cd" command in the linux command line followed by the path to the data directory.

```
cd /home/YourDataDirectory
```
Run the R script using the Rscript command.

```
Rscript stopcodon.rscript <treefile.ph> <seqfile.fasta>
```

**First Optimisation:** Four numbers will then appear in the terminal window. These show current estimates the parameters during the optimization. The parameters are: kappa, omega and treescale factor respectively, with initial starting values of 2 (kappa), 0.2 (omega) and 1 (tresscale). Kappa and omega are the transition-transversion rate ratio and the ratio of non-synonymous to synonymous substitutions, respectively. The treescale scaling factor parameter scales the tree length, to account for the use of a different rate matrix in the 64x64 stop-codon extended model when compared with the model from which the input tree branch lengths were obtained. The fourth and final value is the maximum likelihood value. In the initial optimization phi (the rate of substitution between stop codons relative to the synonymous substitutions between sense codons) is set to 1. 

**Second Optimisation:** After the first optimisation has converged, a second optimisation is performed, this time, optimising also over the phi variable. Progress of the optimisation is again reported to the terminal window (current values of kappa, omega, treescale and phi). The fifth and final value to appear is the maximum likelihood value.


**Output:** A file with the same name as <seqfile> but with .stopcodon appended. It contains the following: The maximum log likelihood and parameter estimates (kappa, omega, treescale, phi) from the second optimisation (above), delta_lnL (the difference in log likelihood between the first optimisation, with phi fixed, and the second optimization, with phi a free parameter) and a number (0/1) indicating the successful convergence of the second optimization. 

***


### Break down into end to end tests

Ensure all data is in the required directory. Then enter this directory using the "cd" command in the linux command line followed by the path to the data directory.

```
cd /home/YourDataDirectory
```
Run the R script using the Rscript command.

### Running the tests
**[sim_mgf1x4.rscript](https://github.com/cseoighe/StopEvol/blob/master/sim_mgf1x4.rscript)**

The code is run on the command line using the following command:

```
Rscript sim_mgf1x4.rscript treefile parameterfile
```

Where "paramterfile" is a text file containing the following 8 values on a single line: gene_name kappa omega pi_A pi_C pi_G pi_T (equilibrium frequencies) alignment_length_in_codons and the "treefile.ph" is a maximum likelihood estimate of the tree given the codon-aware alignment.

***


### Break down into end to end tests

Ensure all data is in the required directory. Then enter this directory using the "cd" command in the linux command line followed by the path to the data directory.

```
cd /home/YourDataDirectory
```
Run the R script using the Rscript command.

```
Rscript sim_mgf1x4.rscript treefile parameterfile
```

**Output:** A file called <gene_name>.sim.mgf1x4.fastas containing a simulated coding sequence alignment, including
stop codon.

```
<gene_name>.sim.mgf1x4.fastas
```
***
### Authors

* **Cathal Seoighe**
* **Stephen J. Kiniry**
* **Pavel V. Baranov**
* **Haixuan Yang**

***

### License

This project is licensed  - see the [LICENSE](LICENSE) file for details

***

### Acknowledgments
* **Andrew Peters (PhD)**

### Test 
![alt text](https://github.com/cseoighe/StopEvol/blob/master/Sim1.png)
