# terraform-workspace-cleanup


## Use Case

Sometimes when building products, we want environments for developers to be built on-demand or on the opening of pull requests. However, 
we need an automated way to destroy these environments, so they don't sit around costing us $$$$

So what is the purpose of this? Well, when you create a new environment using a terraform workspace using this you can add the workspace you have 
created in your automated pipeline into a DynamoDB table with a TTL value. You can then set up an automated job to also use this tool to 
pull expired workspaces from DynamoDB

## Creating the DynamoDB table

We have provided a Terraform Module that will create a DynamoDB table in the configuration required by our tool. 
To use our module you can use the following snippet:

```
module "workspace-cleanup" {
  source  = "git@github.com:TomTucka/terraform-workspace-cleanup/terraform/workspace_cleanup"
  enabled = true
}
```

## Using this tool

```
Usage: tf-workspace-cleanup -register-workspace=<workspace> -aws-account-id=12345678 -aws-iam-role=operator
Usage: tf-workspace-cleanup -expired-workspaces=true -aws-account-id=12345678 -aws-iam-role=operator
  -aws-account-id string
    	Account ID for IAM Role
  -aws-iam-role string
    	AWS IAM Role Name
  -expired-workspaces
    	get list of expired workspaces for deletion
  -register-workspace string
    	Register a workspace to be deleted at a later point
```

### Puting a workspace into DynamoDB

Putting something into DynamoDB is really simple

`tf-workspace-cleanup -put=<workspace> -aws-account-id=<account_id> -aws-iam-role=<iam_role_name>`
