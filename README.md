{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "virtualNetworkName": {
      "type": "string",
      "defaultValue": "allurisamplenic",
      "metadata": {
        "description": "This is the name of the Virtual Network"
      }
    },
    "networkInterfaceName": {
      "type": "string",
      "defaultValue": "aaasamplenin",
      "metadata": {
        "description": "This is the prefix name of the Network interfaces"
      }
    }
  },
   "variables": {
    "VitualNetworkName": "[concat(parameters('virtualNetworkName'),'vnet')]",
    "NetworkInterfaceName": "[concat(parameters('networkInterfaceName'),'nic')]",
    "SubnetName": "akhilaaanet",
    "VnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
    "SubnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "NumberOfInstances": 1
  },
  "resources": [
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[parameters('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "10.0.0.0/16"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "10.0.2.0/24"
            }
          }
        ]
      }
    },
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(parameters('networkInterfaceName'), copyindex())]",
      "location": "[resourceGroup().location]",
      "copy": {
        "name": "nicLoop",
        "count": "[variables('numberOfInstances')]"
      },
      "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    }
  ]
}
