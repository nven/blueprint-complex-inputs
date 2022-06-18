# Blueprints example of working with complex Terraform variables 

This is a simple starter example to demonstrate the use of complex variables with Schematics Blueprints. 

Following resources are deployed:
- null-resource    (no IBM Cloud resources are deployed)

## Blueprint definition - complex-blueprint.yaml

The blueprint links two Terraform configs as Workspaces. 
- terraform_module1
- terraform_module1

TF configs are sourced from https://github.com/Cloud-Schematics/blueprint-example-modules
```
Blueprint file: complex-blueprint.yaml
├── terraform_module1
|    └── source: github.com/Cloud-Schematics/blueprint-example-modules/tf-inputs-outputs
└── terraform_module2
     └── source: github.com/Cloud-Schematics/blueprint-example-modules/tf-inputs-outputs
```

### Blueprint definition inputs 
The complex-blueprint.yaml definition file accepts the following inputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| region | string | null | Sample string var for resource deployment region |
| resource_group | string | null | Sample string var for resource group |
| sample_var | string | null | Sample string var |
| boolian_var | string | null | true/false |
| list_any_flow_scalar | list(any) |  null |
| list_any_block_scalar | list(any) |  null |
| docker_ports  |  list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  null | Sample complex input variable | 



## Blueprints Outputs
The complex-blueprint.yaml definition creates the following outputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| nested_complex | list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  | Sample output dynamically created |

### Input file - complex-input.yaml
The input file defines the variable values for all the required Blueprint definition inputs. Review the file contents to observe the difference in formating for the yaml scalar and block scalar formats. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| region | string | us-east | Sample var for resource deployment region |
| resource_group | string | default | Sample var for resource group |
| sample_var | string | testconfig | Sample var |
| boolian_var | string | false | Sample boolian var |
| list_any_flow_scalar | list(any) |  ["36", "mqm-grand", "madison-circle-garden"] | list in yaml scalar format  |
| list_any_block_scalar: | list(any) | [ <br>   "36",  <br>  "mqm-grand",  <br> "madison-circle-garden"  <br>   ] | list in yaml block scalar format |
| docker_ports  |  list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  [{  internal = 9900 <br>       external = 9900 <br>  protocol = "tcp" <br>   },  { <br>  internal = 9901 <br>  external = 9901 <br> protocol = "ldp" }] | Sample complex input variable | 





## Prerequisities
- Resource group `Default` exists for Schematics IAM access control. 




## Prerequisites
1. Install the Schematics CLI plugin by follow the instructions in the [documentation](https://cloud.ibm.com/docs/schematics?topic=schematics-setup-cli)  
2. Configure [IAM access permissions](https://cloud.ibm.com/docs/schematics?topic=schematics-access) for the Schematics Blueprints service. 
3. Set Schematics Target Region
The target (manage from) Schematics region for the Blueprint instance is determined by the IBM Cloud CLI target region. The region can be set with the `ibmcloud target` command.


## Usage 
**TEMPORARY USE OF payload_complex.json file from /test folder for CLI testing**

**copy payload_complex.json to CLI execution folder**  

Choose a Resource Group to associate the Blueprint with for Access Control. Available Resource Groups can be confirmed from the [Console Resource Groups](https://cloud.ibm.com/account/resource-groups) page.  

Depending on your account the RG in the payload_complex.json file may need changing to the name of an existing RG. Note some accounts have 'Default', others 'default'.  


```
$ ibmcloud target -r <region>

$ ibmcloud schematics blueprint create --file payload_complex.json
```

CLI flag support due 27th June 2022
```
$ ibmcloud schematics blueprint create 
-name=Blueprint_Complex
-resource_group=Default
-bp_git_url https://github.ibm.com/schematics-solution/blueprint-example-modules/complex_blueprint.yaml
-input_git_url https://github.ibm.com/schematics-solution/blueprint-example-modules/complex_input.yaml
```

```
$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint jobs -id blueprint_id

$ ibmcloud schematics blueprint get -id blueprint_id

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```

