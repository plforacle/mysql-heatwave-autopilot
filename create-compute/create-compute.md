# Create Compute Instance for LAMP Server

![mysql heatwave](./images/mysql-heatwave-logo.jpg "mysql heatwave")

## Introduction

Create Linux Compute Instance to install Apache and PHP for the LAMP server. 


_Estimated Lab Time:_ 10 minutes

### Objectives

In this lab, you will be guided through the following tasks:

- Create SSH Key on OCI Cloud
- Create Compute Instance
- Install MySQL Shell on the Compute Instance
- Test connection to MySQL Database System

### Prerequisites

- An Oracle Trial or Paid Cloud Account
- Some Experience with MySQL Shell
- Must Complete Lab 5

## Task 1: Create SSH Key on OCI Cloud Shell

The Cloud Shell machine is a small virtual machine running a Bash shell which you access through the Oracle Cloud Console (Homepage). You will start the Cloud Shell and generate a SSH Key to use  for the Bastion  session.

1. To start the Oracle Cloud shell, go to your Cloud console and click the cloud shell icon at the top right of the page. This will open the Cloud Shell in the browser, the first time it takes some time to generate it.

    cloudshell-main

    ![cloud shell main](./images/cloud-shell.png  "cloud shell main " )

    ![cloud shell button](./images/cloud-shell-setup.png  "cloud shell button " )

    ![open cloud shell](./images/cloud-shell-open.png "open cloud shell" )

    _Note: You can use the icons in the upper right corner of the Cloud Shell window to minimize, maximize, restart, and close your Cloud Shell session._

2. Once the cloud shell has started, create the SSH Key using the following command:

    ```bash
    <copy>ssh-keygen -t rsa</copy>
    ```

    Press enter for each question.

    Here is what it should look like.  

    ![ssh key](./images/ssh-key-show.png "ssh key show")

3. The public  and  private SSH keys  are stored in ~/.ssh/id_rsa.pub.

4. Examine the two files that you just created.

    ```bash
    <copy>cd .ssh</copy>
    ```

    ```bash
    <copy>ls</copy>
    ```

    ![ssh key list ](./images/shh-key-list.png "shh key list")

    Note in the output there are two files, a *private key:* `id_rsa` and a *public key:* `id_rsa.pub`. Keep the private key safe and don't share its content with anyone. The public key will be needed for various activities and can be uploaded to certain systems as well as copied and pasted to facilitate secure communications in the cloud.

## Task 2: Create Compute instance

You will need a compute Instance to connect to your brand new MySQL database.

1. Before creating the Compute instance open a notepad 

2. Do the followings steps to copy the public SSH key to the  notepad

    Open the Cloud shell
    ![open cloud shell large](./images/cloud-shell-open-large.png "open cloud shell large ")

    Enter the following command  

    ```bash
    <copy>cat ~/.ssh/id_rsa.pub</copy>
    ```

    ![ssh key display](./images/ssh-key-display.png "ssh key display ") 

3. Copy the id_rsa.pub content the notepad

    Your notepad should look like this
    ![show ssh key](./images/notepad-rsa-key.png "show ssh key")  

4. Minimize cloud shell

    ![minimize cloud shell](./images/ssh-key-display-minimize.png "minimize cloud shell")  

5. To launch a Linux Compute instance, go to 
    Navigation Menu
    Compute
    Instances
    ![navigation compute](./images/navigation-compute.png "navigation compute")

6. On Instances in **(root)** Compartment, click  **Create Instance**
    ![compute menu create instance](./images/compute-menu-create-instance.png "ccompute menu create instance ")

7. On Create Compute Instance 

    Enter Name

    ```bash
    <copy>HEATWAVE-Client</copy>
    ```

8. Make sure **(root)** compartment is selected 

9. On Placement, keep the selected Availability Domain

10. On Security, keep the default

    - Shielded instance: Disabled
    - Confidential computing:Disabled

      ![compute create security](./images/compute-create-security.png "compute create security ") 

11. On Image  keep the selected Image, Oracle Linux 8 and click Edit

      ![compute create image](./images/compute-create-image.png "compute create image ")  

12. Click Change Shape

      ![compute create change shape](./images/compute-create-change-shape.png "compute create change shape")  

13. Select Instance Shape: VM.Standard.E2.2

      ![compute create select shape](./images/compute-create-select-shape.png "compute create select shape")  

14. On Networking, click Edit

      ![compute create networking](./images/compute-create-networking.png "compute create networking ")  

