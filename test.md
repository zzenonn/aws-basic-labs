<header>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.5.0/css/all.css" integrity="sha384-B4dIYHKNBt8Bc12p+WXckhzcICo0wtJAoU8YZTY5qE0Id1GSseTk6S+L3BlXeVIU" crossorigin="anonymous">
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <!-- Optional theme -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">
    <!-- Latest compiled and minified JavaScript -->
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
 </header>
 <!--include:Logo-->
 <style type="text/css">
    body {
    font-family:  "Roboto", "Helvetica", sans-serif;
    font-size: 12pt;
    font-color: Gray;
    line-height: 1.6;
    margin: 50px;
    }
    p {
    list-style-position: inside;
    }
    #ssb_blue {
    background-color: #257ACF;
    font-weight: bold;
    font-size: 90%;
    color: white;
    border-radius: 5px;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssb_s3_blue {
    background-color: #329ad6;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_voc_grey {
    background-color: #F2F3F4;
    font-weight: normal;
    font-size: 90%;
    color: black;
    border-radius: 3px;
    border: 1px solid gray;
    padding-top: 5px;
    padding-bottom: 5px;
    padding-left: 6px;
    padding-right: 6px;
    white-space: nowrap;
    }
    #ssb_grey {
    background-color: #DEDEDE;
    font-weight: bold;
    font-size: 90%;
    color: #444;
    position: relative;
    top:-1px;
    border-radius: 5px;
    border-width: 1px;
    border-style: solid;
    border-color: #444;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    white-space: nowrap;
    }
    #ssl_alexa_ocean {
    color: #00a0d2;
    font-weight: bold;
    }
    #ssb_services {
    background-color: #232f3e;
    font-weight: bold;
    font-size: 90%;
    color: white;
    padding-top: 3px;
    padding-bottom: 3px;
    padding-left: 10px;
    padding-right: 10px;
    }
    #ssb_orange {
    background-color:#ec7211;
    font-weight:bold;
    font-size:90%;
    color:white;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    white-space:
    nowrap;
    }
    #ssbox_cloudformation_blue {
    font-weight:bold;
    background-color:#f1faff;
    font-size:90%;
    border-color:#00A1C9;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
    #ssb_ssm_white {
    background-color:white;
    font-weight:bold;
    font-size:90%;
    color:#545b64;
    border-color:#545b64;
    border-radius:2px;
    border-width:1px;
    border-style:solid;
    padding-top:3px;
    padding-bottom:3px;
    padding-left:10px;
    padding-right:10px;
    }
 </style>

# Lab 2: Build your VPC and Launch a Web Server

<!-- Note to translators: This is based on Technical Essentials Lab 1. Copy the translation from there. Do not re-translate the whole document.-->

&nbsp;

**Version 4.6.6 (TESS1)**

In this lab, you will use Amazon Virtual Private Cloud (VPC) to create your own VPC and add additional components to produce a customized network. You will also create security groups for your EC2 instance. You will then configure and customize an EC2 instance to run a web server and launch it into the VPC.

**Amazon Virtual Private Cloud (Amazon VPC)** enables you to launch Amazon Web Services (AWS) resources into a virtual network that you defined. This virtual network closely resembles a traditional network that you would operate in your own data center, with the benefits of using the scalable infrastructure of AWS. You can create a VPC that spans multiple Availability Zones.

&nbsp;

**Scenario**

In this lab you build the following infrastructure:

<img src="../../images/architecture.png" alt="Architecture" width="800">

&nbsp;&nbsp;

**Objectives**

After completing this lab, you can:

- Create a VPC.
- Create subnets.
- Configure a security group.
- Launch an EC2 instance into a VPC.

&nbsp;

**Duration**

This lab takes approximately **30 minutes** to complete.

&nbsp;&nbsp;
## Accessing the AWS Management Console

1. At the top of these instructions, click <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

    A Start Lab panel opens displaying the lab status.

