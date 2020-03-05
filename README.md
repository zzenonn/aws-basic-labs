# Module 03: Scale and Load Balance Your Architecture

&nbsp;&nbsp;

This lab walks you through using the Elastic Load Balancing (ELB) and Auto Scaling services to load balance and automatically scale your infrastructure.

**Elastic Load Balancing** automatically distributes incoming application traffic across multiple Amazon EC2 instances. It enables you to achieve fault tolerance in your applications by seamlessly providing the required amount of load balancing capacity needed to route application traffic.

**Auto Scaling** helps you maintain application availability and allows you to scale your Amazon EC2 capacity out or in automatically according to conditions you define. You can use Auto Scaling to help ensure that you are running your desired number of Amazon EC2 instances. Auto Scaling can also automatically increase the number of Amazon EC2 instances during demand spikes to maintain performance and decrease capacity during lulls to reduce costs. Auto Scaling is well suited to applications that have stable demand patterns or that experience hourly, daily, or weekly variability in usage.  

&nbsp;

**Objectives**

After completing this lab, you can:

- Create an Amazon Machine Image (AMI) from a running instance.
- Create a load balancer.
- Create a launch configuration and an Auto Scaling group.
- Automatically scale new instances within a private subnet
- Create Amazon CloudWatch alarms and monitor performance of your infrastructure.

&nbsp;

**Duration**

This lab takes approximately **30 minutes**.

&nbsp;

**Scenario**

You start with the following infrastructure:

![architecture](__assets/architecture-lab2.png)

&nbsp;

The final state of the infrastructure is:

&nbsp;

![architecture](__assets/architecture-lab3.png)

&nbsp;

&nbsp;
___
## Task 1: Create an AMI for Auto Scaling

In this task, you will create an AMI from the existing _Web Server 1_. This will save the contents of the boot disk so that new instances can be launched with identical content.

<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>


1. In the **AWS Management Console**, on the **Services** menu, click **EC2**.

2. In the left navigation pane, click **Instances**.

    First, you will confirm that the instance is running.

3. Wait until the **Status Checks** for **Web Server 1** displays *2/2 checks passed*. Click refresh to update.

    You will now create an AMI based upon this instance.

4. Select **Web Server 1**.

5. In the **Actions** menu, click **Image** &gt; **Create Image**, then configure:

   - **Image name:** `Web Server AMI`
   - **Image description:** `Lab AMI for Web Server`

6. Click **Create Image**

    The confirmation screen displays the **AMI ID** for your new AMI.

7. Click **Close**

    You will use this AMI when launching the Auto Scaling group later in the lab.
    
</p>
    </details>

&nbsp;
___
## Task 2: Create a Load Balancer

In this task, you will create a load balancer that can balance traffic across multiple EC2 instances and Availability Zones.

<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>

1. In the left navigation pane, click **Load Balancers**.

2. Click **Create Load Balancer**

    Several different types of load balancer are displayed. You will be using an _Application Load Balancer_ that operates at the request level (layer 7), routing traffic to targets — EC2 instances, containers, IP addresses and Lambda functions — based on the content of the request. For more information, see: <a href="https://aws.amazon.com/elasticloadbalancing/features/#compare" target="_blank">Comparison of Load Balancers</a>

3. Under **Application Load Balancer** click **Create** and configure:

    - **Name:** `LabELB`
    - **VPC:** _Lab VPC_ (In the **Availability Zones** section)
    - **Availability Zones:** Select the checkbox both to see the available subnets.
    - Select **Public Subnet 1** and **Public Subnet 2**

    This configures the load balancer to operate across multiple Availability Zones.

4. Click **Next: Configure Security Settings**

    You can ignore the _"Improve your load balancer's security."_ warning.

5. Click **Next: Configure Security Groups**

    A _Web Security Group_ has already been created for you, which permits HTTP access.

6. Select **Web Security Group** and deselect **default**.

7. Click **Next: Configure Routing**

    Routing configures where to send requests that are sent to the load balancer. You will create a _Target Group_ that will be used by Auto Scaling.

8. For **Name**, enter: `LabGroup`

9. Click **Next: Register Targets**

    Auto Scaling will automatically register instances as targets later in the lab.

10. Click **Next: Review**

11. Click **Create** then click **Close**

    The load balancer will show a state of _provisioning_. There is no need to wait until it is ready. Please continue with the next task.
