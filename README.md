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
<p>
<img width="442" alt="Screenshot 2024-11-19 at 5 44 14 PM" src="https://github.com/user-attachments/assets/1e1956fd-528b-45d3-9610-6fe162b71235">
<br />
<img width="399" alt="Screenshot 2024-11-19 at 6 11 07 PM" src="https://github.com/user-attachments/assets/c1fa7914-bc08-4f03-b708-02461eae648e">
<br />
<img width="785" alt="Screenshot 2024-11-20 at 2 18 33 PM" src="https://github.com/user-attachments/assets/d1ae59ea-8fb4-43d2-b946-ab61cbdf2100">
</p>


**Managing Account Lockouts, Password Resets, and Account Enablement/Disablement Using Group Policy**

In this lab, we will learn how to handle account lockouts and password resets, enable and disable user accounts, and monitor authentication and security logs within an Active Directory environment. These practices are essential for maintaining robust security and efficient user management.

### **Steps:**

#### 1. **Configure Domain Lockout Policy**

   - **Open Group Policy Management Console (GPMC):**
     1. **Access GPMC:**
        - On the domain controller (**DC-1**), click the **Start** button at the bottom left corner.
        - Type **"Run"** in the search bar and press **Enter**.
        - In the Run dialog, type **`GPMC.MSC`** and press **Enter** to launch the **Group Policy Management Console**.
   
   - **Edit the Default Domain Policy:**
     1. In the **GPMC**, navigate to **Forest** > **Domains** > **mydomain.com**.
     2. Locate **"Default Domain Policy"**, right-click on it, and select **"Edit"**.
   
   - **Set Account Lockout Policies:**
     1. In the **Group Policy Management Editor**, expand the following path:
        - **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Account Policies** > **Account Lockout Policy**.
     2. **Configure the Following Settings:**
        - **Account lockout threshold:**
          - Double-click **"Account lockout threshold"**.
          - Set it to **5 invalid attempts**.
          - Click **OK**.
        - **Account lockout duration:**
          - Double-click **"Account lockout duration"**.
          - Set it to **30 minutes**.
          - Click **OK**.
        - **Reset account lockout counter after:**
          - Double-click **"Reset account lockout counter after"**.
          - Set it to **30 minutes**.
          - Click **OK**.
   
   - **Apply and Update Group Policy:**
     1. To ensure that the client VM receives the updated policy, log into **Client-1** and open **Command Prompt** or **PowerShell**.
     2. Run the command:
        ```
        gpupdate /force
        ```
     3. This forces an immediate update of Group Policy settings on **Client-1**.

#### 2. **Simulate Account Lockouts by Triggering Failed Login Attempts**

   - **Log into DC-1 as an Administrator:**
     1. Open **Remote Desktop Protocol (RDP)** on your local machine.
     2. Enter **DC-1's public IP address** and click **Connect**.
     3. Authenticate using your **domain administrator credentials** (e.g., `mydomain.com\jane_admin` and password).
   
   - **Select a Random User Account:**
     1. On DC-1, open **Active Directory Users and Computers**:
        - Click the **Start** button.
        - Type **"Active Directory Users and Computers"** in the search bar and press **Enter**.
     2. Navigate to the appropriate Organizational Unit (e.g., **_EMPLOYEES**).
     3. Select a **random user account** (e.g., `jdoe`).
   
   - **Obtain Client-1's Public IP Address:**
     1. In the Azure portal, navigate to **Virtual Machines** and select **Client-1**.
     2. Copy the **Public IP address** for use in the RDP connection.
   
   - **Initiate RDP Connection to Client-1:**
     1. Open **Remote Desktop Connection** on your local machine.
     2. Enter **Client-1's public IP address** and click **Connect**.
   
   - **Attempt Failed Logins:**
     1. On the login screen, enter the **username** of the selected user (e.g., `mydomain.com\jdoe`).
     2. Enter an **incorrect password** deliberately.
     3. Click **OK** to attempt the login.
     4. **Repeat this process 10 times** to trigger the account lockout policy.
     5. After **5 failed attempts**, the account should be locked, and a message indicating the lockout should appear.

#### 3. **Verify Account Lockout in Security Logs**

   - **Access Event Viewer on DC-1:**
     1. On DC-1, click the **Start** button.
     2. Type **"Event Viewer"** in the search bar and press **Enter**.
   
   - **Navigate to Security Logs:**
     1. In the **Event Viewer** window, expand **Windows Logs** and select **Security**.
   
   - **Review Lockout Events:**
     1. Look for **Event ID 4740**, which indicates an account lockout.
     2. Confirm that the lockout was triggered for the selected user account (`jdoe`).

#### 4. **Reset the Client's Group Policy Log**

   - **Log into Client-1 as Jane Admin:**
     1. Open **Remote Desktop Protocol (RDP)** on your local machine.
     2. Enter **Client-1's public IP address** and click **Connect**.
     3. Authenticate using the domain administrator credentials (e.g., `mydomain.com\jane_admin` and password).
   
   - **Update Group Policy:**
     1. On **Client-1**, click the **Start** button at the bottom left corner.
     2. Type **"Command Prompt"** in the search bar and open it.
     3. In the command line, type:
        ```
        gpupdate /force
        ```
     4. Press **Enter** to force an update of Group Policy settings.

#### 5. **Unlock the User Account and Reset Password**

   - **Unlock the Account:**
     1. On **DC-1**, open **Active Directory Users and Computers**.
     2. Navigate to the **_EMPLOYEES** Organizational Unit.
     3. Locate and double-click the locked user account (`jdoe`).
     4. In the user properties window, go to the **Account** tab.
     5. Uncheck the **"Account is locked out"** option.
     6. Click **Apply**, then **OK** to unlock the account.
   
   - **Reset the Password:**
     1. In **Active Directory Users and Computers**, right-click on the user account (`jdoe`).
     2. Select **"Reset Password"** from the context menu.
     3. Enter a **new secure password** for the user.
     4. Confirm the password and click **OK** to reset.

#### 6. **Enable and Disable User Accounts**

   - **Disable an Account:**
     1. In **Active Directory Users and Computers**, right-click on the desired user account.
     2. Select **"Disable Account"** from the context menu.
     3. The user account will now appear with a downward arrow, indicating it is disabled.
     4. Attempting to log in with this account will result in a message indicating that the account is disabled.
   
   - **Enable an Account:**
     1. To re-enable the account, right-click on the disabled user account.
     2. Select **"Enable Account"** from the context menu.
     3. The downward arrow will be removed, indicating the account is active.
     4. The user can now log in with their credentials.
 <p>
<img width="445" alt="Screenshot 2024-11-20 at 3 35 36 PM" src="https://github.com/user-attachments/assets/894de1b3-f7cc-441a-a04d-b5beb197193f">
<br />
<img width="249" alt="Screenshot 2024-11-20 at 3 36 58 PM" src="https://github.com/user-attachments/assets/a602e535-7b92-4ddb-963b-842c4987c68d">
<br />
<img width="409" alt="Screenshot 2024-11-20 at 3 40 30 PM" src="https://github.com/user-attachments/assets/24d59db5-6957-46c0-b403-878e6829575b">
<br />
<img width="376" alt="Screenshot 2024-11-20 at 3 54 39 PM" src="https://github.com/user-attachments/assets/71864926-b173-4cd1-b383-67dc5c39c570">
<br />
<img width="283" alt="Screenshot 2024-11-20 at 3 58 40 PM" src="https://github.com/user-attachments/assets/da74c018-8f70-4daf-b583-29d3f37e952d">
</p>
