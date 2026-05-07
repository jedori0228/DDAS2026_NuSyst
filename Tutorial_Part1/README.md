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

  # List of variations (e.g., X-sigma)
  # - DIALNAME_variation_descriptor: "(START_VALUE, END_VALUE, STEP)"
  # - DIALNAME_variation_descriptor: "[VALUE_0, VALUE_1, VALUE_2, ...]"
  DIALNAME_variation_descriptor: "[-1, +1]"

}

# You can include multiple ToolConfig block in a single file
# At the end, make a list out of them
syst_providers: [
  DDAS_ToolConfig
]
```

## ParameterHeader file

You can then run `GenerateSystProviderConfigNuSyst -c <ToolConfig FHICL file> -o <Output name for the ParameterHeader FHICL file>` to convert the ToolConfig file into a ParameterHeader file