</p>
    </details>

&nbsp;
___
## Task 3: Create a Launch Configuration and an Auto Scaling Group

In this task, you will create a _launch configuration_ for your Auto Scaling group. A launch configuration is a template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances such as the AMI, the instance type, a key pair, security group and disks.

<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>

1. In the left navigation pane, click **Launch Configurations**.

2. Click **Create launch configuration**

3. In the left navigation pane, click **My AMIs**.

    You will use the AMI that you created from the existing _Web Server 1_.

4. In the row for **Web Server AMI**, click **Select**

5. Select the **t3.micro** instance type and click **Next: Configure details**

    **Note:** If you have launched the lab in the us-east-1 Region, select the **t2.micro** instance type. To find the Region, look in the upper right-hand corner of the Amazon EC2 console.
    
6. Configure these settings:

    - **Name:** `LabConfig`
    - **Monitoring:** **Enable CloudWatch detailed monitoring**

    This allows Auto Scaling to react quickly to changing utilization.

7. Click **Next: Add Storage**

    You will use the default storage settings.

8. Click <span id="ssb_grey">Next: Configure Security Group</span>

    You will configure the launch configuration to use the _Web Security Group_ that has already been created for you.

9. Configure these settings:

    - Click **Select an existing security group**
    - Select **Web Security Group**
    - Click **Review**

10. Review the details of your launch configuration and click **Create launch configuration**

    You can ignore the warning "Improve security..." warning.

11. On the **Select an existing key pair** dialog:

    - Select **I acknowledge...**
    - Click **Create launch configuration**

    You will now create an Auto Scaling group that uses this Launch Configuration.

12. Click <span id="ssb_blue">Create an Auto Scaling group using this launch configuration</span>

13. Configure the following settings:

    - **Group name:** `Lab Auto Scaling Group`
    - **Group size:** Start with: `2` instances
    - **Network:** _Lab VPC_
    - You can ignore the message regarding "No public IP address"
    - **Subnet:** Select _Private Subnet 1 (10.0.1.0/24)_ **and** _Private Subnet 2 (10.0.3.0/24)_

    This will launch EC2 instances in private subnets across both Availability Zones.

14. Expand **Advanced Details**, then configure:

    - **Load Balancing:** Select **Receive traffic from one or more load balancers**
    - **Target Groups:** _LabGroup_
    - **Monitoring:** Select **Enable CloudWatch detailed monitoring**

    This will capture metrics at 1-minute intervals, which allows Auto Scaling to react quickly to changing usage patterns.

15. Click **Next: Configure scaling policies**

16. Select **Use scaling policies to adjust the capacity of this group**.

17. Modify the **Scale between** text boxes to scale between **2** and **6** instances.

    This will allow Auto Scaling to automatically add/remove instances, always keeping between 2 and 6 instances running.

18. In **Scale Group Size**, configure:

    - **Metric type:** _Average CPU Utilization_
    - **Target value:** `50`

    This tells Auto Scaling to maintain an _average_ CPU utilization _across all instances_ at 60%. Auto Scaling will automatically add or remove capacity as required to keep the metric at, or close to, the specified target value. It adjusts to fluctuations in the metric due to a fluctuating load pattern.

19. Click <span id="ssb_grey">Next: Configure Notifications</span>

    Auto Scaling can send a notification when a scaling event takes place. You will use the default settings.

20. Click <span id="ssb_grey">Next: Configure Tags</span>

    Tags applied to the Auto Scaling group will be automatically propagated to the instances that are launched.

21. Configure the following:

    - **Key:** `Name`
    - **Value:** `Lab Instance`

22. Click **Review**

23. Review the details of your Auto Scaling group, then click **Create Auto Scaling group**. If you encounter an error **Failed to create Auto Scaling group**, then click **Retry Failed Tasks**.

24. Click **Close** when your Auto Scaling group has been created.

    Your Auto Scaling group will initially show an instance count of zero, but new instances will be launched to reach the **Desired** count of 2 instances.
    
</p>
    </details>

&nbsp;
___
## Task 4: Verify that Load Balancing is Working

In this task, you will verify that Load Balancing is working correctly.

<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>

1. In the left navigation pane, click **Instances**.

    You should see two new instances named **Lab Instance**. These were launched by Auto Scaling.

    <i class="fas fa-comment"></i> If the instances or names are not displayed, wait 30 seconds and click refresh <i class="fas fa-sync"></i> in the top-right.

    First, you will confirm that the new instances have passed their Health Check.

