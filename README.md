
# LocusZoom Installation Guide

Welcome to the installation guide for LocusZoom. This guide will walk you through setting up LocusZoom on your system using a Conda environment and the necessary configuration steps.

## Prerequisites

Before starting, ensure you have Conda installed on your system. If not, install it from [Miniconda](https://docs.conda.io/en/latest/miniconda.html) or [Anaconda](https://www.anaconda.com/products/individual).

## Step 1: Clone the Repository

First, clone the repository to get the `environment.yml` file and other necessary resources:

```bash
git clone https://github.com/ApurvaChitre/locuszoom_standalone_installation.git
cd locuszoom_standalone_installation
```

## Step 2: Create and Activate the Conda Environment

Create the Conda environment using the `environment.yml` file provided in the repository:

```bash
conda env create -f environment.yml
conda activate locuszoom_standalone
```

## Step 3: Download and Install LocusZoom

Download the latest version of LocusZoom from the official source:

```bash
wget --no-check-certificate https://statgen.sph.umich.edu/locuszoom/download/locuszoom_1.4.tgz
tar -xzvf locuszoom_1.4.tgz
cd locuszoom
mkdir -p conf
```

## Step 4: Configure LocusZoom

Navigate to the `conf` directory and create the configuration file `m2zfast.conf` dynamically:

```bash
cd conf
install_dir=$(pwd)/..
conda_env="locuszoom_standalone"
rscript_path=$(conda run -n $conda_env which Rscript)

echo "
# Required programs.
METAL2ZOOM_PATH = "$install_dir/locuszoom_1.4/src/locuszoom.R"
RSCRIPT_PATH = "$rscript_path"
NEWFUGUE_PATH = "new_fugue"
PLINK_PATH = "plink"
TABIX_PATH = "tabix"

# SQLite database settings.
SQLITE_DB = {
  'rn7' : "$install_dir/databases/rn7.db"
}

LATEST_BUILD = 'rn7'

# GWAS catalog files
GWAS_CATS = {
  'rn7' : {
    'whole-cat_significant-only' : {
      'file' : "$install_dir/data/gwas_catalog/gwas_catalog_rn7.txt",
      'desc' : "The NHGRI GWAS catalog, filtered to SNPs with p-value < 5E-08"
    }
  }
}

# Location of genotypes to use for LD calculations.
LD_DB = {
  'dummy': {
    'rn7': {
      'EUR': {
        'bim_dir': 'dummy_path'
      }
    }
  }
}
" > m2zfast.conf
```

## Step 5: Test the Installation

Ensure that LocusZoom is properly installed and configured by running:

```bash
../bin/locuszoom --version
```

This should display the version of LocusZoom installed.

## Conclusion

Congratulations! You have successfully installed and configured LocusZoom. If you encounter any issues, please open an issue on this repository's [Issues page](https://github.com/ApurvaChitre/locuszoom_standalone_installation/issues).

## Note on SQLite Database

This installation uses a pre-made SQLite database for demonstration purposes. For detailed instructions on setting up your own databases or custom configurations, please refer to the [LocusZoom Standalone documentation on the wiki page](https://genome.sph.umich.edu/wiki/LocusZoom_Standalone).
