# Install apache on a Ubuntu Virtual Machine using Cloud-Init

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Falihhussain%2Fazure-cloud-init%2Fmaster%2Fapache%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>
<a href="http://armviz.io/#/?load=https%3A%2F%2Fraw.githubusercontent.com%2Falihhussain%2Fazure-cloud-init%2Fmaster%2Fapache%2Fazuredeploy.json" target="_blank">
    <img src="http://armviz.io/visualizebutton.png"/>
</a>

This template deploys a apache web server on a Ubuntu Virtual Machine. This template also deploys a Public IP address, Network Security Group, Virtual Network, Network Interface, and a Virtual Machine.

1. To deploy the template:
```bash
export rgName="apacheCloud" && \
export rgLocation="eastus" && \
az group create -l $rgLocation -n $rgName && \
az group deployment create --name MasterDeployment --resource-group $rgName --template-file ./azuredeploy.json

# Onces deployed run the following command to see the FQDN for the Web Page
az group deployment show -n MasterDeployment -g $rgName --query properties.outputs.http.value | awk -F '"' '{print $2}'
az group deployment show -n MasterDeployment -g $rgName --query properties.outputs.sshCommand.value | awk -F '"' '{print $2}'
az group deployment show -n MasterDeployment -g $rgName --query properties.parameters.userScript.value | awk -F '"' '{print $2}' | base64 --decode

# Once done delete via
az group delete -g $rgName -y --no-wait

```

To view the output:
```bash
az group deployment show -n MasterDeployment -g $rgName | jq .properties.outputs.http.value
```



```bash
cd /var/lib/cloud/instances
vim user-data.txt
cat /var/log/cloud-init.log | grep config-scripts-user
cat /var/log/boot.log

cat /var/www/html/index.html

# Apache2 config files
cd /etc/apache2
```