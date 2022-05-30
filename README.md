# Simple Blueprint and blueprint example

This is a simple example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to deploy a simple 2 module solution stack. It does not require a Cloud APIKey and assumes the users IAM credentials. It will take about 10 minutes to install and delete. 

All user configurable parameters are specified on the command line at Blueprint create time. 

Following resources are deployed:
- Resource Group
- COS instance and bucket

The user must have IAM access permisions to create resource groups and COS buckets. 

The blueprint is comprised of two linked modules to deploy the resource group and COS resources. Source Terraform templates are in repo https://github.ibm.com/steve-strutt/bp-template-examples


```
Blueprint file: basic-blueprint.yaml
├── basic-resource-group
|    └── source: github.ibm.com/steve-strutt/bp-template-examples/IBM-ResourceGroup
└── basic-cos-storage
     └── source: github.ibm.com/steve-strutt/bp-template-example/IBM-Storage
```


## Blueprint input values

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null | Name for COS instance |


## Input file values
All inputs to this Blueprint example are defined on the CLI. Null values specified in the input file.  

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null  | Name for COS instance |

## CLI input values
| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null  | Name for COS instance |




## Usage 


```

$ ibmcloud schematics blueprint create ----ccccccccccc  c c  c c c c c c

-bp_giturl 
-config_giturl

$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```


