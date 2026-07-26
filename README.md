# OpenSABR

OpenSABR stands for "Open-source System for Auditory Brainstem Response".
This repository was previously named "ABR2025".

## Introduction

This repository contains code to implement the ABR measuring system described
in [Gargiullo et al](https://www.biorxiv.org/content/10.64898/2025.12.17.694771v1), 
as well as the analysis scripts needed to replicate the figures in that paper.

The repository is structured as follows:
- `./` - The toplevel directory, containing this README file
- `./hardware_designs` - Design files, bill of materials, and other information about 
physically building the system.
- `./src/opensabr` - source code for analysis and GUI. Import like `import opensabr`
- `./src/opensabr/gui` - A Python module for running the graphical user interface (GUI) 
to take ABR data. It can be imported like `import opensabr.gui`. This code is 
run on a desktop PC connected to the ABR measuring system hardware.
- `./scripts` - A folder of demo scripts that generate the figures in the
paper, using data that we collected and have shared on Zenodo. These scripts 
are meant to be run one at a time, not imported and called by other scripts. 

## Requirements

We used Python 3.11. Newer versions may also work but we did not test them.
- pyproject.toml contains a list of the required dependencies
- requirements-lock.txt includes the exact versions we used

For the analysis scripts (but not the GUI), the following repository is 
required. It should be on your PYTHONPATH so that it can be imported using
the syntax `import my`.
- `git clone git@github.com:Rodgers-PAC-Lab/my.git`

## Instructions

To build the hardware, refer to the file `./hardware_designs/README.md`. 

To run the GUI and measure ABR, refer to the file `./gui/README`.

To run the analysis scripts and regenerate the figures included in the paper,
refer to the file `./scripts/README`.

## Installation of OpenSABR

A note about our package management approach:
- Repositories that are install-able (i.e., using a method like `pip install` or `python setup.py`) are cloned into `~/installed`
- Repositories that need to be importable (like `import reponame`) but are not install-able are cloned into `~/dev`, which is a directory on `$PYTHONPATH`. Two examples of repositories like this are ABRA and Rodgers-PAC-Lab:my.git

The reason for this distinction is because it can cause errors to mix these two kinds of repositories in the same directory.

### Clone the OpenSABR repo

Run the following commands:

```
cd ~/installed
git clone git@github.com:Rodgers-PAC-Lab/OpenSABR
cd OpenSABR
```

Then pick one of the following installation approaches.
- Option A is easier and does not pin any versions of dependencies, so you will get the newest available
- Option B will download the exact versions of the repositories we used. This is mainly useful for reproducing our results exactly. This approach will only work on linux, since that is what we used.

### Option A: Typical install (no version pins)

```
conda create -n opensabr -c conda-forge python=3.11 pip ipython
conda activate opensabr
conda install -c conda-forge numpy scipy pandas matplotlib tqdm pyqt pyqtgraph pyserial pyarrow seaborn
pip install -e . --no-deps
```

### Option B: Reproducible install from lock (linux-64 only)

```
conda create -n opensabr --file opensabr-conda-lock.txt
conda activate opensabr
pip install -e . --no-deps
```

Now you have installed opensabr. Try `import opensabr` to be sure.

You can stop here if you all you want to do is run or use OpenSABR. You only have to continue if you want to reproduce the comparisons to ABRpresto and ABRA that were made in the paper.

## Installation of other ABR software used as a comparison in the paper

In the paper we also compared our analysis software to two others. If you wish to reproduce this comparison, you will need to install them.

- ABRA, installed into a separate conda environment called "abra"
- ABRpresto, installed into a separate conda environment called "abrpresto"

You do not need to install these others if you just want to run OpenSABR.

### Installing ABRpresto

This is an "install-able" module. 

#### Clone repo

Run the following to install our fork. Our demo scripts require the use of our fork, which contains only minor modifications.

```
cd ~/installed 
git clone git@github.com:Rodgers-PAC-Lab/ABRpresto # our fork
cd ABRpresto
git checkout dev # our branch with fixes
```

Then pick one of the following installation approaches, A (flexible) or B (reproducible), analogous to the options above.

#### Option A: Typical install (no version pins)

```
conda create -n abrpresto -c conda-forge python=3.11 ipython pip
conda activate abrpresto
conda install -c conda-forge numpy scipy pandas matplotlib setuptools_scm pyarrow tqdm
pip install -e . --no-deps
```

#### Option B: Reproducible install from lock (linux-64 only)

```
conda create -n abrpresto --file abrpresto-conda-lock.txt
conda activate abrpresto
pip install -e . --no-deps
```

Now ABRpresto should be installed. Check with `import abrpresto`


### Installing ABRA

This is not an "install-able" module, but it is importable, so it need to go on PYTHONPATH.

#### Clone repo (must be under ~/dev or some location on PYTHONPATH)

```
cd ~/dev
git clone git@github.com:Rodgers-PAC-Lab/abranalysis # our fork
cd abranalysis
git checkout dev # our branch with fixes
```

As above, pick Option A or B.

#### Option A: Typical install (no version pins)

```
conda create -n abra -c conda-forge python=3.11 ipython pip
conda activate abra
conda install -c conda-forge numpy scipy pandas scikit-learn pytorch=*=cpu* keras tensorflow streamlit easydict "altair<5" pyarrow matplotlib tqdm
conda env config vars set PYTHONPATH=$HOME/dev
conda deactivate && conda activate abra   # reload so PYTHONPATH takes effect
```

#### Option B: Reproducible install from lock (linux-64 only)

```
conda create -n abra --file abra-conda-lock.txt
conda activate abra
conda env config vars set PYTHONPATH=$HOME/dev
conda deactivate && conda activate abra   # reload so PYTHONPATH takes effect
```

Now ABRA should be installed. Check with `import abranalyais`
