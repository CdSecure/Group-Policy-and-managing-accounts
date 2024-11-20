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

**Simulating Account Lockouts and Configuring Account Lockout Policies**

In this step, we will configure the domain lockout policy using the Group Policy Management Console (GPMC), then simulate account lockouts by performing multiple failed login attempts with a user account. Finally, we will verify the lockout in the security logs to ensure that the policies are functioning as intended.

### **Steps:**

1. **Log into DC-1 as an Administrator:**
   
   - **Access DC-1:**
     - Open **Remote Desktop Protocol (RDP)** on your local machine.
     - Enter DC-1's **public IP address** and click **Connect**.
     - Log in using your **domain administrator credentials** (e.g., `mydomain.com\jane_admin` and password).

2. **Configure Domain Lockout Policy:**
   
   - **Open Group Policy Management Console (GPMC):**
     - On DC-1, click the **Start** button at the bottom left corner.
     - Type **"Run"** in the search bar and press **Enter**.
     - In the Run dialog, type **`GPMC.MSC`** and press **Enter** to launch the Group Policy Management Console.

   - **Edit the Default Domain Policy:**
     - In the GPMC, navigate to **Forest** > **Domains** > **mydomain.com**.
     - Locate **"Default Domain Policy"**, right-click on it, and select **Edit**.

   - **Set Account Lockout Policies:**
     - In the **Group Policy Management Editor**, expand the following path:
       - **Computer Configuration**
       - **Policies**
       - **Windows Settings**
       - **Security Settings**
       - **Account Policies**
       - **Account Lockout Policy**
     
     - **Configure the Following Settings:**
       1. **Account lockout threshold:**
          - Double-click on **"Account lockout threshold"**.
          - Set it to **5 invalid attempts**.
          - Click **OK**.
       2. **Account lockout duration:**
          - Double-click on **"Account lockout duration"**.
          - Set it to **30 minutes**.
          - Click **OK**.
       3. **Reset account lockout counter after:**
          - Double-click on **"Reset account lockout counter after"**.
          - Set it to **30 minutes**.
          - Click **OK**.
   
   - **Apply and Update Group Policy:**
     - To ensure that the client VM receives the updated policy, run the following command on **Client-1**:
       ```
       gpupdate /force
       ```
     - This forces an immediate update of Group Policy settings on the client.

3. **Simulate Account Lockouts by Triggering Failed Login Attempts:**
   
   - **Select a Random User Account:**
     - On DC-1, open **Active Directory Users and Computers**.
     - Navigate to the appropriate Organizational Unit (e.g., **_EMPLOYEES**).
     - Select a **random user account** (e.g., `jdoe`).

   - **Obtain Client-1's Public IP Address:**
     - In the Azure portal, navigate to **Virtual Machines** and select **Client-1**.
     - Copy the **Public IP address** for use in the RDP connection.

   - **Initiate RDP Connection to Client-1:**
     - Open **Remote Desktop Connection** on your local machine.
     - Enter **Client-1's public IP address** and click **Connect**.

   - **Attempt Failed Logins:**
     - On the login screen, enter the **username** (e.g., `mydomain.com\jdoe`).
     - Enter an **incorrect password** deliberately.
     - Click **OK** to attempt the login.
     - **Repeat this process 10 times** to trigger the account lockout policy.
     - After **5 failed attempts**, the account should be locked, and a message indicating the lockout should appear.

4. **Verify Account Lockout in Security Logs:**
   
   - **Access Event Viewer on DC-1:**
     - On DC-1, click the **Start** button.
     - Type **"Event Viewer"** in the search bar and press **Enter**.

   - **Navigate to Security Logs:**
     - In the **Event Viewer** window, expand **Windows Logs** and select **Security**.

   - **Review Lockout Events:**
     - Look for **Event ID 4740**, which indicates an account lockout.
     - Confirm that the lockout was triggered for the selected user account (`jdoe`).
<p>
<img width="442" alt="Screenshot 2024-11-19 at 5 44 14 PM" src="https://github.com/user-attachments/assets/1e1956fd-528b-45d3-9610-6fe162b71235">
<br />
<img width="399" alt="Screenshot 2024-11-19 at 6 11 07 PM" src="https://github.com/user-attachments/assets/c1fa7914-bc08-4f03-b708-02461eae648e">
<br />
<img width="785" alt="Screenshot 2024-11-20 at 2 18 33 PM" src="https://github.com/user-attachments/assets/d1ae59ea-8fb4-43d2-b946-ab61cbdf2100">
</p>
