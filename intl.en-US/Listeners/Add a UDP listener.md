# Add a UDP listener {#task_1563673 .task}

This topic describes how to add a UDP listener to a Server Load Balancer \(SLB\) instance. You can add a UDP listener to forward requests from the UDP protocol.

An SLB instance is created. For more information, see [../DNSLB19102239/EN-US\_TP\_15644.md\#](../reseller.en-US/Instance/Create an SLB instance.md#).

Note the following before you add a UDP listener:

-   Currently, ports 250, 4789, and 4790 are reserved.
-   Currently, fragmented packets are not supported.
-   UDP listeners of an SLB instance of the classic network do not support viewing source IP addresses.
-   The following operations require five minutes to take effect if they are performed in a UDP listener:
    -   Remove a backend ECS instance.
    -   Set the weight of a backend server to 0 after the backend server is declared as unhealthy.
-   Because IPv6 has a longer IP header than IPv4, when you use a UDP listener on an IPv6 SLB instance, you must ensure that the MTU of the NIC on the backend server \(ECS instance\) communicating with the SLB instance is not greater than 1480 \(some applications need to synchronize its configuration files based on this MTU value\). Otherwise, packets may be discarded because they are too large.

    If you use a TCP, HTTP, or HTTPS listener, no additional configurations are required because the TCP protocol supports MSS auto-negotiation.


## Step 1 Open the listener configuration wizard {#section_zd7_xi8_xj0 .section}

To open the listener configuration wizard, follow these steps:

1.  Log on to the [Server Load Balancer console](https://partners-intl.console.aliyun.com/#/slb). 
2.  In the left-side navigation pane, choose **Instances** \> **Server Load Balancer**.
3.  Select the region of the target SLB instance.
4.  Select one of the following methods to open the listener configuration wizard: 
    -   On the Server Load Balancer page, find the target SLB instance and then click **Configure Listener** in the Actions column.

        ![Listener configuration wizard](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16139/156579770710004_en-US.png)

    -   On the Server Load Balancer page, click the ID of the target SLB instance. On the Listeners tab, click **Add Listener**.

        ![Add listener](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16161/15657977077399_en-US.png)


## Step 2 Configure the UDP listener {#section_k58_mb2_whz .section}

To configure the UDP listener, follow these steps:

1.  On the Protocol and Listener page, configure the UDP listener according to the following information. 

    |Configuration|Description|
    |:------------|:----------|
    |**Select Listener Protocol**|Select the protocol type of the listener. In this topic, select **UDP**.

 |
    |**Listening Port**|The listening port used to receive requests and forward the requests to backend servers. The port number is in the range of 1 to 65535.

 **Note:** In the same SLB instance, the UDP or TCP listener port numbers can be the same in the following regions. However, you must first apply for the **privilege to use the beta function of configuring the same ports in TCP/UDP listeners** on the [Quota Management](https://slb.console.aliyun.com/slb/quota) page of the SLB console. In other cases, the listener port numbers must be unique.

    -   UAE \(Dubai\)
    -   Australia \(Sydney\)
    -   UAE \(Dubai\)
    -   UK \(London\)
    -   Germany \(Frankfurt\)
    -   US \(Silicon Valley\)
    -   US \(Virginia\)
    -   Indonesia \(Jakarta\)
    -   Japan \(Tokyo\)
    -   India \(Mumbai\)
    -   Singapore
    -   Malaysia \(Kuala Lumpur\)
    -   China \(Hong Kong\)
    -   China \(Shenzhen\)
    -   China \(Hohhot\)
    -   China \(Qingdao\)
    -   China \(Chengdu\)
    -   China \(Zhangjiakou\)
    -   China \(Shanghai\)
 |
    |**Advanced configurations**|
    |**Scheduling Algorithm**|SLB supports four scheduling algorithms: round robin, weighted round robin \(WRR\), weighted least connections \(WLC\), and consistent hash.     -   **Weighted Round-Robin \(WRR\)**: A backend server with a higher weight receives more requests.
    -   **Round-Robin \(RR\)**: Requests are evenly and sequentially distributed to backend servers.
    -   **Weighted Least Connections \(WLC\)**: A server with a higher weight receives more requests. When the weight values of two backend servers are the same, the backend server with a smaller number of connections is more likely to be polled.
    -   **Consistent Hash \(CH\)**:

        -   **Source IP**: the consistent hash based on source IP addresses. Requests from the same source IP address are scheduled to the same backend server.
        -   **Tuple**: the consistent hash based on four factors: source IP address + destination IP address + source port + destination port. The same streams are scheduled to the same backend server.
        -   **QUIC ID**: the consistent hash based on the QUIC Connection ID. The same QUIC Connection IDs are scheduled to the same backend server.

**Note:** The QUIC protocol is rapidly evolving. The algorithm is based on [draft-ietf-quic-transport-10](https://datatracker.ietf.org/doc/draft-ietf-quic-transport/10/) and does not guarantee the compatibility of all QUIC versions. We recommend that you do enough tests before using it for the production environment.

**Note:** 

Currently, the Consistent Hash \(CH\) algorithm is only supported in the following regions:

        -   Japan \(Tokyo\)
        -   Australia \(Sydney\)
        -   Malaysia \(Kuala Lumpur\)
        -   Indonesia \(Jakarta\)
        -   Germany \(Frankfurt\)
        -   US \(Silicon Valley\)
        -   US \(Virginia\)
        -   UAE \(Dubai\)
        -   China \(Hohhot\)
        -   UK \(London\)
        -   Zone B and Zone C of Singapore
        -   China \(Hong Kong\)
        -   China \(Qingdao\)
        -   China \(Zhangjiakou\)
        -   China \(Chengdu\)
        -   Zone I and Zone H of China \(Hangzhou\)
        -   Zone G and Zone H of China \(Beijing\)
        -   Zone D and Zone E of China \(Shenzhen\)
        -   Zone F and Zone G of China \(Shanghai\)
 |
    |**Enable Access Control**|Select whether to enable the access control function.|
    |**Access Control Method**| Select an access control method after you enable the access control function:

     -   **Whitelist**: Only requests from IP addresses or CIDR blocks in the selected access control list are forwarded. It applies to scenarios where the application only allows access from some specific IP addresses.

Enabling a whitelist poses some risks to your services. After a whitelist is configured, only the IP addresses in the list can access the listener. If you enable the whitelist without adding any IP addresses in the corresponding access control list, all requests are forwarded.

    -   **Blacklist**: Requests from IP addresses or CIDR blocks in the selected access control list are not forwarded. It applies to scenarios where the application only denies access from some specific IP addresses.

If you enable a blacklist without adding any IP addresses in the corresponding access control list, all requests are forwarded.

 |
    |**Access Control List**|Select an access control list as the whitelist or the blacklist. **Note:** An IPv6 instance can only be associated with IPv6 access control lists and an IPv4 instance can only be associated with IPv4 access control lists. For more information, see [Configure an access control list](reseller.en-US/Archives/User Guide (Old Console)/Access control/Configure an access control list.md#).

 |
    |**Enable Peak Bandwidth Limit**| Select whether to configure the listener bandwidth.

 If the SLB instance is charged based on bandwidth, you can set different peak bandwidth values for different listeners to limit the traffic passing through each listener. The sum of the peak bandwidth values of all listeners under an SLB instance cannot exceed the bandwidth value of that SLB instance.

 By default, all listeners share the bandwidth of the SLB instance.

 **Note:** SLB instances billed by traffic have no bandwidth peak limit by default.

 |
    |**Get Client Source IP Address**|Backend servers of a UDP listener can directly obtain source IP addresses of clients. **Note:** UDP listeners of an SLB instance of the classic network do not support viewing source IP addresses.

 |
    |**Automatically Enable Listener After Creation**|Choose whether to start the listener after the listener is configured. The listener is started by default.|

2.  Click **Next**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16161/15657977077426_en-US.png)


## Step 3 Add backend servers {#section_2p9_p8x_lc1 .section}

After configuring the listener, you need to add backend servers to process requests. You can use the default server group configured for the SLB instance, or configure a VServer group or an active/standby server group for the listener. For more information, see [Backend server overview](../reseller.en-US/Backend servers/Backend server overview.md#).

In this example, use the default server group.

1.  Select **Default Server Group** and then click **Add More**. 

    ![Add default servers](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16139/156579770710030_en-US.png)

2.  Select the ECS instances to add, and then click **Next: Set Weight and Port**. 

    ![Configure weights](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16139/15657977077499_en-US.png)

3.  Configure ports and weights for the added backend servers \(ECS instances\). 
    -   Port

        The port opened on the backend server to receive requests. The port number is in the range of 1 to 65535. Ports of backend servers can be the same in an SLB instance.

    -   Weight

        The weight of the backend server. A backend server with a higher weight receives more requests.

        **Note:** If the weight is set to 0, no requests are sent to the backend server.

        ![Set weights](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16139/15657977077504_en-US.png)

4.  Click **Next**.

## Step 4 Configure health checks {#section_gum_68h_177 .section}

SLB checks the service availability of backend servers by performing health checks. The health check function improves the overall availability of your services and avoids the impact of backend server failures. Click **Modify** to change health check configurations. For more information, see [../DNSLB19102266/EN-US\_TP\_15663.md\#](../reseller.en-US/Health check/Health check overview.md#).

## Step 5 Submit the configurations {#section_n00_0iz_5ti .section}

To submit the listener configurations, follow these steps:

1.  On the Submit page, check the listener configurations. You can click **Modify** to change the configurations.
2.  Click **Submit**.
3.  On the Submit page, click **OK** after the configurations are successful. 

    After the configurations are successful, you can view the created listener on the Listeners page.


[Configure health checks](../reseller.en-US/Health check/Configure health checks.md#)

[Manage a default server group](../reseller.en-US/Backend servers/Manage a default server group.md#)

[Manage a VServer group](../reseller.en-US/Backend servers/Manage a VServer group.md#)

[Manage an active/standby server group](../reseller.en-US/Backend servers/Manage an active__standby server group.md#)

[Configure access control](../reseller.en-US/Access control/Configure access control.md#)

[CreateLoadBalancerUDPListener](../reseller.en-US/Developer Guide/UDP listeners/CreateLoadBalancerUDPListener.md#)