2. In the left navigation pane, click **Target Groups** (in the _Load Balancing_ section).

3. Click the **Targets** tab.

    Two **Lab Instance** targets should be listed for this target group.

4. Wait until the **Status** of both instances transitions to *healthy*. Click Refresh <i class="fas fa-sync"></i> in the upper-right to check for updates.

    _Healthy_ indicates that an instance has passed the Load Balancer's health check. This means that the Load Balancer will send traffic to the instance.

    You can now access the Auto Scaling group via the Load Balancer.

5. In the left navigation pane, click **Load Balancers**.

6. In the lower pane, copy the **DNS name** of the load balancer, making sure to omit "(A Record)".

    It should look similar to: _LabELB-1998580470.us-west-2.elb.amazonaws.com_

7. Open a new web browser tab, paste the DNS Name you just copied, and press Enter.

    The application should appear in your browser. This indicates that the Load Balancer received the request, sent it to one of the EC2 instances, then passed back the result.
 
 </p>
    </details>

&nbsp;
___
## Task 5: Test Auto Scaling

You created an Auto Scaling group with a minimum of two instances and a maximum of six instances. Currently two instances are running because the minimum size is two and the group is currently not under any load. You will now increase the load to cause Auto Scaling to add additional instances.

<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>

1. Return to the AWS management console, but do not close the application tab — you will return to it soon.

2. On the **Services** menu, click **CloudWatch**.

3. In the left navigation pane, click **Alarms** (*not* **ALARM**).

    Two alarms will be displayed. These were created automatically by the Auto Scaling group. They will automatically keep the average CPU load close to 50% while also staying within the limitation of having two to six instances.


    **Note**: Please follow these steps only if you do not see the alarms in 60 seconds.
    - On the **Services** menu, click **EC2**.
    - In the left navigation pane, click **Auto Scaling Groups** and then click on **Scaling Policies**.
    - Click **Actions⌄** and **Edit**.
    - Change the **Target Value** to `30`.
    - Click **Save**.
    - On the **Services** menu, click **CloudWatch**.
    - In the left navigation pane, click **Alarms** (*not* **ALARM**) and verify you see two alarms.

&nbsp;

4. Click the **OK** alarm, which has _AlarmHigh_ in its name.

    If no alarm is showing **OK**, wait a minute then click refresh in the top-right until the alarm status changes.

    The **OK** indicates that the alarm has _not_ been triggered. It is the alarm for **CPU Utilization > 60**, which will add instances when average CPU is high. The chart should show very low levels of CPU at the moment.

    You will now tell the application to perform calculations that should raise the CPU level.

5. Return to the browser tab with the web application.

6. Click **Load Test** beside the AWS logo.

    This will cause the application to generate high loads. The browser page will automatically refresh so that all instances in the Auto Scaling group will generate load. Do not close this tab.

7. Return to browser tab with the **CloudWatch** console.

    In less than 5 minutes, the **AlarmLow** alarm should change to **OK** and the **AlarmHigh** alarm status should change to *ALARM*.

    You can click Refresh in the top-right every 60 seconds to update the display.

    You should see the **AlarmHigh** chart indicating an increasing CPU percentage. Once it crosses the 60% line for more than 3 minutes, it will trigger Auto Scaling to add additional instances.

8. Wait until the **AlarmHigh** alarm enters the _ALARM_ state.

    You can now view the additional instance(s) that were launched.

9. On the **Services** menu, click **EC2**.

10. In the left navigation pane, click **Instances**.

    More than two instances labeled **Lab Instance** should now be running. The new instance(s) were created by Auto Scaling in response to the Alarm.
    
</p>
    </details>

&nbsp;
___
## Task 6: Terminate Web Server 1

In this task, you will terminate _Web Server 1_. This instance was used to create the AMI used by your Auto Scaling group, but it is no longer needed.


<details>
  <summary><strong>Step-by-step instructions (expand for details)</strong></summary>
  <p>

1. Select **Web Server 1** (and ensure it is the only instance selected).

2. In the **Actions** menu, click **Instance State** > **Terminate**.

3. Click <span id="ssb_bluey">Yes, Terminate</span>

</p>
    </details>

&nbsp;
