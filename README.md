# Splunk Implementation Guide
![image](https://github.com/user-attachments/assets/f998bcdc-e3f1-4634-84eb-542d91b2865c)

## Introduction

Welcome to my comprehensive guide on installing and implementing Splunk! This documentation provides a step-by-step approach to setting up Splunk in a Windows environment, configuring it for log management, and establishing alerts for security monitoring.

Splunk is a powerful platform for searching, monitoring, and analyzing machine-generated data. It excels in handling large volumes of log data, making it a critical tool for IT operations, security monitoring, and data analytics. This guide covers the installation of Splunk Universal Forwarder and Splunk Enterprise, as well as the configuration of alerting mechanisms to enhance your system's visibility and responsiveness.

The primary goal of this documentation is to streamline the process of setting up Splunk from scratch, ensuring that users can efficiently deploy, configure, and utilize the platform to meet their specific needs. 


<b>By following this guide, you will learn how to:</b>

- Install and configure Splunk Universal Forwarder and Splunk Enterprise.
- Create and manage alerts to monitor for critical events and anomalies.
- Troubleshoot common issues encountered during the setup process.

## 1. Create Virtual Machines
1. Provision a VM running Windows Server 2022
2. Provision another VM running Windows 10.


*Note that the VMs should be on the same subnet.*

## 2. Configure Firewall and/or NSG's
Open the following ports on your firewall:
- 9997: For forwarders to the Splunk indexer
- 8000: For clients to access the Splunk Search page
- 8089: For splunkd (used by the deployment server)

*You may need to create a new inbound rule on the Windows firewall for these ports. Additionally, if your VMs are in an Azure environment then you may have to allow these ports through the NSGs.*

## 3. Install Splunk Enterprise on Windows Server 2022

1. Go to the [Splunk Website](https://www.splunk.com/en_us/download/splunk-enterprise.html?locale=en_us)
2. Create an account or sign in
3. Download the Windows MSI Installer
4. Run the Installer and Choose to Customize Options
5. Install Splunk as a local system
6. Set the credentials
7. Continue the install
8. After the job is done, verify installation by connecting to: https://127.0.0.1:8000/ and login with the credentials you set

![image](https://github.com/user-attachments/assets/cec6c05c-5040-477c-8acf-bbb89a45c83f)

![image](https://github.com/user-attachments/assets/e028a1ee-09e6-4352-b6e8-6e309dce79c4)


## 4. Install Splunk Universal Forwarder on Windows 10 Client
1. Download the [Splunk Universal Forwarder](https://www.splunk.com/en_us/download/universal-forwarder.html)
2. Launch the installer and accept the license agreement
3. Configure Forwarder:
    - Select "An on-premises Splunk Enterprise Instance"
    - Use the default configuration
    - Set a username
    - Set the Deployment Server: Enter your server's IP address or hostname and the default port (8089)
    - Set the Receiving Indexer: Enter your server's IP address or hostname and the default port (9997)
    - Complete the installation

*It's important to note that you must configure BOTH the Deployment Server and the Receiving Indexer.*

## 5. Verify Connectivity

1. Open PowerShell on the Windows 10 Client and run the following command to test connectivity: <br>
<b>"Test-NetConnection -ComputerName <Splunk_IP> -Port 9997"</b>

*Replace <Splunk_IP> with your Server's IP address*


2. If you are unable to establish a TCP connection over port 9997 to your Server, you must enable Splunk to listen on port 9997. 
    - Open an administrative CMD prompt on the Server and navigate to: <b>"C:\Program Files\Splunk\bin"</b>
    - Run the CMD: <b>"splunk enable listen 9997 -auth username:password"</b>

*Replace the username and password with the Splunk credentials you created to begin this lab*

3. Once you can establish a TCP connection over port 9997 to your Server, you should see your Windows client in the Splunk Console:
    - Open Splunk > Settings (Top Right) > Forwarder Manager

![image](https://github.com/user-attachments/assets/1d6c8c54-0725-4674-8223-77d887227528)

## 6. Adding Data
1. Open Splunk > Settings > Add Data
2. Choose to Forward data from a Splunk forwarder
3. Select your Windows client and create a Server Class Name

![image](https://github.com/user-attachments/assets/d008c1de-0a09-4aef-ad19-4760e996a8ae)

4. Click Next
5. Choose <b>Local Event Logs</b> and select Application, Security, Setup, and System logs

![image](https://github.com/user-attachments/assets/e27f4326-7691-496b-b5c3-4d1c85f75878)

6. Click Next
7. On the index page, select create new index.

![image](https://github.com/user-attachments/assets/4c0a550d-2758-4cd9-ab5c-3eec38d88f3e)

8. Submit your changes
9. Click on <b>Apps</b> in the top left corner and select <b>Search and Reporting</b>
10. Start a new search: <b>Index="winlog_clients"</b>

![image](https://github.com/user-attachments/assets/dbf56346-9205-44ce-b014-85331cd61de9)






## Conclusion

You’ve successfully managed to set up Splunk, troubleshoot issues, and configure data inputs to monitor your Windows event logs. With the firewall settings in place and the software installed, you are now equipped to perform searches and analyze your log data effectively. 

Thank you for following this guide as it walked you through the essential steps to get your Splunk environment up and running. Wishing you success with your Splunk setup and efficient monitoring of your systems!
