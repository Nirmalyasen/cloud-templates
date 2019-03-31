
# Deploying Dremio to Azure

Try it out [![Azure ARM Template](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/microsoft.template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FNirmalyasen%2Fcloud-templates%2Fmaster%2Fdremio%2Fazure%2FmainTemplate.json)

You can clone the repo and try it out on Azure:
 - Go to Azure Portal and search for "Template deployment"
 - Choose "Build your own template in the editor"
 - Choose "Load file" and load azure/mainTemplate.json. Save.
    - mainTemplate.json  refers to dremioState.json and
      dremioCluster.json directly in github. So, if you customize them,
      you need to fork the github repo and upload your
      changes there. And update the reference in mainTemplate.json.
 - Provide the required inputs on the page
    - Deploy to a new resource group. It helps in deleting the cluster - you can delete the resource group.
    - Agree to the terms and conditions and purchase.
 - Once the deployment is successful, you will find the URL to Dremio UI in the output section
