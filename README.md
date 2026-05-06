# NuSystematics for DUNE Data Analysis School 2026

Tutorial on NuSystematics package: https://github.com/NuSystematics/nusystematics
I am asuuming that you are on SL7 container on dunegpvm

# In every new terminal session, run below

```
# Current directory
export nusystwd=`pwd`

# To use UPS
source /cvmfs/icarus.opensciencegrid.org/products/icarus/setup_icarus.sh

# cmake
setup cmake v3_21_4

# for fhicl
setup boost v1_80_0 -q e20:prof

# GENIE
setup genie v3_04_00 -q e20:prof
# GENIE cross section for AR23_20i tune
setup genie_xsec v3_04_00 -q AR2320i00000:e1000:k250
```

# Installing nusystematics package

```
# Getting the source from github
# - We will clone it into nusystematics-src directory
# - We will checkout to DDAS2026 branch
git clone https://github.com/NuSystematics/nusystematics.git nusystematics-src -b DDAS2026

# Creating directory for the build
mkdir nusystematics-build
cd nusystematics-build

# Run cmake
cmake ../nusystematics-src/

# Compile and install
make install
```

The package is installed in `nusystematics-build/Linux`. 

# Basic setups to use NuSystematics

```

```
