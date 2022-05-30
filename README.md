# Simple Blueprint and blueprint example

This is a simple all-in-one, blueprint, blueprint, config and TF files, example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to deploy a simple 2 module solution stack. It does not require a Cloud APIKey and assumes the users IAM credentials. It will take about 10 minutes to install and delete. 

Following types of resources are deployed:
- Resource Group
- COS instance and bucket


The blueprint is comprised of two linked modules to deploy the resource group and COS resources. These create the Schematics Workspaces
- basic-resource-group
- basic-cos-storage

The blueprint demonstrates passing of the resource_group_id variable from the basic-resource-group module to the basic-cos-storage module. 



## Blueprint input values

| Name | Type | Value | Description |
|------|------|------|----------------|
| region | string | us-east | Resource deployment region |
| resource_group_name | string | basic2comp-us | Resource group to create |
| provision_rg | string | true | Create RG - true. Use existing RG - false |
| cos_instance_name | string | basic2comp-cos | Name for COS instance |




## Usage 


```

$ ibmcloud schematics blueprint create ----ccccccccccc  c c  c c c c c c

$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```


