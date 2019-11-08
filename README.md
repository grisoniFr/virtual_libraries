# Generating customized compound libraries for drug discovery with machine intelligence


## Table of Contents
1. [Description](#Description)
2. [Requirements](#Requirements)
3. [How to run an example experiment](#Run)
4. [How to run an experiment on your own data](#OwnData)
5. [What can be found in the results?](#Results)
    1. [A figure displaying results as in the paper](#Results1)
    2. [An interactive UMAP plot that can be opened in the browser](#Results2)
    3. [The generated novo molecules by the chemical language model](#Results3)
6. [How is the pipeline working?](#Pipeline)
7. [FAQ](#FAQ)
8. [How to cite this work](#Cite)
9. [License](#license)


### Description<a name="Description"></a>

This is the supporting code for the paper «Generating customized compound libraries for drug discovery with machine intelligence»

**Abstract of the paper**: Generative machine learning models sample drug-like molecules from chemical space without the need for explicit design rules. 
A deep learning framework for customized compound library generation is presented, aiming to enrich and expand the pharmacologically relevant chemical 
space with new molecular entities ‘on demand’. This de novo design approach was used to generate molecules that combine features from bioactive synthetic 
compounds and natural products, which are a primary source of inspiration for drug discovery. The results show that the data-driven machine intelligence 
acquires implicit chemical knowledge and generates novel molecules with bespoke properties and structural diversity. The method is available as an 
open-access tool for medicinal and bioorganic chemistry.    

**Keywords**: artificial intelligence; drug design; generative model; natural product; neural network; language model

### Requirements<a name="Requirements"></a>

First, you need to clone the repo:

```
git clone git@gitlab.ethz.ch:moretm/virtual_libraries.git
```
Then, you can run the following command which will create a conda virtual environement and install all the needed packages:   

If you are working with a Mac:
```
cd virtual_libraries/
sh install_mac.sh
```
If you are working with a linux system:
```
cd virtual_libraries/
sh install_linux.sh
```

Once the installation is done, you can activate the virtual conda environment for this project:

```
conda activate mollib
```
Please note that you will need to activate this virtual conda environement every time you want to use this project. 

### How to run an example experiment<a name="Run"></a>

Now, you can run an example experiment, *e.g.*, the one with five dissimilar molecules from MEGx in the paper:

```
cd experiments/
sh run_morty.sh ../data/paper_five_dissimilar_MEGx.txt
```

The results of the analysis can be found in experiments/results/paper_five_dissimilar_MEGx/

### How to run an experiment on your own data<a name="OwnData"></a>

To apply transfer learning on your own set of molecules, you will need a *.txt* file with one SMILES string per line.
Then, you can just run the following command:

```
sh run_morty.sh {path/to/your/file.txt}
```

You will find the results of your experiment in experiments/results/{name_of_your_file}/

### What can be found in the results folder?<a name="Results"></a>

##### 1. A figure displaying the results, as in the paper:<a name="Results1"></a>
(in *experiments/results/{name_of_your_file}/resume/resume_0.7.jpg*)   
**a.** Fréchet ChemNet Distance (FCD) plot showing how the generated molecules evolved during transfer learning with respect to the source space (ChEMBL24)
and the target space (in the paper: MEGx natural products).  
**b.** Boxplot displaying the evolution of the CSP3 fraction during transfer learning.   
**c.** Uniform Manifold Approximation and Projection (UMAP), which displays molecules from the training set (ChEMBL24) used to pretrain the 
chemical language model (CLM),
molecules from the target set (MEGx), molecules generated from the pretrained CLM, molecules generated at the last epoch of transfer learning and finally
molecules of the transfert learning set.  
**d.** The five most common scaffolds for which molecules were generated, along with their scaled Shannon entropy (SSE) 
and the time during which they were present (in percent with respect to all sampled molecules at the given epoch).

##### 2. An interactive UMAP plot that can be opened in the browser:<a name="Results2"></a>
When the example run has finished, the interactive map of chemical space can be found there: *experiments/results/paper_five_dissimilar_MEGx/umap/interative_umap.html* 
or in *experiments/results/{name_of_your_file}/umap/interative_umap.html* if you ran an experiment on your own data). 
You can open this html file with a web browser. This interactive map allows you to zoom in our out and
to see some properties of the molecules when you'll pass over them with your pointer. Finally, if you click on a molecule, 
it will open it on http://molview.org where you can see its structure.   

##### 3. The generated novo molecules by the chemical language model:<a name="Results3"></a>
You can find the list of molecules (.txt file with one SMILES string per line) in *experiments/results/{name_of_your_file}/novo_molecules/*. 
Each of those molecules are grammatically valid and not found in the training set. Molecules were sampled every 10 epochs
(10, 20, 30 and 40) during transfer learning.


### How is the pipeline working?<a name="Pipeline"></a>

In experiments/ you can find all the files used to process the data, train the chemical language model and analyze the results.
All *do_{something}.py* files are where the actual code resides. All those files are run from a bash script called *run_morty.sh*, 
which executes other bash scripts called *run_do_{something}.sh*. Each of those *run_do_{something}.sh* files takes care of the compute pipeline, 
namely (i) preprocessing, (ii) training, (iii) data generation, and (iv) data analysis. You can use any of these scripts seperately if needed.   
The parameters of the neural network, how to do the processing, etc ., are defined in the file *parameters.init*. 
If you want, you can play with it. However, keep in mind that you will need access to a GPU if you wish to (re)train a language model, 
which is already provided here (otherwise, it will take a very long time to run the full pipeline). 
Note that there are some additional parameters and helper functions defined in *src/python/*.

### FAQ<a name="FAQ"></a>

*Do I need a GPU to run this code?*   
No, you don't. We already trained the chemical language model, which is the part for which you really need a GPU. Even though it might take some time to 
sample new molecules, you can do it on your own computer – which was one of our goals.

*I have never used a terminal, nor do I know what Git really is. What should I do?*   
If your goal is just to use our project to generate molecules, then we suggest that you use the release we made on 
Code Ocean (*TODO* when manuscript accepted). There, you have the possibility to run the code without
having to install anything.

*Can I use this code if I work with Windows*?   
We did not test our code with Windows. If you try, please let us know what was the modifications you had to make to the requirements such that
everybody can benefit from your experience! 
 
### How to cite this work<a name="Cite"></a>
TODO: update when published.

### License<a name="License"></a>
[MIT License](LICENSE)
