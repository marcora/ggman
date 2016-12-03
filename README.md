


# R package to create manhattan plots using ggplot 



- [R package to create manhattan plots using ggplot](#)
	- [Note:](#note)
	- [Installation](#installation)
	- [Usage](#usage)
		- [Example data](#example-data)
		- [Create a basic Manhattan plot](#create-a-basic-manhattan-plot)
		- [Add labels to Manhattan plot](#add-labels-to-manhattan-plot)
		- [Highlight markers](#highlight-markers)
		- [Add multiple highlights](#add-multiple-highlights)
		- [Manhattan plot with clumps](#manhattan-plot-with-clumps)
			- [Step 1](#step-1)
			- [Step 2](#step-2)
			- [Step 3](#step-3)
		- [Inverted Manhattan Plot](#inverted-manhattan-plot)
		- [Plot Odds Ratio instead of P values](#plot-odds-ratio-instead-of-p-values)

##Note: 
The package is currently under development.  Please raise issues for any bugs you identify.

## Installation

```
library(devtools)
install_github("veera-dr/ggman")
```

##Usage 

### Example data 

```
head(gwas)

  CHR     SNP BP      P     OR
1   1   rs1_0  1 0.2954 1.1050
2   1 rs1_0_M  2 0.9296 0.9922
3   1   rs1_1  3 0.8162 1.0160
4   1 rs1_1_M  4 0.6669 0.9719
5   1   rs1_2  5 0.4310 0.9426
6   1 rs1_2_M  6 0.3743 1.0580
>
```

### Create a basic Manhattan plot 

```
ggman(gwas, snp = "SNP", bp = "BP", chrom = "CHR", pvalue = "P")
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/manhattan.basic.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)

### Add labels to Manhattan plot 

```
p1 <- ggman(gwas, snp = "SNP", bp = "BP", chrom = "CHR", pvalue = "P")

gwas.sig <- gwas[-log10(gwas$P) > 8,]

ggmanLabel(p1,labelDfm = gwas.sig, snp = "SNP", label = "SNP")
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/manhattan.labelled.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)


### Highlight markers 

```
ggmanHighlight(p1, highlight = highlights, colour="red")
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/Manhattan.highlights.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)

### Add multiple highlights

```
p1 <- ggman(gwas, snp = "SNP", bp = "BP", chrom = "CHR", pvalue = "P")

## add first highlights
p2 <- ggmanHighlight(p1, highlight = highlights, colour = "red")

## Lets create some more vectors of SNPs to highlight

highlights2 <- with(gwas, SNP[grep("rs06",SNP)])
highlights3 <- with(gwas, SNP[grep("rs0011",SNP)])

## add highlights

p3 <- ggmanHighlight(p2, highlight = highlights2, colour = "orange")
p4 <- ggmanHighlight(p3, highlight = highlights3, colour = "green4")
p4
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/multi%20highlights.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)


### Manhattan plot with clumps 

It's possible to add clumps to the Manhattan plot. The following demonstration uses real data set from the paper den Hoed M et al (summary data available @ https://walker05.u.hpc.mssm.edu/ )

**Reference**: den Hoed M et al. Identification of heart rate-associated loci and their effects on cardiac conduction and rhythm disorders. Nat Genet 45 (6); 621-631

#### Step 1

Clump the summary file, using plink's `--clump` function.  Here, for the demonstration, the summary file was clumped using hapmap-phase-1 genotypes. The following settings were used.

```
--clump-p1 0.00000001
--clump-r2 0.1
--clump-kb 1000
``` 

#### Step 2

Create the `ggclumps` object, using the `ggClumps()` function. 

```
plink.clumps <- read.table("plink.clumped", header = TRUE)
plot.clumps <- ggClumps(plink.clumps)
```

#### Step 3

Create Manhattan plot along with clumps. 

```
ggman(hrgwas, clumps = plot.clumps, snp = "SNP", bp = "POS", chrom = "CHR", pvalue = "P",  ymin = 2)
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/hr.gwas.clumps.png)

Reference: Hoed M et al 2013, Nat Genet 45 (6); 621-631

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)


### Inverted Manhattan Plot

```
ggmanInvert(gwas, snp = "SNP", bp = "BP", chrom = "CHR", pvalue = "P", effect = "OR", method = "or")
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/inverted.manhattan.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)

### Plot Odds Ratio instead of P values

```
ggman(gwas, snp = "SNP", bp = "BP", pvalue = "OR", chrom = "CHR", logTransform = FALSE, ymax = 3, ylab = "Odds Ratio")
```

![enter image description here](https://github.com/veera-dr/ggman/blob/master/data/or_plot.png)

[go to top](#r-package-to-create-manhattan-plots-using-ggplot)
