<center><div style="width: 75%; height: auto;"><img src="img/xpirit.png"/></div></center>
<br />
<center>
<table>
  <tr>
    <td>**Slides**</td><td>[http://nauts.io/workshop-azure-container-services](http://nauts.io/workshop-azure-container-services)</td>
  </tr>
</table>
</center>

!SLIDE
# Azure Container Service
<center>![ACS](img/acs.png)</center>

!SLIDE
<center>![ACS](img/docker.png)</center>

!SUB
# Introduction Docker

!SLIDE
# Azure Container Service DC/OS
-

!SUB
# Mesos
<center>![Mesos](img/mesos.png)</center>

!SUB
# Marathon
<center>![Mesos](img/marathon.png)</center>

!SUB
# Chronos
<center>![Mesos](img/chronos.png)</center>

!SUB
# marathon-dns
<center>![Mesos](img/marathon-dns.png)</center>

!SUB
# marathon-lb
<center>![Mesos](img/marathon-lb.png)</center>

!SLIDE
# Prerequisites
- Microsoft Azure trial account
- Docker Machine installed
- putty / ssh-agent installed

!SLIDE
# Hands-on style
- generic instruction, try to solve it yourself
- are you stuck? press 's' and check the presenter notes for typing instructions

!SLIDE
# Hands-on
- local paas-monitor application
- Setup a Azure Container Service 
- Install marathon-lb package
- Deploy paas-monitor application
- Deploy ASP.net Core application
- Scaling

!SUB
# Local paas-monitor application

paas-monitor is a small docker application that allows you to see
the effect of rolling upgrades, scaling, failures etc.

- start a local docker-machine
- run the docker image mvanholsteijn/paas-monitor:latest 
- point your browser at the application
- what do you see? what happens if you stop the paas-monitor?

<center>![DC/OS on ACS](img/paas-monitor.png)</center>


!NOTE
```
docker-machine create -d virtualbox  dev
eval $(docker-machine env dev)
docker run -d -p :1337:1337 mvanholsteijn/paas-monitor:latest
open http:$(echo $DOCKER_HOST |cut -d: -f2):1337
docker stop $(docker ps -ql)
```

!SUB  
# Deploy ACS - DC/OS
- goto the Azure portal and create a DC/OS test cluster
<center>![DC/OS on ACS](img/azure-create.png)</center>


!NOTE
- goto [Azure Portal](https://portal.azure.com/), 
- Click New
- Search 'Azure Container Service (test cluster with DC/OS)'
- Click Create 
- Do a basic configuration in West Europe
See [Deploy an ACS Cluster](https://azure.microsoft.com/en-us/documentation/articles/container-service-deployment/) for full details.


!SUB
# Connect to ACS - DC/OS
Point your browser to the DC/OS console via an SSH tunnel to your mesos master machine at &lt;username>@&lt;dns-prefix>mgmt.westeurope.cloudapp.azure.com. The master listens on port 2200.
<center>![DC/OS on ACS](img/dcos-test-deployment.png)</center>

!NOTE
```
sudo ssh -i $HOME/.ssh/id_rsa -N -L 80:localhost:80 &lt;user>@&lt;dns-prefix>mgmt.westeurope.cloudapp.azure.com -p 2200
open http://localhost
```

See [Connect to ACS cluster](https://azure.microsoft.com/en-us/documentation/articles/container-service-connect/) for full details.


!SUB
# Explore the Console
Explore the DC/OS console a bit.
<center>![DC/OS Console](img/dcos-console.png)</center>

!SUB
# Install the marathon loadbalancer
On the console, goto the DC/OS Universe and install the marathon-lb with 1 cpu and 256Mb memory.
<center>![DC/OS Console](img/marathon-lb-install.png)</center>


!NOTE
- Click on Universe
- Search marathon-lb
- Click 'Advanced Installation'
- Set cpus to 1 
- Set memory to 256
- Click review and install.

!SUB
# Scaling VM Scale Sets
- Increase the public VMSS to 2
- Increase the private VMSS to 3

See
[https://github.com/.../201-vmss-scale-existing](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)
