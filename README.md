# Deploy Network Slices

## 1. Introduction

### End-to-End demo 5G Slice

### End-to-End demo 5G Slice and Firewall


## 2. Prepare for the lab

> The credentials for the group X are: **Username: groupX** - **Password: groupX**

### Download the lab's Github repository

Download the lab's repository and cd into the respective folder for your group:

```bash
git clone https://github.com/medianetlab/OuluLabs.git
cd OuluLabs/groupX
```

### Connect to the NCSRD premises 


### Login to your group's tools

#### OpenStack

OpenStack is the Virtual Infrastructure Manager that you will use for deploying Network Slices. Go the OpenStack home page `http://10.161.0.11/horizon` and log into your account using your group's credential. Navigate around the various windows that present different OpenStack resource objects, such as virtual servers, virtual networks, images, etc.

![OpenStack Home Page](images/openstack0.PNG)

#### Open Source MANO (OSM)

OSM is an ETSI-hosted project to develop an Open Source NFV Management and Orchestration (MANO) software stack aligned with ETSI NFV. It acts as an NFV Orchestrator (NFVO) in our testbed. Go to the OSM home page `http://10.161.5.155/projects/` and log in using your group's credential. Navigate around the various OSM windows to see the various stored VNFD and NSD packages.

![OSM Home Page](images/osm0.PNG)

#### Katana Slice Manager

You will find the Katana Slice Manager server that you will be using instantiated inside your group's instances on OpenStack. Copy the IP address of the VM hosting Katana and use it to open the Katana swagger page at `http://KATANA_IP:8001`. Check the various endpoints and resources supported by Katana.

![Katana Home Page](images/katana0.png)

### Create the client VM

Create the VM that will be used as a client node for the demo 5G slices. On the OpenStack dashboard, go to the __Project/Instances__ window and click the __Launch Instance__ button located on the top-right corner. 

On the pop-up window, select the following properties for the new VM:

* Details/Instance Name: 
  * groupX_client
* Source:
  * ClientImage
* Flavor
  * m1.tiny
* Networks
  * groupX_client_net

Open the VM console and log in using the credentials: __Username: cirros__ - __Password: gocubsgo__. Check the IP address and the defined routes of the VM.


## 3. Create a single Network Service (NS) via the OSM

OSM allows us to create a virtual NS om a specific domain of the testbed (e.g. Core, edge, etc.). In this example, we will create a demo gNodeB network service. Your client VM will be able to communicate with the new gNodeB, but it won't be able to reach any other network on the Internet, since we will be missing the 5G Core part of the 5G service. The first step is to register the VIM that OSM will use for hosting the NS. From the OSM dashboard go to the __VIM Accounts__ window and click the __New VIM__ button located on the top-right corner.

On the pop-up window, select the following properties for the new VIM:

* Name: DemoVIM
* Type: Openstack
* URL: http://10.161.0.11:5000/v3
* Tenant name: groupX
* Username: groupX
* Password: groupX

Next, go open the __Instances/NS Instances__ window, and click the __New NS__ button button located on the top-right corner to launch a new NS. You will use the demo gNodeB NSD to create a gNodeB instances.

On the pop-up window, select the following properties for the new NS:

* Name: demoNS
* Description: A new demo NS
* Nsdd Id: groupX_demoGnbnsd
* Vim Account Id: DemoVIM

You will see a new NS and a new VNF instantiating on the respective tabs. Go to the OpenStack dashboard and check the new VM starting as part of the gNodeB VNF. From your client VM console you should be able to ping the default GW, but nothing else on the Internet. You may now delete the new NS using the OSM Dashboard.

