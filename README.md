# Blueprints all in one example to deploy a simple Blueprints environment

This is a simple all-in-one starter example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to link multiple workspaces, passing variables between workspaces and returning an output from deploying the blueprint.  

The [5.3 Full stack IKS (all in one) tutorial](https://github.ibm.com/schematics-solution/schematics-blueprints/wiki/5.3-Full-stack-vsi-(all-in-one)-tutorial) on the Blueprints wiki can be followed as a guideline of how to deploy this example.  

Following types of resources are deployed:
- null-resource    (no IBM Cloud resources are deployed)

The blueprint is comprised of two linked workitems. These create two Schematics Workspaces
- terraform-workitem1 
- terraform-workitem2


The blueprint demonstrates deploying a full application stack composed from 4 components. 

The logical structure and file naming of this Blueprints example is illustrated below. All definitions and Terraform code are included within this one repo. 

```
env-starter.yaml                                 (blueprint environment definition)
├── config-starter.yaml                          (config)
└── blueprintdef-starter.yaml                    (blueprint definition)
    └── tf-inputs-outputs                        (Terraform config)
```

## Blueprint Environment Inputs
| Name | Type | Value | Description |
|------|------|------|----------------|
| name | string | Blueprints Starter Sample | |
| location| string | us-south | Schematics manage from region |
| resource_group | string | Default | Resource group for Schematics IAM access control |


## Config Inputs

| Name | Type | Value | Description |
|------|------|------|----------------|
| region | string | us-east | Sample var for resource deployment region |
| resource_group_name | string | default | Sample var for resource group |
| sample_var | string | testconfig | Sample var |
| list_any_flow_scalar | list(any) |  ["36", "mqm-grand", "madison-circle-garden"] |
| docker_ports  |  list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  [{  internal = 9900 <br>       external = 9900 <br>  protocol = "tcp" <br>   },  { <br>  internal = 9901 <br>  external = 9901 <br> protocol = "ldp" }] | Sample complex input variable | 

## Blueprints Outputs

| Name | Type | Value | Description |
|------|------|------|----------------|
| nested_complex | list(object({  <br> internal = number <br> external = number <br> protocol = string }) |  | Sample output dynamically created |



## Prerequisities
- Resource group `Default` exists for Schematics IAM access control. 


## Usage 
This example can be deployed as is. Alternatively modify the example as per the instructions below. 

To deploy in us-south, the IBM Cloud CLI must be configured with us-south as the target region. 


```
$ ibmcloud schematics environment external -repo-url https://github.ibm.com/schematics-solution/blueprints-starter -env-file env-starter.yaml -profile detailed

$ ibmcloud target -r us-south


$ ibmcloud schematics environment create

$ ibmcloud schematics job run -rt env -n init -id environment_id

$ ibmcloud schematics environment get -id environment_id -profile detailed

$ ibmcloud schematics job run -rt env -n install -id environment_id

$ ibmcloud schematics environment get -id environment_id -profile variables

$ ibmcloud schematics job run -rt env -n destroy -id environment_id

$ ibmcloud schematics job run -rt env -n delete -id environment_id
```

## Forking and modifying the example
To experiment with Blueprints, fork this example to a personal repo. Any of the definitions or TF configs can be modified. Follow the instructions in the Blueprints wiki for [Modifying the Sample App](https://github.ibm.com/schematics-solution/schematics-blueprints/wiki/5.4-Tutorial-Modifying-the-Environment-Examples) Use the git url of the forked repo as input to the `ibmcloud schematics environment external` command. 

Outline instructions:
- Fork the repo
- Replace all occurances of `schematics-solution` in the env and config files with the new github org. 
- Modify config inputs for your environment
- Proceed as per the tutorial


