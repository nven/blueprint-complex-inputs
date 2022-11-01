# Blueprints example of working with complex Terraform variables 

This is a starter example to demonstrate creating a simple scalable Blueprint from two linked Workspaces. It demonstrates working with complex Terraform input variables and resource data passing between the Workspaces. No cloud resources are created by this example.

Following Terraform resources are deployed:
- null-resource    (no IBM Cloud resources are deployed)

## Blueprint definition - complex-blueprint.yaml

The Blueprint creates two modules, linking two Terraform configs as Workspaces. 
- terraform_module1
- terraform_module1


The module TF configs are sourced from https://github.com/Cloud-Schematics/blueprint-example-modules
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
| boolean_var | string | null | true/false |
| list_any_flow_scalar | list(any) |  null |
| list_any_block_scalar | list(any) |  null |
| docker_ports  |  list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  null | Sample complex input variable | 



## Blueprints Outputs
The complex-blueprint.yaml definition creates the following outputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| nested_complex | list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  | Sample output dynamically created |

### Input file - complex-input.yaml
The input file defines the variable values for all the required Blueprint definition inputs. Review the file contents to observe the difference in formatting for the yaml scalar and block scalar formats. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| region | string | us-east | Sample var for resource deployment region |
| resource_group | string | default | Sample var for resource group |
| sample_var | string | testconfig | Sample var |
| boolean_var | string | false | Sample boolean var |
| list_any_flow_scalar | list(any) |  ["36", "mqm-grand", "madison-circle-garden"] | list in yaml scalar format  |
| list_any_block_scalar: | list(any) | [ <br>   "36",  <br>  "mqm-grand",  <br> "madison-circle-garden"  <br>   ] | list in yaml block scalar format |
| docker_ports  |  list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  [{  internal = 9900 <br>       external = 9900 <br>  protocol = "tcp" <br>   },  { <br>  internal = 9901 <br>  external = 9901 <br> protocol = "ldp" }] | Sample complex input variable | 



## Prerequisites
1. Install the Schematics CLI plugin by follow the instructions in the [documentation](https://cloud.ibm.com/docs/schematics?topic=schematics-setup-cli)  
2. Configure [IAM access permissions](https://cloud.ibm.com/docs/schematics?topic=schematics-access) for the Schematics Blueprints service. 
3. Set Schematics Target Region
The target (manage from) Schematics region for the Blueprint instance is determined by the IBM Cloud CLI target region. The region can be set with the `ibmcloud target` command.

## Usage 
The example command input here uses the minimum rights to create new resources. It assumes that the IBM Cloud resource group `Default` exists and the user has been granted Schematics access permissions to this group. 







The following parameters are used for the `blueprint create` configuration. 
- Name of the blueprint: `Blueprint_complex_inputs`
- Schematics management resource group: `Default`
- Blueprint URL: `https://github.com/Cloud-Schematics/blueprint-complex-inputs`
- Blueprint file: `complex-blueprint.yaml`
- Blueprint Git branch `main`
- Input file URL: `https://github.com/Cloud-Schematics/blueprint-complex-inputs`
- Input file: `complex-input.yaml` 
- Input file Git branch `main`


Example commands
```sh
$ ibmcloud target -r <region>

$ ibmcloud schematics blueprint create -name Blueprint_Complex -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs -bp-git-branch main -bp-git-file complex-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs -input-git-branch main -input-git-file complex-input.yaml 


$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint job list -id blueprint_id

$ ibmcloud schematics blueprint get -id blueprint_id -profile outputs

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```

## Next Steps

Looking for more samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). 

Check the example Readme files for further Blueprint customization and usage scenarios for each sample. 
