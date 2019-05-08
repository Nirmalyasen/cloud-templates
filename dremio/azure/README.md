
# Deploying Dremio to Azure

This deploys a Dremio cluster on Azure VMs. The deployment creates a master coordinator node and number of executor nodes depending on the size of the cluster chosen. The table below provides the machine type and number of executor nodes for the different sizes of Dremio clusters. 

| Cluster size | Coordinator VM Type | Executor VM Type | No. of Executors |
|--------------|---------------------|------------------|------------------|
| X-Small      | Standard_D4_v3      | Standard_E16s_v3 |        1         |
| Small        | Standard_D4_v3      | Standard_E16s_v3 |        5         |
| Medium       | Standard_D8_v3      | Standard_E16s_v3 |        10        |
| Large        | Standard_D8_v3      | Standard_E16s_v3 |        25        |
| X-Large      | Standard_D8_v3      | Standard_E16s_v3 |        50        |

The deployment resources are:
```
                     ┌───────────────────────────┐
                     │       WebUI on 9047       │
                     │ JDBC/ODBC client on 31010 │
                     └──────────────┬────────────┘
                                    │
┌───────────────────────────────────┼───────────────────────────────────┐
│ VirtualNetwork                    │                                   │
│ ┌─────────────────────────────────▼─────────────────────────────────┐ │
│ │ Subnet            ┌──────────────────────────┐ ┌────────────────┐ │ │
│ │                   │       LoadBalancer       │ │ Security Group │ │ │
│ │                   └─────────────┬────────────┘ │Allow access to │ │ │
│ │                                 │              │22, 9047, 31010 │ │ │
│ │           ┌─────────────────────┤              └────────────────┘ │ │
│ │           │                     │                                 │ │
│ │           │                     │                                 │ │
│ │           ▼                     ▼                                 │ │
│ │ ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐ │ │
│ │ │Master Coordinator │ │ Slave Coordinator │ │     Executor      │ │ │
│ │ │    (Azure VM)     │ │(Azure VM Scaleset)│ │(Azure VM Scaleset)│ │ │
│ │ └───────────────────┘ └───────────────────┘ └───────────────────┘ │ │
│ │                                                                   │ │
│ │ ┌───────────────────┐                                             │ │
│ │ │  Dremio Metadata  │                                             │ │
│ │ │   (Azure Disk)    │                                             │ │
│ │ └───────────────────┘                                             │ │
│ └───────────────────────────────────────────────────────────────────┘ │
│                                                                       │
│                                                                       │
└───────────────────────────────────────────────────────────────────────┘
```
You can try it out: [![Azure ARM Template](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/microsoft.template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FNirmalyasen%2Fcloud-templates%2Fmaster%2Fdremio%2Fazure%2FmainTemplate.json)

The inputs required during deployment are:

|Input Parameter|Description |
|---|---|
|Subscription |Azure subscription where the cluster should be deployed. |
|Resource Group |The Azure Resource group where the cluster should be deployed. You can create a new one too.|
|Location |The Azure location where the cluster resources will be deployed. |
| Cluster Name |A name for your cluster.|
| Cluster Size |Pick a size based on your needs.|
| SSH Username |The username that can be used to login to your nodes.|
| Authentication Type |Password or Key based authentication for ssh.|
| Password or Key |The password or ssh public key |
| Use Existing Subnet | (Optional) id of an existing subnet. The subnet must be in the same region as the Dremio cluster resource group. It is of the form /subscriptions/xxxx/resourceGroups/xxxx/providers/Microsoft.Network/virtualNetworks/xxxx/subnets/xxxx|
| External Load Balancer | If you are using an existing subnet and want to use an internal ip address from the subnet for load-balancing, set to false. | 
| Dremio Download URL | (Optional) URL of a Dremio rpm. If empty, it will deploy the latest published release. |


Once the deployment is successful, you will find the URL to Dremio UI in the output section of the deployment. 
