



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

This can be set on a per VN basis, see below.

![Screenshot](img/virtual_networks/VR-L2L3-settings.png)

In next sections, we will focus:
- only on "Red" VN
- from compute-4v-7.sdn.lab that is hosting vSRX-3

ARP algo is as follow (from VM traffic to vRouter):

    Do Inet route lookup for the IP Destination
    If Route found for the IP Destination
        If Proxy Flag set in the route
            If MAC Stitching information present in the route
                Send ARP Respone with the MAC present in stitching information
            Else // MAC Stitching not found
                If Flood Flag set in the route
                    Flood the ARP packet
                Else // PROXY set, No-Flood, No-MAC Stitching
                    Send ARP response with VRouter MAC address
        Else // Proxy Flag not set
            If Flood flag set in the Route
                Flood the ARP packet
            Else // Proxy Not set, Flood not set
                Drop the packet
    Else // Route not found
        Flood the ARP

Note: ARP processing is greatly further detailed here: https://github.com/Juniper/contrail-controller/wiki/Contrail-VRouter-ARP-Processing 

#### L2 and L3

In this mode IPv4 traffic lookup is done via IP FIB and all non IPv4 traffic is directed to MAC FIB.

This is the default option and should be used unless having a specific requirement.

It is worth mentioning that since we are on compute-4v-7.sdn.lab that is hosting vSRX-3, 10.0.0.3/32 has local tap-interface, while for instance 10.0.0.4/32 has tunneling via MPLSoUDP since being on another compute. Simiraly, 02:42:d3:0f:50:35 has local tap-interface while for instance 02:37:f4:6f:5f:cb has tunneling via MPLSoUDP since being on another compute

![Screenshot](img/virtual_networks/VR-L2L3-L3view.png)

![Screenshot](img/virtual_networks/VR-L2L3-L2view.png)

In this mode, vRouter will act as proxy-ARP. As explained earlier on the ARP Algo, there are two subcases:
- a) Neither vSRX_3 and vRouter know the MAC@ of vSRX4
- b) vSRX_3 does not know the MAC@ of vSRX4, but vRouter knows it.

Below vSRX_3 is pinging vSRX_4. ARP table on vSRX_3 was cleared. We notice that vSRX_3 sends an ARP request for 10.0.0.4. Besides, on vif interface we can see the flag "L3L2" and "P" for proxy.

![Screenshot](img/virtual_networks/VR-L2L3-ping.png)

#### L2 only

All traffic goes via MAC FIB lookup only.

This option is useful once we have non-IP traffic like legacy protocols in VNF. 

Below we can notice that the IP FIB is empty while the MAC FIB is populated.


![Screenshot](img/virtual_networks/VR-L2-L3view.png)

![Screenshot](img/virtual_networks/VR-L2-L2view.png)

In this mode, vRouter will never answer ARP, it will flood ARP. 

Below vSRX_3 is pinging vSRX_4. ARP table on vSRX_3 was cleared. We notice that vSRX_3 sends an ARP request for 10.0.0.4. vSRX_4 is answering back accordingly. Besides, on vif interface we can see the flag "L2" and "F" to flood.

![Screenshot](img/virtual_networks/VR-L2-ping.png)

#### L3 only

All traffic goes via IP FIB lookup only.

Below we can notice that the IP FIB is populated while the MAC FIB is only having the local MAC@. 

![Screenshot](img/virtual_networks/VR-L3-L3view.png)

![Screenshot](img/virtual_networks/VR-L3-L2view.png)




