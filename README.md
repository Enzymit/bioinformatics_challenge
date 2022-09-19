# Welcome to Enzymit's Grand Challenge!

## Background
Today you will help us investigate [Polyphosphate Glucokinases](https://en.wikipedia.org/wiki/Polyphosphate%E2%80%94glucose_phosphotransferase). These are enzymes that can catalyze the phosphorylation of glucose with polyphosphates. Most kinases, perform phosphorylation reactions with ATP, which is an expensive co-factor that imposes high costs on phosphorylation reactions in industrial settings. Having the ability to utilize polyphosphates for phosphorylation reactions can dramatically lower costs of many chemical reactions that are currently not cost effective.

Let's start!

## The Challenge

### Part 1: Initial Setup

1. First thing's first. Your first task is to clone this repo to your local machine (or the machine that you were provided with). You can do this by running the following command in your terminal:

    ```bash
    git clone <repo_url>
    ```

    where `<repo_url>` is the url of this repo. You can find this url by clicking on the green `Code` button on the top right of this page. You can also download the repo as a zip file by clicking on the `Download ZIP` button.

2. Next, you will need to install the dependencies for this project. You can do this by running the following command in your terminal:

    ```bash conda env create -f environment.yml```

    This will create a conda environment called `enzymit` with all the dependencies installed. You can activate this environment by running the following command:

    ```bash conda activate enzymit```

    You can deactivate this environment by running the following command:

    ```bash conda deactivate```

### Part 2: Data Acquisition

Now let's start to gather our data. In your repo, there's a DNA sequence of an enzyme that was identified as a potential polyphosphate glucokinase. The sequence is stored in the file `data/sequence.fasta`. Your first task is to find the protein structure that corresponds to this DNA sequence. 


{% note %}

**Note:** You can use the [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) tool to find the protein structure that corresponds to this DNA sequence.

{% endnote %}

