# Simple Blueprint and blueprint example

This is a simple example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to deploy a simple 2 module solution stack using linked Terraform configs (templates). It does not require a Cloud APIKey and assumes the users IAM credentials. It will take about 10 minutes to install and delete. 

Following resources are deployed:
- Resource Group
- COS instance and bucket

The user must have IAM access permisions to create resource groups and COS buckets. 


## Blueprint definition - basic-blueprint.yaml

The blueprint demonstrating linking two Terraform configs as Workspaces to deploy a resource group and COS resources. 
- basic-resource-group
- basic-cos-storage

TF configs are sourced from https://github.ibm.com/steve-strutt/blueprint-examples-modules/
```
Blueprint file: basic-blueprint.yaml
├── basic-resource-group
|    └── source: github.ibm.com/steve-strutt/blueprint-examples-modules/IBM-ResourceGroup
└── basic-cos-storage
     └── source: github.ibm.com/steve-strutt/blueprint-examples-modules/IBM-Storage
```

### Blueprint definition inputs 
The blueprint.yaml definition file accepts the following inputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null | Name for COS instance |

### Blueprint outputs
The blueprint.yaml definition creates the following outputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| cos_id | string |  | ResourceID/CRN of COS instance |


## Blueprint input file basic-input-null.yaml
This example uses both CLI inputs and an input file basic-input-null.yaml.


### Input file - basic-input-null.yaml
The input file defines the variable names for all the required Blueprint definition inputs. In this example no input values are set and must be supplied as dynamic inputs when the Blueprint is created. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null  | Name for COS instance |

### CLI input values
Inputs to be specified on the CLI at Blueprint create. These values can be customied at create time to use an existing ResourceGroup or change names to avoid creating a duplicate. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | basic_RG | Resource group to create |
| provision_rg | string | true | Create new ResourceGrroup - true. Use existing an ResourceGroup - false |
| cos_instance_name | basic_COS | null  | Name for COS instance |



## Prerequisites
1. Install the Schematics CLI plugin by follow the instructions in the [documentation](https://cloud.ibm.com/docs/schematics?topic=schematics-setup-cli)  
2. Configure [IAM access permissions](https://cloud.ibm.com/docs/schematics?topic=schematics-access) for the Schematics Blueprints service. 
3. Set Schematics Target Region
The target (manage from) Schematics region for the Blueprint instance is determined by the IBM Cloud CLI target region. The region can be set with the `ibmcloud target` command.


## Usage 
Input values on the create command can be customised to user needs and allow referencing of an existing ResourceGroup by specifying `provision_rg=false`. 

<name>
<resourcegroup>

<provision_rg>
<resource_group_name>

```
$ ibmcloud target -r <region>

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


