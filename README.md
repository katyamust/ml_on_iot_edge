# Get Onboard with IoT Edge

## Software Prerequisites

- [iotedgectl](https://pypi.org/project/azure-iot-edge-runtime-ctl/)
- [Docker](https://docker.com)
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## Azure ML configuration

### Deploy Azure Machine Learning as an IoT Edge module

#### Create the Azure ML container
**** this par only workd with Azure ML Workbench ******

az login

az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService

az provider show -n Microsoft.MachineLearningCompute
az provider show -n Microsoft.MachineLearningCompute -o table

## Local deployment

### set up a local environment (creates ACR)
az ml env setup -l [Azure Region, e.g. eastus2] -n [your environment name] [-g [existing resource group]]
az ml env setup -l westEurope -n mlgetonboard -g GetOnboard-rg

### Create the Azure ML container and pushes it into Azure Containter Registry
az ml service create realtime --model-file model.pkl -f iot_score.py -n mlgob -r python

check : on portal: go to ACR, it says running

**** this par only works with Azure ML Workbench ******

# set up IoT Edge device
on portal: create iot edge service in iot hub
cmd: install iotedgectl
    iotedgectl setup --connection-string "lkajsdfklj" --nopass
    iotedgectl start

on portal : add iotedge device

### Add registry credentials to your Edge device
iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>


### Cleanup installation

```
iotedgectl stop
iotedgectl uninstall

docker system prune -a -f

iotedgectl setup --connection-string "lkajsdfklj" --nopass

iotedgectl start

docker logs -f edgeAgent


# Input Output Routing

Sample:
{
  "routes": {
    "sensorToMachineLearning": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/mlgob/inputs/amlInput\")",
    "machineLearningToIoTHub": "FROM /messages/modules/mlgob/outputs/amlOutput INTO $upstream"
  }
}
