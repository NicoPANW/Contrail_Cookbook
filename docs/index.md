



# Virtual Networks

On below screenhoot, we can notice
- Red VN having 3 VMs attached: vSRX_3, vSRX_4 and vSRX_5
- We can understand on which compute the VM runs. Besides, they all run on different computes. 

![Screenshot](img/virtual_networks/VN-general.png)

The following screenshoots show for each VM the associated IP@ and Mac@
- vSRX_3: 10.0.0.3 / 02:42:d3:0f:50:35
- vSRX_4: 10.0.0.4 / 02:35:62:da:f5:b0
- vSRX_4: 10.0.0.5 / 02:03:04:6d:19:c1
![Screenshot](img/virtual_networks/vSRX_3-interface.png)

![Screenshot](img/virtual_networks/vSRX_4-interface.png)

![Screenshot](img/virtual_networks/vSRX_5-interface.png)


## Advanced Options

### Forwarding Mode
Contrail supports 3 forwarding modes: L2 and L3, L2 only and L3 only.

In next sections, we will focus:
- only on "Red" VN and from compute viewpoint.
- from compute-4v-7.sdn.lab that is hosting vSRX-3

#### L2 and L3

![Screenshot](img/virtual_networks/VR-L2L3-L3view.png)

![Screenshot](img/virtual_networks/VR-L2L3-L2view.png)


#### L2 only

![Screenshot](img/virtual_networks/VR-L2-L3view.png)

![Screenshot](img/virtual_networks/VR-L2-L2view.png)

#### L3 only

![Screenshot](img/virtual_networks/VR-L3-L3view.png)

![Screenshot](img/virtual_networks/VR-L3-L2view.png)




