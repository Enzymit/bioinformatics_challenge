# Welcome to Enzymit's Grand Challenge!

## Background
Today you will help us investigate [Polyphosphate Glucokinases](https://en.wikipedia.org/wiki/Polyphosphate%E2%80%94glucose_phosphotransferase). These are enzymes that can catalyze the phosphorylation of glucose with polyphosphates. Most kinases, perform phosphorylation reactions with ATP, which is an expensive co-factor that imposes high costs on phosphorylation reactions in industrial settings. Having the ability to utilize polyphosphates for phosphorylation reactions can dramatically lower costs of many chemical reactions that are currently not cost effective.

Let's start!

## The Challenge

### Part 1: Initial Setup

1. First thing's first. Your first task is to clone this repo to your local machine (or the machine that you were provided with). You can do this by running the following command in your terminal:

    ```
    git clone <repo_url>
    ```

    where `<repo_url>` is the url of this repo. You can find this url by clicking on the green `Code` button on the top right of this page. You can also download the repo as a zip file by clicking on the `Download ZIP` button.

2. Next, you will need to install the dependencies for this project. You can do this by running the following command in your terminal:

    ``` conda env create -f environment.yml```

    This will create a conda environment called `enzymit` with all the dependencies installed. You can activate this environment by running the following command:

    ``` conda activate enzymit```

    You can deactivate this environment by running the following command:

    ``` conda deactivate```

### Part 2: Data Acquisition

Now let's start to gather our data. 
1. In your repo, there's a DNA sequence of an enzyme that was identified as a potential polyphosphate glucokinase. The sequence is stored in the file `data/sequence.fasta`. Your first task is to find the protein structure that corresponds to this DNA sequence. 
We'll develop the short pipeline programatically using the package [Bioservices](https://github.com/cokelaer/bioservices) and [Biopython](https://https://biopython.org/) Those are python packages that will enable us to work with various biological databases in one scripted pipeline. 

1. Let's use the [Blast](https://blast.ncbi.nlm.nih.gov/Blast.cgi) service to find the protein structure that corresponds to this DNA sequence. You may use the [NCBI](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast) service in BioServices to do this. 

Several tips:
- Use the PDB as the database to search against. The PDB Blast database contain protein sequences, you'll need to use the appropriate blast application to search for a protein sequence that matches your **DNA** sequence.
- The query may take some time to complete. In order to avoid running it multiple times, you may cache it and skip the Blast phase once you have it.
- Since the query may take several minutes to run, you will need to query the job status every few seconds in a loop (or use the ```wait``` function of NCBIblast). Use the ```get_status(jobid)``` of the Ncbiblast service to do this. You can find more information about this method [here](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast.get_status).
- Choose stype="dna" in the run function
- Once the job reaches the status of FINISHED, you can retrieve the results using the ```get_results(jobid,result_type='ids')``` method of the Ncbiblast service. You can find more information about this method [here](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast.get_results).
The results will be a string of PDB ids separated by newline characters:
```
PDB:1A0A_A
PDB:1A0A_B
PDB:3VGL_A
...
```
Choose the first PDB id in the list, as it is the most similar to your DNA sequence.
2. Use the urllib package to download the PDB file directly from RCSB. The URL is of the form: ```https://files.rcsb.org/download/<PDB_ID>.pdb```
You can find more information about this package [here](https://docs.python.org/3/library/urllib.html).

3. Use the [Biopython](https://biopython.org/) package to parse the PDB file and extract the sequence of the protein. You can find more information about this package [here](https://biopython.org/wiki/Documentation). Look for the PDBParser class.

4. Extract Chain A from the Structure object. (Hint: use the ```[0]``` operator on the structure object to get the first model, and then use the ```['A']``` operator on the model object to get the first chain).

5. Search for all the phosphate molecules in the chain. (Hint: use the ```get_residues``` method of the chain object to get all the residues in the chain. Then use the ```filter``` method of the atoms object to filter the residues by their name. The phosphate molecules appears in the PDB file as "PO4".

6. Next, save as a CSV list of all the residues that interact with the phosphate molecules (Interaction is defined by the residues being < 4A away from the phosphate molecule, and are of a type that is capable of interacting with the charged phosphate). Hint: Use the ```NeighborSearch``` class from Biopython's Bio.PDB. The CSV should look like:

```
RESIDUE_NAME,RESIDUE_NUM,DISTANCE
ASN,1,3.2
...
```

