# Part 1: Understanding the package

# Writing a configuration file
A FHICL file ("ParameterHeader") is used to initiate nusystematics weight provider. It is sometime not stratightforward writing a ParameterHeader file from scratch. We first write a simpler FHICL file called "ToolConfig", and then use a NuSyst script, `GenerateSystProviderConfigNuSyst`, to convert it into a ParameterHeader file.

## ToolConfig file

An example of a ToolConfig file is written as "DDAS.TC.fcl":
```
DDAS_ToolConfig:{

  # Name of the systprovider
  tool_type: "DUNEDAS2026ExampleReweighter"

  # Some name
  instance_name: "NuSystTutorial"

  # Central value
  DIALNAME_central_value: 0
  # List of variations (e.g., X-sigma)
  # - DIALNAME_variation_descriptor: "(START_VALUE, END_VALUE, STEP)"
  # - DIALNAME_variation_descriptor: "[VALUE_0, VALUE_1, VALUE_2, ...]"
  DIALNAME_variation_descriptor: "[+3]"

}

# You can include multiple ToolConfig block in a single file
# At the end, make a list out of them
syst_providers: [
  DDAS_ToolConfig
]
```

## ParameterHeader file

You can then run `GenerateSystProviderConfigNuSyst -c <ToolConfig FHICL file> -o <Output name for the ParameterHeader FHICL file>` to convert the ToolConfig file into a ParameterHeader file

Each systprovider module contains a member function `BuildSystMetaData` that should be defined by the module developer, which parses the ToolConfig contents. 

You may have noticed following line printed from `GenerateSystProviderConfigNuSyst`:
```
[DUNEDAS2026ExampleReweighter::BuildSystMetaData] Called
[DUNEDAS2026ExampleReweighter::BuildSystMetaData] No dial is set
```
Let's see what we have in BuildSystMetaData; https://github.com/NuSystematics/nusystematics/blob/DDAS2026/src/nusystematics/systproviders/DUNEDAS2026ExampleReweighter_tool.cc#L23-L47$0
```
SystMetaData DUNEDAS2026ExampleReweighter::BuildSystMetaData(ParameterSet const &cfg,
                                                     paramId_t firstId) {

  std::cout << "[DUNEDAS2026ExampleReweighter::BuildSystMetaData] Called" << std::endl;

  SystMetaData smd;

  // Name of the dials that are supported by this module
  std::vector<std::string> AvailPNames = {"DialA", "DialB"};

  // Loop over available names and check if they are specified in ToolConfig
  for(std::string const &pname: AvailPNames){
    systtools::SystParamHeader phdr;
    if (ParseFhiclToolConfigurationParameter(cfg, pname, phdr, firstId)) {
      printf("[DUNEDAS2026ExampleReweighter::BuildSystMetaData] %s is found from ToolConfig\n", pname.c_str());
      phdr.systParamId = firstId++;
      smd.push_back(phdr);
    }
  }
  if(smd.size()==0){
    std::cout << "[DUNEDAS2026ExampleReweighter::BuildSystMetaData] No dial is set" << std::endl;
  }
...
```
As you can see, this module supports {"DialA", "DialB"}, but we have `DIALNAME` in current ToolConfig file.
 
## :pencil2: Exercise 1-1

Update "DDAS.TC.fcl" and genearate a ParameterHeader file for following purpose:
1. DialA
  1. Central value: 0
  1. We want reweights for -1 and +1 sigmas
1. DialB
  1. Central value: 0
  1. We want reweights for -1 and +1 sigmas

If you see
```
[DUNEDAS2026ExampleReweighter::BuildSystMetaData] Called
[DUNEDAS2026ExampleReweighter::BuildSystMetaData] DialA is found from ToolConfig
[DUNEDAS2026ExampleReweighter::BuildSystMetaData] DialB is found from ToolConfig
```
, you are good!

# Running NuSystematics

We now have ParamterHeader file, so we can run NuSystematics. Various downstream packages (CAFMaker, Fitter, etc..) can be used, but NuSystematics provides a simple TreeMaker; "DumpConfiguredTweaksNuSyst":
```
DumpConfiguredTweaksNuSyst -c DDAS.PH.fcl \
-i <Input ROOT file that contains GENIE tree> \
-o NuSystTree.root \
```
The output ROOT file contains two TTrees;
- "events": Event tree
- "tweak_metadata": Metadata contains dial information

# Analyzing NuSyst TreeMaker

Let's go to [NuSystTreeAna.ipynb][NuSystTreeAna.ipynb]