2. Wait until you see the message "**Lab status: ready**", then click the **X** to close the Start Lab panel.

3. At the top of these instructions, click <span id="ssb_voc_grey">AWS</span>

    This will open the AWS Management Console in a new browser tab. The system will automatically log you in.

    **Tip**: If a new browser tab does not open, there will typically be a banner or icon at the top of your browser indicating that your browser is preventing the site from opening pop-up windows. Click on the banner or icon and choose "Allow pop ups."

4. Arrange the AWS Management Console tab so that it displays along side these instructions. Ideally, you will be able to see both browser tabs at the same time, to make it easier to follow the lab steps.

&nbsp;
___
## Task 1: Create Your VPC

In this task, you will use the VPC Wizard to create a VPC an Internet Gateway and two subnets in a single Availability Zone. An **Internet gateway (IGW)** is a VPC component that allows communication between instances in your VPC and the Internet.

After creating a VPC, you can add **subnets**. Each subnet resides entirely within one Availability Zone and cannot span zones. If a subnet's traffic is routed to an Internet Gateway, the subnet is known as a *public subnet*. If a subnet does not have a route to the Internet gateway, the subnet is known as a *private subnet*.

The wizard will also create a _NAT Gateway_, which is used to provide internet connectivity to EC2 instances in the private subnets.

5. In the **AWS Management Console**, on the <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span> menu, click **VPC**.

6. Click <span id="ssb_blue">Launch VPC Wizard</span>

7. In the left navigation pane, click **VPC with Public and Private Subnets** (the second option).

8. Click <span id="ssb_blue">Select</span> then configure:

   - **VPC name:** `Lab VPC`
   - **Availability Zone:** Select the *first* Availability Zone
   - **Public subnet name:** `Public Subnet 1`
   - **Availability Zone:** Select the *first* Availability Zone (the same as used above)
   - **Private subnet name:** `Private Subnet 1`
   - **Elastic IP Allocation ID:** Click in the box and select the displayed IP address

9. Click <span id="ssb_blue">Create VPC</span>

    The wizard will create your VPC.

10. Once it is complete, click <span id="ssb_blue">OK</span>

    The wizard has provisioned a VPC with a public subnet and a private subnet in the same Availability Zone, together with route tables for each subnet:

    <img src="../../images/task1.png" alt="Task 1" width="800">

    &nbsp;

    The Public Subnet has a CIDR of **10.0.0.0/24**, which means that it contains all IP addresses starting with **10.0.0.x**.

    The Private Subnet has a CIDR of **10.0.1.0/24**, which means that it contains all IP addresses starting with **10.0.1.x**.

&nbsp;
___
## Task 2: Create Additional Subnets

In this task, you will create two additional subnets in a second Availability Zone. This is useful for creating resources in multiple Availability Zones to provide _High Availability_.

11. In the left navigation pane, click **Subnets**.

    First, you will create a second Public Subnet.

12. Click <span id="ssb_blue">Create subnet</span> then configure:

    - **Name tag:** `Public Subnet 2`
    - **VPC:** _Lab VPC_
    - **Availability Zone:** Select the *second* Availability Zone
    - **IPv4 CIDR block:** `10.0.2.0/24`

    The subnet will have all IP addresses starting with **10.0.2.x**.

13. Click <span id="ssb_blue">Create</span> then click <span id="ssb_blue">Close</span>

    You will now create a second Private Subnet.

14. Click <span id="ssb_blue">Create subnet</span> then configure:

    - **Name tag:** `Private Subnet 2`
    - **VPC:** _Lab VPC_
    - **Availability Zone:** Select the *second* Availability Zone
    - **CIDR block:** `10.0.3.0/24`

    The subnet will have all IP addresses starting with **10.0.3.x**.

