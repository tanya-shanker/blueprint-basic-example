# Simple Blueprint and blueprint example

This is a simple example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to deploy a simple 2 module solution stack using linked Terraform configs (templates). It does not require a Cloud APIKey and assumes the users IAM credentials. It will take about 10 minutes to install and delete. 

All user configurable parameters are specified on the command line at Blueprint create time. 

Following resources are deployed:
- Resource Group
- COS instance and bucket

The user must have IAM access permisions to create resource groups and COS buckets. 


## Blueprint definition - basic-blueprint.yaml

The blueprint is comprised of two linked modules to deploy the resource group and COS resources. 
- basic-resource-group
- basic-cos-storage

Source Terraform configs are in repo https://github.ibm.com/steve-strutt/blueprint-examples-modules

```
Blueprint file: basic-blueprint.yaml
├── basic-resource-group
|    └── source: github.ibm.com/steve-strutt/blueprint-examples-modules/IBM-ResourceGroup
└── basic-cos-storage
     └── source: github.ibm.com/steve-strutt/blueprint-examples-modules/IBM-Storage
```

### Blueprint input definitions 
The blueprint accepts the following inputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null | Name for COS instance |


## Blueprint inputs
This example uses CLI inputs and an input file. 


### Config input file - basic-input-null.yaml
The input file must define the variable names for all the required Blueprint input. In this example these are all set to null to demonstrate the passing of user customised inputs via the CLI.   

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null  | Name for COS instance |

### CLI input values
Inputs to be specified on the CLI at Blueprint create. These values can be customised at create time. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | basic_RG | Resource group to create |
| provision_rg | string | true | Create new ResourceGrroup - true. Use existing an ResourceGroup - false |
| cos_instance_name | basic_COS | null  | Name for COS instance |




## Usage 
Input values on the create command can be customised to user needs and allow referencing of an existing ResourceGroup by specifying `provision_rg=false`. 

```

$ ibmcloud schematics blueprint create 
-name=Blueprint_Basic
-resource_group=Default
-bp_git_url https://github.ibm.com/steve-strutt/blueprint-example-modules/basic_blueprint.yaml
-input_git_url https://github.ibm.com/steve-strutt/blueprint-example-modules/basic_input_null.yaml
-value1
-value2


$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint jobs -id blueprint_id

$ ibmcloud schematics blueprint get -id blueprint_id

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```


