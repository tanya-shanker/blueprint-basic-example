# Simple Blueprint example

This is a simple example to demonstrate the capabilites of IBM Cloud Schematics Blueprints to deploy solutions using linked Workspaces and Terraform configs. A Pay-go or subscription account, either as the account owner or user is required to exercise this example as cloud resources are created and a cost is incurred. 

As an alternative, the example [blueprint-complex-inputs](https://github.com/Cloud-Schematics/blueprint-complex-inputs) can be deployed at no cost.  

Following cloud resources are deployed or referenced:
- Resource Group
- COS instance and bucket

The user must have IAM access permisions to create resources in the Default resource group and [Cloud Object Storage](https://test.cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Also permissions to create Schematics [Workspaces and Blueprints](https://test.cloud.ibm.com/docs/schematics?topic=schematics-access). 


## Blueprint definition - basic-blueprint.yaml

This blueprint demonstrates linking two Terraform configs as Workspaces to reference a resource group and create Cloud Object Storage (COS) resources. 

The Blueprint consists of two Modules:
- basic-resource-group
- basic-cos-storage

The TF configs used in this Blueprint are sourced from the repo https://github.com/Cloud-Schematics/blueprint-example-modules/
```
Blueprint file: basic-blueprint.yaml
├── basic-resource-group
|    └── source: github.com/Cloud-Schematics/blueprint-example-modules/IBM-ResourceGroup
└── basic-cos-storage
     └── source: github.com/Cloud-Schematics/blueprint-example-modules/IBM-Storage
```

### Blueprint definition inputs 
The basic-blueprint.yaml definition file accepts the following inputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | null | Name for COS instance |
| cos_storage_plan | string | null | Service plan type for COS instance |

### Blueprint outputs
The basic-blueprint.yaml definition creates the following outputs:

| Name | Type | Value | Description |
|------|------|------|----------------|
| cos_id | string |  | ResourceID/CRN of COS instance |


## Blueprint input file basic-input.yaml
This example uses both CLI inputs and an input file basic-input.yaml.


### Input file - basic-input.yaml
The input file defines the variable names and values for the required Blueprint definition inputs. In this example the  input values related to the target resource group must are supplied as dynamic inputs when the Blueprint is created. This allows for user customisation of the Blueprint at deploy time. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | null | Resource group to create |
| provision_rg | string | null | Create RG - true. Use existing RG - false |
| cos_instance_name | string | Blueprint-basic  | Name for COS instance |
| cos_storage_plan | string | standard | Service plan type for COS instance |

### CLI input values
Inputs to be specified on the CLI at Blueprint create time. These values can be customied at Blueprint create time to use an existing ResourceGroup or change group names to avoid creating a duplicate. 

| Name | Type | Value | Description |
|------|------|------|----------------|
| resource_group_name | string | Default | Resource group to reference or create |
| provision_rg | string | false | Create new ResourceGroup - true. Use existing an ResourceGroup - false |



## Prerequisites
1. Install the Schematics CLI plugin by follow the instructions in the [documentation](https://cloud.ibm.com/docs/schematics?topic=schematics-setup-cli).
2. Configure [IAM access permissions](https://cloud.ibm.com/docs/schematics?topic=schematics-access) for the Schematics Blueprints service. 
3. Configure [Cloud Object Storage IAM permissions](https://test.cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-iam) to create COS instances.
4. Set Schematics Target Region. The region can be set with the `ibmcloud target -r` command. The Schematics Blueprint instance is determined by the IBM Cloud CLI target region.


## Usage 
This example can be customised to work with different types of IBM Cloud account and the users level of IAM access permissions to resources and resource groups. To work with the broadest range of accounts and scenarios, the example commnd input here requires the minimum rights to create new resources. It assumes that the resource group `Default` exists and the user has been granted Schematics access permissions and COS access permissions to this group. The input parameters configure the Blueprint to retrieve the ID of the existing `Default` resource group and the COS instance is created in this group. 


The following parameters are used for the `blueprint create` configuration. 
- Name of the blueprint: `Blueprint_basic`
- Schematics management resource group: `Default`
- Blueprint URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Blueprint file: `basic-blueprint.yaml`
- Blueprint Git branch `main`
- Input file URL: `https://github.com/Cloud-Schematics/blueprint-basic-example`
- Input file: `basic-input.yaml` 
- Input file Git branch `main`
- Inputs: 
    - `provisiong_rg`=**false**
    - `resource_group_name`=**Default**

```
$ ibmcloud target -r <region>

$ ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=false,resource_group_name=Default

$ ibmcloud schematics blueprint install -id blueprint_id

$ ibmcloud schematics blueprint job list -id blueprint_id

$ ibmcloud schematics blueprint get -id blueprint_id -profile outputs

$ ibmcloud schematics blueprint destroy -id blueprint_id

$ ibmcloud schematics blueprint delete -id blueprint_id
```

## Customise blueprint-basic-example to create a resource group
In Pay-Go or Subscription accounts, or where the user has administrative permissions to create resource groups, the Blueprint can be customised to create a user specified resource group. The user must be an account owner or have been granted  [Account Management, editor or administrator permissions](https://cloud.ibm.com/docs/account?topic=account-account-services&interface=ui#account-management-actions-roles) to create resource groups. 

With the required IAM permissions, a user specified resource group can be created by changing the folllowing input parameters on the create command:
- `provision_rg=true` 
- `resource_group_name=test_group`

```
$ ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default -bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-branch main -bp-git-file basic-blueprint.yaml -input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-branch main -input-git-file basic-input.yaml -inputs provision_rg=true,resource_group_name=test_group
```

This will create the resource group `test_group` and the COS instance will be created in this group. 

Refer to the [Schematics FAQ](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprints-faq&interface=ui#faqs-bp-basic-example) documentation for diagnosing and resolving the typical configuration errors with this example and their resolution.

## Next Steps

Looking for more samples? Check out the [IBM Cloud Schematics GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). 

Check the example [Readme](https://github.com/Cloud-Schematics/blueprint-basic-example/edit/main/README.md) files for further Blueprint customisation and the scenario usage.

