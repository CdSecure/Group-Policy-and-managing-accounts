<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# Group-Policy-and-managing-accounts

 Managing Account Lockouts and Password Resets Using Group Policy

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>Deployment Steps</h2>

<p>
  Managing Account Lockouts, Password Resets, Account Enablement/Disablement, and Monitoring Authentication and Security Logs

In this lab, we will learn how to handle account lockouts and password resets, enable and disable user accounts, and monitor authentication and security logs. These skills are essential for maintaining robust security and efficient user management within an Active Directory environment.
</p>

**Simulating Account Lockouts by Triggering Failed Login Attempts**

In this step, we will simulate account lockouts by deliberately attempting to log into Client-1 with incorrect passwords for a randomly selected user. This will generate security logs and demonstrate the account lockout policy in action.

### **Steps:**

1. **Log into DC-1:**
   
   - **Access DC-1:**
     - Open **Remote Desktop Protocol (RDP)** on your local machine.
     - Enter DC-1's **public IP address** and click **Connect**.
     - Authenticate using your **domain administrator credentials** (e.g., `mydomain.com\jane_admin` and password).

2. **Select a Random User Account:**
   
   - **Open Active Directory Users and Computers:**
     - On DC-1, click the **Start** button.
     - Type **"Active Directory Users and Computers"** in the search bar and press **Enter**.
   
   - **Choose a User:**
     - Navigate to the appropriate Organizational Unit (e.g., **_EMPLOYEES**).
     - Select a **random user account** that was previously created (e.g., `jdoe`).

3. **Attempt to Log into Client-1 with Incorrect Passwords:**
   
   - **Obtain Client-1's Public IP Address:**
     - In the Azure portal, navigate to **Virtual Machines** and select **Client-1**.
     - Copy the **Public IP address** for use in the RDP connection.
   
   - **Initiate RDP Connection to Client-1:**
     - Open **Remote Desktop Connection** on your local machine.
     - Enter **Client-1's public IP address** and click **Connect**.
   
   - **Attempt Failed Logins:**
     - On the login screen, enter the **username** of the selected user (e.g., `mydomain.com\babu.veve`).
     - Enter an **incorrect password** deliberately.
     - Click **OK** to attempt the login.
     - **Repeat this process 10 times** to trigger the account lockout policy.
     - After the predefined number of failed attempts (as configured in Group Policy), you should receive a message indicating that the account is locked.

4. **Verify Account Lockout in Security Logs:**
   
   - **Access Event Viewer on DC-1:**
     - On DC-1, click the **Start** button.
     - Type **"Event Viewer"** in the search bar and press **Enter**.
   
   - **Navigate to Security Logs:**
     - In the **Event Viewer** window, expand **Windows Logs** and select **Security**.
   
   - **Review Lockout Events:**
     - Look for **Event ID 4740** which indicates an account lockout.
     - Confirm that the lockout was triggered for the selected user account (`jdoe`).