15. Click <span id="ssb_blue">Create</span> then click <span id="ssb_blue">Close</span>

    You will now configure the Private Subnets to route internet-bound traffic to the NAT Gateway so that resources in the Private Subnet are able to connect to the Internet, while still keeping the resources private. This is done by configuring a _Route Table_.

    A *route table* contains a set of rules, called *routes*, that are used to determine where network traffic is directed. Each subnet in a VPC must be associated with a route table; the route table controls routing for the subnet.

16. In the left navigation pane, click **Route Tables**.

17. Select <i class="far fa-check-square"></i> the route table with **Main = Yes** and **VPC = Lab VPC**. (Expand the _VPC ID_ column if necessary to view the VPC name.)

18. In the lower pane, click the **Routes** tab.

    Note that **Destination 0.0.0.0/0** is set to **Target nat-xxxxxxxx**. This means that traffic destined for the internet (0.0.0.0/0) will be sent to the NAT Gateway. The NAT Gateway will then forward the traffic to the internet.

    This route table is therefore being used to route traffic from Private Subnets. You will now add a name to the Route Table to make this easier to recognize in future.

19. In the **Name** column for this route table, click the pencil <i class="fas fa-pencil-alt"></i> then type `Private Route Table` and click <i class="fas fa-check-circle"></i>

20. In the lower pane, click the **Subnet Associations** tab.

    You will now associate this route table to the Private Subnets.

21. Click <span id="ssb_grey">Edit subnet associations</span>

22. Select <i class="far fa-check-square"></i> both **Private Subnet 1** and **Private Subnet 2**.

    <i class="fas fa-comment"></i> You can expand the _Subnet ID_ column to view the Subnet names.

23. Click <span id="ssb_blue">Save</span>

    You will now configure the Route Table that is used by the Public Subnets.

24. Select <i class="far fa-check-square"></i> the route table with **Main = No** and **VPC = Lab VPC** (and deselect any other subnets).

25. In the **Name** column for this route table, click the pencil <i class="fas fa-pencil-alt"></i> then type `Public Route Table`, and click <i class="fas fa-check-circle"></i>

26. In the lower pane, click the **Routes** tab.

    Note that **Destination 0.0.0.0/0** is set to **Target igw-xxxxxxxx**, which is the Internet Gateway. This means that internet-bound traffic will be sent straight to the internet via the Internet Gateway.

    You will now associate this route table to the Public Subnets.

27. Click the **Subnet Associations** tab.

28. Click <span id="ssb_grey">Edit subnet associations</span>

29. Select <i class="far fa-check-square"></i> both **Public Subnet 1** and **Public Subnet 2**.

30. Click <span id="ssb_blue">Save</span>

    Your VPC now has public and private subnets configured in two Availability Zones:

    <img src="../../images/task2.png" alt="Task 2" width="800">

&nbsp;
___
## Task 3: Create a VPC Security Group

In this task, you will create a VPC security group, which acts as a virtual firewall. When you launch an instance, you associate one or more security groups with the instance. You can add rules to each security group that allow traffic to or from its associated instances.

31. In the left navigation pane, click **Security Groups**.

32. Click <span id="ssb_blue">Create security group</span> and then configure:

    - **Security group name:** `Web Security Group`
    - **Description:** `Enable HTTP access`
    - **VPC:** _Lab VPC_

33. Click <span id="ssb_blue">Create</span> then click <span id="ssb_blue">Close</span>

    You will now add a rule to the security group to permit inbound web requests.

34. Select <i class="far fa-check-square"></i> **Web Security Group**.

35. Click the **Inbound Rules** tab.

36. Click <span id="ssb_grey">Edit rules</span>

37. Click <span id="ssb_grey">Add Rule</span> then configure:

    - **Type:** _HTTP_
    - **Source:** _Anywhere_
    - **Description:** `Permit web requests`

38. Click <span id="ssb_blue">Save rules</span> then click <span id="ssb_blue">Close</span>

    You will use this security group in the next task when launching an Amazon EC2 instance.

&nbsp;
___
## Task 4: Launch a Web Server Instance