15. Make sure **HEATWAVE-VCN**  and  and  **public subnet-HEATWAVE-VCN** are selected. Keep Public IPV4 address **Assign..** default

      ![compute create networking](./images/compute-create-networking-select.png "compute create networking ")

16. On Add SSH keys, paste the public key from the notepad.
  
    ![compute create add ssh key](./images/compute-create-add-ssh-key.png "compute create add ssh key ")

17. Keep Boot Volume default and Click **Create** button to finish creating your Compute Instance.

    ![compute create boot volue](./images/compute-create-boot-volume.png "compute create boot volume")

18. The New Virtual Machine will be ready to use after a few minutes. The state will be shown as 'Provisioning' during the creation
    ![compute provisioning](./images/compute-provisioning.png "compute provisioning ")

19. The state 'Running' indicates that the Virtual Machine is ready to use.

    ![compute active](./images/compute-active.png "compute active")

## Task 3: Connect to MySQL Database System

1. Copy the public IP address of the active Compute Instance to your notepad

    - Go to Navigation Menu
        - Compute
        - Instances
        - Copy **Public IP**
    ![navigation compute with instance](./images/navigation-compute-with-instance.png "navigation compute with instance ")

2. Copy the private IP address of the active MySQl Database Service Instance to your notepad

    - Go to Navigation Menu 
        - Databases 
        - MySQL
        - Click the `HEATWAVE-DB` Database System link
     ![navigation mysqlwith instance](./images/navigation-mysql-with-instance.png " navigation mysqlwith instance")

3. Copy the HEATWAVE-DB  `Private IP Address` to the notepad
     ![mysql detail ip](./images/mysql-detail-ip.png "mysql detail ip")

4. Your notepad should look like the following:
     ![notepad rsa key compute db](./images/notepad-rsa-key-compute-db.png "notepad rsa key compute db ")

5. Go to Cloud shell to SSH into the new Compute Instance

    Enter the username **opc** and the Public **IP Address**.

    Note: The **HEATWAVE-Client**  shows the  Public IP Address as mentioned on TASK 5: #1

    (Example: **ssh -i ~/.ssh/id_rsa opc@132.145.170...**) 

    ```bash
    <copy>ssh -i ~/.ssh/id_rsa opc@<your_compute_instance_ip></copy>
    ```

    For the **Are you sure you want to continue connecting (yes/no)?**
    - answer **yes**

    ![connect signin](./images/connect-first-signin.png "connect signin ")

    **Install MySQL Shell on the Compute Instance**

6. You will need a MySQL client tool to connect to your new MySQL DB System from your client machine.

    Install MySQL Shell with the following command (enter y for each question)

    **[opc@…]$**

    ```bash
    <copy>sudo yum install mysql-shell -y</copy>
    ```

    ![mysql shell install](./images/mysql-install-shell.png "mysql shell install ")

   **Connect to MySQL Database Service**

7. From your Compute instance, connect to HEATWAVE-DB  using the MySQL Shell client tool.

   The endpoint (IP Address) can be found in your notepad or  the MHEATWAVE-DB  System Details page, under the "Endpoint" "Private IP Address". 

    ![mysql endpoint private ip](./images/mysql-endpoint-private-ip.png "mysql endpoint private ip")

8. Use the following command to connect to MySQL using the MySQL Shell client tool. Be sure to add the MDS-HW private IP address at the end of the command. Also enter the admin user and the db password created on Lab 1

    (Example  **mysqlsh -uadmin -p -h10.0.1..**)

    **[opc@...]$**

    ```bash
    <copy>mysqlsh -uadmin -p -h 10.0.1... </copy>
    ```

    ![mysql shell first connect](./images/mysql-shell-first-connect.png "mysql shell first connect ")

9. View  the airportdb total records per table

    ```bash
    <copy>\sql</copy>
    ```

    ```bash
    <copy>SELECT table_name, table_rows FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = 'airportdb';</copy>
    ```

    ![airportdb total records](./images/airportdb-list.png "airportdb total records ") 

You may now **proceed to the next lab**

## Learn More

- [Cloud Shell](https://www.oracle.com/devops/cloud-shell/?source=:so:ch:or:awr::::Sc)
- [Virtual Cloud Network](https://docs.oracle.com/en-us/iaas/Content/Network/Concepts/overview.htm)
- [OCI Bastion Service ](https://docs.public.oneportal.content.oci.oraclecloud.com/en-us/iaas/Content/Bastion/Tasks/connectingtosessions.htm)



## Acknowledgements

- **Author** - Perside Foster, MySQL Solution Engineering 
- **Contributor** - Frédéric Descamps, MySQL Community Manager 
- **Last Updated By/Date** - Perside Foster, February 2022