# NuSystematics for DUNE Data Analysis School 2026

Tutorial on NuSystematics package: https://github.com/NuSystematics/nusystematics
I am asuuming that you are on SL7 container on dunegpvm

# In every new terminal session, run below

```
# Current directory
export nusystwd=`pwd`

# To use UPS
source /cvmfs/icarus.opensciencegrid.org/products/icarus/setup_icarus.sh

# Dependencies
setup cmake v3_27_4
setup genie v3_04_02 -qe26:prof
setup genie_xsec   v3_04_00 -q AR2320i00000:e1000:k250
setup boost v1_82_0 -qe26:prof
setup eigen v23_08_01_66e8f
setup fhiclcpp v4_18_04 -qe26:prof
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
Again in every new terminal session, you need to run following bash scripts
```
cd ${nusystwd}
source ${nusystwd}/nusystematics-build/Linux/bin/setup.systematicstools.sh
source ${nusystwd}/nusystematics-build/Linux/bin/setup.nusystematics.sh
```

To quickly check whether the install was successful, run `DumpConfiguredTweaksNuSyst`, and if you see following "expected" error messsage, you are done!
```
$ DumpConfiguredTweaksNuSyst 
-c is not given, running without evaluating reweights
[ERROR]: Expected to be passed a -i option.
[USAGE]: DumpConfiguredTweaksNuSyst

	-?|--help        : Show this message.
	-c <config.fcl>  : fhicl file to read.
	-k <list key>    : fhicl key to look for parameter headers,
	                   "generated_systematic_provider_configuration"
	                   by default.
	-i <ghep.root>   : GENIE TChain descriptor to read events
	                   from. (n.b. quote wildcards).
	-b <NtpMCEventRecord branch name>   : Name of the NtpMCEventRecord branch (default:gmcrec)
	-N <NMax>        : Maximum number of events to process.
	-s <NSkip>       : Number of events to skip.
	-o <out.root>    : File to write validation canvases to.
```

# Part 1
Here we will run what NuSystematics package is

# Part 2
Here we will transfer the systematic reweights to the CAFs we used in the previous session
