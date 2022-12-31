# Code for "Inplace Access to the Surface Code Y Basis"

This repository contains the source code used to generate and benchmark circuits for the paper "Inplace Access to the Surface Code Y Basis".
The circuits implement a variety of surface code memory experiments in the X, Y and Z bases.

In principle, the plots in the paper can be regenerated by running the following commands.
In practice, because the paper uses an internal decoder, you will have to use a different decoder.
The scripts are setup to use 'pymatching'.
This will give slightly worse results than in the paper, because pymatching does not do correlated decoding.

```bash
# STEP 0: SETUP ENVIRONMENT
# gnu-parallel is used by the circuit generation scripts
sudo apt install parallel
# This step heavily depends on your OS and your preferences for python environments.
# These specific instructions create a python 3.9 virtualenv assuming a debian-like linux.
sudo apt install python3.9-venv
python3 -m venv .venv
source .venv/bin/activate
# Install python dependencies into venv:
pip install -r requirements.txt

# STEP 1: MAKE CIRCUITS. (creates and populates out/circuits directory)
./step1_generate_circuits.sh

# Step 2: SAMPLE CIRCUITS. (creates out/stats.csv)
# NOTE: this script differs from what was done for the paper in the following ways:
#     - Uses 'pymatching' instead of 'internal_correlated' as the decoder
#     - Samples at most a million shots per circuit instead of up to a billion
#     - Samples at most a hundred errors per circuit instead of a thousand
./step2_collect_stats.sh

# STEP 3: PLOT RESULTS. (creates and populates out/plot directory)
./step3_plot_stats.sh
```
