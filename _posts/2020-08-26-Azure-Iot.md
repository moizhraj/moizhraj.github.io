# **Azure IoT Offerings**
> Documentation is from Microsoft Azure docs

Azure provides many IoT services to manage your devices which includes IoT Central, IoT Hub to manage IoT devices and Edge devices and Edge gateways. Below sections will give you a quick start guide on each of these services.

**Pre requisites**
* Basic understanding of Azure IoT offerings i.e. [IoT Central](https://docs.microsoft.com/en-us/azure/iot-central/core/), [IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/about-iot-hub), IoT Devices, [IoT Edge devices](https://docs.microsoft.com/en-us/azure/iot-edge/), [IoT Edge gateways](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-create-transparent-gateway).
* Basic understanding of Docker containers


## **Azure IoT Central**
No code solution. Provides a portal to 

* Configure IoT Hub, Iot devices, edge devices, edge gateways and modules.
* Configure rule on the data streamed from these devices
* Display analytics (twins, stream analytics, time series insights) in form of charts and dashboard.
* Trigger emails, azure functions, connect to Dynamics 365 or call a webhook on a rule match

## **Azure IoT Hub**
Allows you to manage IoT devices, edge devices, edge gateways and modules.

Getting started with IoT Hub:
* Create an IoT hub in Azure Portal
* Register IoT device identity to your IoT hub
  * Open the registered device identity, go to properties and copy the connection string over to your physical device. 
  * On physical devices, the connection string can be copied inside your code that uses Azure IoT SDK. SDK is availble in different langauges i.e. C/C++, Node, Python, Java, .Net.
* (Optional) Register IoT edge device identity to your IoT hub
  * Same step applies for getting and using the connection string for edge device. Downstream device will have an extra property in their connection string that points to the Gateway device.

## **IoT Edge device**
**Use cases**
  * Analytics at the edge: Use AI services locally to process data coming from downstream devices without sending full-fidelity telemetry to the cloud. Find and react to insights locally and only send a subset of data to IoT Hub.
  * Downstream device isolation: Shield downstream devices from exposure to internet. 
  * Connection multiplexing: All devices connecting to IoT Hub through an IoT Edge gateway use the same underlying connection.
  * Traffic smoothing:  The IoT Edge device will automatically implement exponential backoff if IoT Hub throttles traffic, while persisting the messages locally. This benefit makes your solution resilient to spikes in traffic.
  * Offline support: The gateway device stores messages and twin updates that cannot be delivered to IoT Hub.

**[Setup linux as edge device](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux)**
* Create an IoT Hub
* Register an IoT edge device to your IoT hub
* [Install the Azure IoT edge runtime on your device](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux)
* Check on the status of your IoT edge service
  ```
  sudo systemctl status iotedge
  ```
  To troubleshoot any issues for iotedge service run
  ```
  journalctl -u iotedge
  ```
  List all the iotedge modules running
  ```
  sudo iotedge list
  ```

**[Setup modules on edge device in azure portal](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#deploy-a-module)**

#### **Develop modules for edge**
* [Develop modules for Linux devices](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-develop-for-linux)
* [Develop modules for Windows devices](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-develop-for-windows)

## **IoT Edge as a gateway device**
#### **Gateways are of 3 types. Transparent, Protocol translation, Identity translation.**
  ![Gateway Types](https://docs.microsoft.com/en-us/azure/iot-edge/media/iot-edge-as-gateway/edge-as-gateway.png)

#### **[Configure as a transparent gateway](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-create-transparent-gateway)**
* This requires multiple certs generation at Edge and downstream devices. Root CA cert, Device CA cert, Device CA private key.
  [CA Certs](https://docs.microsoft.com/en-us/azure/iot-edge/media/how-to-create-transparent-gateway/gateway-setup.png)
  * [Create root CA cert](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-create-test-certificates#create-root-ca-certificate). At the end of these instructions, you'll have a root CA certificate file:  
  `<path>/certs/azure-iot-test-only.root.ca.cert.pem`
  * [Create IoT edge device CA cert](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-create-test-certificates#create-iot-edge-device-ca-certificates). At the end of these instructions, you'll have two files, a device CA certificate and its private key:  
  `<path>/certs/iot-edge-device-<cert name>-full-chain.cert.pem` and  
  `<path>/private/iot-edge-device-<cert name>.key.pem`  
* Configure these certs in iot edge's config.yaml file
  * Windows: `C:\ProgramData\iotedge\config.yaml`
  * Linux: `/etc/iotedgeconfig.yaml`
* Restart iotedge service
* Make sure you open ports on your gateway device
    | Port | Protocol |
    | ---- | -------- |
    | 8883 | MQTT |
    | 5671 | AMQP |
    | 443 | HTTPS <br> MQTT+WS <br> AMQP+WS |
* [Authenticate downstream device](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-authenticate-downstream-device)
    Make sure you modify your hosts file in the downstream device to recognize the Gateway hostname.
	Sample Connection string for a downstream device
	```
	HostName=myiothub.azure-devices.net;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz;GatewayHostName=myGatewayDevice
	```
	Simplified connection string for a downstram device. If you established a parent/child relationship for the downstream device, then you can simplify the connection string by calling the gateway directly as the connection host. Parent/child relationships are required for X.509 authentication but optional for symmetric key authentication:
	```
	HostName=myGatewayDevice;DeviceId=myDownstreamDevice;SharedAccessKey=xxxyyyzzz
	```
* [Connect a downstream device](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-connect-downstream-device)

#### **iotedge cheatsheet**
* check cli version: `iotedge version`
* check service status: `sudo systemctl status iotedge`
* troublesheet any issues for iotedge service: `journalctl -u iotedge`
* list all the iotedge modules: `sudo iotedge list`
* check for common config and deployment issues: `sudo iotedge check`
* check for logs for a module: `sudo iotedge logs <module-name>`
* restart a module: `sudo iotedge restart <module-name>`

###### Note: Wherever the links are provided, the documentation wants you to follow that and once that step is completed you can jump back to the next step.

***

#### Further reading
* [Getting started with Docker](https://docs.docker.com/get-started/)
* [Stream Analytics](https://docs.microsoft.com/en-us/azure/stream-analytics/)