In this task, you will launch an Amazon EC2 instance into the new VPC. You will configure the instance to act as a web server.

39. On the <span id="ssb_services">Services <i class="fas fa-angle-down"></i></span> menu, click **EC2**.

40. Click <span id="ssb_blue">Launch Instance</span>

    First, you will select an _Amazon Machine Image (AMI)_, which contains the desired Operating System.

41. In the row for **Amazon Linux 2** (at the top), click <span id="ssb_blue">Select</span>

    The _Instance Type_ defines the hardware resources assigned to the instance.

42. Select **t2.micro** (shown in the _Type_ column).

43. Click <span id="ssb_grey">Next: Configure Instance Details</span>

    You will now configure the instance to launch in a Public Subnet of the new VPC.

44. Configure these settings:

    - **Network:** _Lab VPC_
    - **Subnet:** _Public Subnet 2_ (_not_ Private!)
    - **Auto-assign Public IP:** _Enable_

45. Expand the <i class="fas fa-caret-right"></i> **Advanced Details** section (at the bottom of the page).

46. Copy and paste this code into the **User data** box:

    ```bash
    #!/bin/bash
    # Install Apache Web Server and PHP
    yum install -y httpd mysql php
    # Download Lab files
    wget https://aws-tc-largeobjects.s3.amazonaws.com/AWS-TC-AcademyACF/acf-lab3-vpc/lab-app.zip
    unzip lab-app.zip -d /var/www/html/
    # Turn on web server
    chkconfig httpd on
    service httpd start
    ```

    This script will be run automatically when the instance launches for the first time. The script loads and configures a PHP web application.

47. Click <span id="ssb_grey">Next: Add Storage</span>

    You will use the default settings for storage.

48. Click <span id="ssb_grey">Next: Add Tags</span>

    Tags can be used to identify resources. You will use a tag to assign a Name to the instance.

49. Click <span id="ssb_grey">Add Tag</span> then configure:

    - **Key:** `Name`
    - **Value:** `Web Server 1`

50. Click <span id="ssb_grey">Next: Configure Security Group</span>

    You will configure the instance to use the _Web Security Group_ that you created earlier.

51. Select <i class="far fa-dot-circle"></i> **Select an existing security group**

52. Select <i class="far fa-check-square"></i> **Web Security Group**.

    This is the security group you created in the previous task. It will permit HTTP access to the instance.

53. Click <span id="ssb_blue">Review and Launch</span>

54. When prompted with a *warning* that you will not be able to connect to the instance through port 22, click <span id="ssb_blue">Continue</span>

55. Review the instance information and click <span id="ssb_blue">Launch</span>

56. In the **Select an existing keypair** dialog, select <i class="far fa-check-square"></i> **I acknowledge...**.

57. Click <span id="ssb_blue">Launch Instances</span> and then click <span id="ssb_blue">View Instances</span>

58. Wait until **Web Server 1** shows *2/2 checks passed* in the **Status Checks** column.

    <i class="fas fa-comment"></i> This may take a few minutes. Click refresh <i class="fas fa-sync"></i> in the top-right every 30 seconds for updates.

    You will now connect to the web server running on the EC2 instance.

59. Copy the **Public DNS (IPv4)** value shown in the **Description** tab at the bottom of the page.

60. Open a new web browser tab, paste the **Public DNS** value and press Enter.

    You should see a web page displaying the AWS logo and instance meta-data values.

    The complete architecture you deployed is:

    <img src="../../images/architecture.png" alt="Architecture" width="800">

&nbsp;
___
## Lab Complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

61. Click <span id="ssb_voc_grey">End Lab</span> at the top of this page and then click <span id="ssb_blue">Yes</span> to confirm that you want to end the lab.  

    A panel will appear, indicating that "DELETE has been initiated... You may close this message box now."

62. Click the **X** in the top right corner to close the panel.

For feedback, suggestions, or corrections, please email us at: *aws-course-feedback@amazon.com*

