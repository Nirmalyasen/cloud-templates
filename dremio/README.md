
# Deploying Dremio to AWS, Azure
### Overview
This repository contains [a sample Cloudformation](aws) template that can be used to deploy Dremio to AWS and a [sample ARM template](azure) that can be used to deploy Dremio to Azure. The deployment architecture for these templates includes:

 - 1 master coordinator that also serves as a zookeeper for the cluster
 - 0 or more slave coordinators
 - 3 or more executors

The templates creates their own virtual network, subnets and security groups and deploys in it.

### Scaling the cluster
The coordinators and executors are setup are scale sets (auto scale group in AWS and vm scale set in Azure). You can use the AWS and Azure UI to scale up or down the number of executors and coordinators.

You can shut down the VMs to shut down your cluster. And bring them up to restart your cluster. However, note that shutting down an executor would lead to loss of any reflection, uploaded files stored on them.

## Using the templates

Clone the git repo. Customize or change, if you need to. Then you can follow the instructions below. These are UI based instructions - you can use the command line to deploy too.

### AWS
- Go to AWS Console -> Cloudformation -> Create Stack
- Select "Upload a template file" and choose aws/dremio_cf.yaml
- Provide the required inputs on the next page
- Review and create your stack
- Once the stack is ready, you will find the URL to Dremio UI in output section of the stack

### Azure
 - Go to Azure Portal and search for "Template deployment"
 - Choose "Build your own template in the editor"
 - Choose "Load file" and load azure/mainTemplate.json. Save.
    - mainTemplate.json  refers to dremioState.json and
      dremioCluster.json directly in github. So, if you customize them,
      you need to fork the github repo and upload your
      changes there. And update the reference in mainTemplate.json.
 - Provide the required inputs on the page - deploy to a new resource group, agree to the terms and conditions and purchase
 - Once the deployment is successful, you will find the URL to Dremio UI in the output section
