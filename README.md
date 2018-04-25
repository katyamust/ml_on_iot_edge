# Get Onboard with IoT Edge

## Software Prerequisites

- [iotedgectl](https://pypi.org/project/azure-iot-edge-runtime-ctl/)
- [Docker](https://docker.com)
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

## Azure ML configuration

### Cleanup installation

```
iotedgectl stop
iotedgectl uninstall

docker system prune -a -f

iotedgectl setup --connection-string "lkajsdfklj" --nopass

iotedgectl start

docker logs -f edgeAgent
```

### AML Container creation

Prerequisite 
- a working scoring model
- a pkl file


```
az login





```