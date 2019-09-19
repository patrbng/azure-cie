# Exercise - Configure network settings

_Recording of exercise review_
_https://siriusazureciestg.blob.core.windows.net/videos/section4/3-Exercise-Recording.mp4_

* 10 minutes

Let's apply a network security group to our network, so that we only allow HTTP traffic through our server.

## Login to the Azure Portal

1. Open the [Azure portal](https://portal.azure.com) in a browser.

2. Sign into Azure using the Microsoft account email address and password you created for this session.

## Create a network security group

Azure should have created a security group for us because we indicated we wanted SSH access. But let's create a new security group, so you can walk through the entire process. This is particularly important if you decide to create your virtual network _before_ your VMs. As mentioned earlier, security groups are _optional_ and not necessarily created with the network.

1. In the [Azure portal](https://portal.azure.com/), click the **Create a resource** button in the left-corner sidebar to start a new resource creation.

2. Type **Network security group** into the filter box and select the matching item in the list.

3. Make sure the **Resource Manager** deployment model is selected and click **Create**.

4. Provide a **Name** for your security group. Again, naming conventions are a good idea here. Let's use **test-web-eus-nsg1** for **Test Web Network Security Group #1 in East US**. You'll likely want to change the location portion of the name to reflect where you put the security group.

5. Select the proper **Subscription** and use your existing **Resource group**.

6. Finally, put it into the same **Location** as the VM / virtual network. This is important; you won't be able to apply this resource if it's in a different location.

7. Click **Create** to create the group.

## Add a new inbound rule to our network security group

Deployment should complete quickly. When it's finished, we can add new rules to our security group:

1. Find the new security group resource and select it in the Azure portal.

2. On the overview page, you'll find that it has some default rules created to lock down the network.

    On the inbound side:

    * All incoming traffic from one VNet to another is allowed. This lets resources on the VNet talk to each other.
    * Azure Load Balancer **probe** requests to ensure the VM is alive.
    * All other inbound traffic is denied.

    On the outbound side:

    * All in-network traffic on the VNet is allowed.
    * All outbound traffic to the internet is permitted.
    * All other outbound traffic is denied.

    **Note**
    These default rules are set with high-priority values, which means that they get evaluated _last_. They cannot be changed or deleted, but you can _override_ them by creating more specific rules to match your traffic with a lower priority value.

3. Click the **Inbound security rules** section in the **Settings** panel for the security group.

4. Click **+ Add** to add a new security rule.

    ![Screenshot of the Azure portal showing the inbound security rules settings with the Add button highlighted.](images/confignetlinux1.png)

    There are two ways to enter the information necessary for a security rule: basic and advanced. You can switch between them by clicking the button at the top of the **add** panel.

    ![A pair of screenshots of the Azure portal showing the toggle between Basic and Advanced rule input, with an arrow linking between the two states of the toggle button highlighted.](images/confignetlinux2.png)

    The advanced mode provides the ability to customize the rule completely. However, if you need to configure a known protocol, the basic mode is a bit easier to work with.

5. Switch to the **Basic** mode.

6. Add the information for our HTTP rule:

    * Set the **Service** to be HTTP. This will set your port range up for you.
    * Set the **Priority** to **1000**. It has to be a lower number than the default **Deny** rule. You can start the range at any value, but it's recommended that you give yourself some space in case an exception needs to be created later.
    * Give the rule a name; we'll use **allow-http-traffic**.
    * Give the rule a description.
7. Switch back to the **Advanced** mode. Notice that our settings are still present. We can use this panel to create more fine-grained settings. In particular, we would likely restrict the **Source** to be a specific IP address or range of IP addresses specific to the cameras. If you know the current IP address of your local computer, you can try that. Otherwise, leave the setting as **Any**, so you can test the rule.

8. Click **Add** to create the rule. This will update the list of inbound rules - notice they are in priority order, which is how they will be examined.

## Apply the security group

Recall that we can apply the security group to a network interface to guard a single VM or to a subnet where it would apply to any resources on that subnet. The latter approach tends to be the most common, so let's do that. We could get to this resource in Azure through either the virtual network resource or indirectly through the VM that is using the virtual network.

1. Switch to the **Overview** panel for the virtual machine. You can find the VM under **All Resources**.

2. Select the **Networking** item in the **Settings** section.

3. In the networking properties, you will find information about the networking applied to the VM, including the **Virtual network/subnet**. This is a clickable link to get to the resource. Click it to open the virtual network. This link is _also_ available on the **Overview** panel of the VM. Either of these will open the **Overview** of the virtual network.

4. In the **Settings** section, select the **Subnets** item.

5. We should have a single subnet defined (default) from when we created the VM + network earlier. Click the item in the list to open the details.

6. Click the **Network security group** entry.

7. Select your new security group: **test-web-eus-nsg1**. There should be another group here as well that was created with the VM.

8. Click **Save** to save the change. It will take a minute to apply to the network.

## Update the NSG on the network interface

Port 80 is open on the NSG applied to the subnet, but it's still going to be blocked because it's not currently allowed on the NSG applied to the network interface. Let's fix that and then we should be able to connect.

1. Switch back to the **Overview** panel for the virtual machine. You can find the VM under **All Resources**.

2. In the **Settings** section, select the **Networking** item.

3. You should see the NSG rules for the subnet in the top section and the NSG rules for the network interface in the bottom section of the same tab. In the bottom section, for the NSG rules for the network interface, select **Add inbound port rule**.

    ![Screenshot that shows the "Add inbound port rule" button in the network security group > network interface section.](images/confignetlinux3.png)

4. Switch to the **Basic** mode.

5. Add the information for our HTTP rule:

    * Set the **Service** to be HTTP. This will set your port range up for you.
    * Set the **Priority** to **310**.
    * Give the rule a name; we'll use **allow-http-traffic**.
    * Give the rule a description.
6. Click **Add** to create the rule.

## Verify the rules

Let's validate the change:

1. Switch back to the **Overview** panel for the virtual machine. You can find the VM under **All Resources**.

2. Select the **Networking** item in the **Settings** section.

3. In the network interface details, there is a link for **Effective security rules** that will quickly show you how rules are going to be evaluated. Click the link to open the analysis and make sure you see your new rules.

    ![Screenshot of the Azure portal showing the Effective security rules blade for our network.](images/confignetlinux4.png)

4. Of course, the best way to validate it's all working is to hit our server with an HTTP request. It should now work.

    ![Screenshot of a web browser showing the Apache default web page hosted at the IP of the new Linux VM.](images/confignetlinux5.png)

## One more thing

Security rules are tricky to get right. We made a mistake when we applied this new security group - we lost our SSH access! To fix this, you can add another rule to the security group applied to the subnet to allow SSH access. Make sure to restrict the inbound TCP/IP addresses for the rule to be the ones you own.

**Warning**
Always make sure to lock down ports used for administrative access. An even better approach is to create a VPN to link the virtual network to your private network and only allow RDP or SSH requests from that address range. You can also change the port used by SSH to be something other than the default. Keep in mind that changing ports is not sufficient to stop attacks. It simply makes it a little harder to discover.
