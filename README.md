# Active Directory Group Management on Windows Server 2022

## Summary

This guide walks administrators through the process of creating and managing Active Directory (AD) security groups on Windows Server 2022. It covers best practices for organizing users, assigning access to shared folders, and using New Technology File System (NTFS) permissions to streamline resource access. This structure allows JWNLD to enforce consistent, secure, and scalable access control across departments.

---

## Scenario

The Information Technology (IT) team at **JWNLD** is rolling out access control policies. Each department requires specific access to network shares. To streamline permission management, the administrator will create security groups in Active Directory (AD), assign users to their department's group, and apply NTFS folder permissions based on those groups.

---

## Who, What, When, Where, and Why

- **Who**: Active Directory (AD) administrators managing domain resources and user access  
- **What**: Create Active Directory (AD) groups, assign users, apply folder access control using New Technology File System (NTFS)  
- **When**: During setup of file shares, onboarding, or role-based access control (RBAC) implementations  
- **Where**: Active Directory Users and Computers (ADUC) & Windows File Explorer  
- **Why**: To simplify permissions management and follow security best practices using group-based access  

---

## Requirements

- Windows Server 2022 with Active Directory Domain Services (AD DS) role installed  
- Domain controller configured (e.g., `jwnld.com`)  
- At least 3 test users (`it.admin`, `hr.jane`, `finance.john`)  
- A folder path on a domain-joined server (e.g., `D:\DeptShares`)  

---

## Steps with Definitions and Explanations

### 1. Create Organizational Units (OUs)

> **Definition**: An *Organizational Unit (OU)* is a container in Active Directory (AD) used to logically organize users, groups, and computers. It helps with delegated administration and Group Policy Object (GPO) targeting.

1. Open **Active Directory Users and Computers (ADUC)**  
![image](https://github.com/user-attachments/assets/f2e29af1-b01c-4594-8004-7f58bdc6659b)

2. Navigate to `jwnld.com`  
3. Right-click the domain > **New > Organizational Unit**  
![image](https://github.com/user-attachments/assets/3992e280-91f7-4353-975e-b61aef7e8183)

4. Create:
   - `Groups`
   - `Employees`
   - `Departments` (optional)

This keeps Active Directory (AD) organized and enables delegation or GPO targeting.

![image](https://github.com/user-attachments/assets/2dc990ce-db4f-48e5-8a7f-c91de8b394a8)
---

### 2. Create Security Groups

> **Definition**: A *Security Group* is used to assign permissions to shared resources such as folders, printers, and applications.

1. In the `Groups` Organizational Unit (OU) > **New > Group**  
2. Create:
   - `IT_Group`
   - `HR_Group`
   - `Finance_Group`  
3. Set:
   - **Scope**: Global  
   - **Type**: Security  

Security groups simplify access control across domain resources.
![image](https://github.com/user-attachments/assets/0d8ffe41-36c1-4a5e-95c1-4a6878be928f)
![image](https://github.com/user-attachments/assets/39c33d80-9af6-40b4-b550-e45d48ee9917)

---

### 3. Create Users and Add to Groups

> **Definition**: Users inherit access permissions from their group membership, streamlining access control.

1. In `Employees` Organizational Unit (OU) > **New > User**:
   - `it.admin`
   - `hr.jane`
   - `finance.john`  

![image](https://github.com/user-attachments/assets/1941b051-2c8c-4d42-b844-eac567fe1537)
![image](https://github.com/user-attachments/assets/edc59540-f56c-49ae-b95a-a39d5ceebaf3)
![image](https://github.com/user-attachments/assets/c426e35a-0c7f-46ff-861c-1bb4886af84b)

2. Add each user to the matching group (`IT_Group`, etc.)

![image](https://github.com/user-attachments/assets/d74a66e2-e06e-4616-ac87-7f170cebdade)
![image](https://github.com/user-attachments/assets/709994a9-5205-4d8b-8b9d-10cf2357cf0d)
![image](https://github.com/user-attachments/assets/c3845f7c-2115-4bd3-ad59-96efea139ede)
![image](https://github.com/user-attachments/assets/7fad9769-1caa-447b-9e0c-b36519ed1da9)
![image](https://github.com/user-attachments/assets/b5731a69-2941-4b8f-bbca-7fa76e5d3557)
![image](https://github.com/user-attachments/assets/d86ee4aa-56d7-444b-89c7-7c71301478e9)



This supports role-based access control (RBAC).

For the purpose of this guide a password change will not be enforced since this is not a live enviornment. If this was a live enviornment it is strongly recommended to force the user to change their password. Forcing a password change at first login closes a critical security gap by ensuring users take control of their credentials and that temporary/shared passwords aren’t used longer than necessary.

---

### 4. Create Shared Folders and Set NTFS Permissions

> **Definition**: *New Technology File System (NTFS) Permissions* control file and folder access on Windows volumes.

1. Create folder: `C:\DeptShares`  
2. Inside, create:
   - `IT`
   - `HR`
   - `Finance`  
![image](https://github.com/user-attachments/assets/65b6cfe4-1504-4278-bf70-645df8352620)
![image](https://github.com/user-attachments/assets/cd8a341f-547b-458a-b8ae-411db072165d)

3. Right-click each folder > **Properties > Security > Edit > Add**  
4. Assign Modify permissions to corresponding groups

This ensures that only appropriate users can access departmental folders.

![image](https://github.com/user-attachments/assets/f61815b1-2b72-482f-b789-95715b0dd2a6)
![image](https://github.com/user-attachments/assets/2880f9de-47d9-4a1c-ad41-6fca1fe84797)
![image](https://github.com/user-attachments/assets/b05957fa-d615-4fb5-924a-1635367681c3)
![image](https://github.com/user-attachments/assets/92ccab65-bf39-492c-b414-fb9082c5cf74)
![image](https://github.com/user-attachments/assets/0f087388-7958-4942-87e1-3054103a9f39)
![image](https://github.com/user-attachments/assets/334e5b4b-3a80-4b44-bddc-95db99e27f30)
![image](https://github.com/user-attachments/assets/51e137b2-007c-4af7-9f5a-d450d6ae5174)
![image](https://github.com/user-attachments/assets/2ae91fd1-3734-4e4f-b8d6-26ffc41b2928)

---

### 5. Configure Network Sharing for the Folder with the Appropriate User or Group

> **Definition**: Network sharing allows folders on a server to be accessible over the network by other users or devices. When a folder is shared, it becomes visible and accessible from remote systems, provided the necessary permissions are granted.

1. Log in to the server.
2. Navigate to the folder you want to share (e.g., `D:\DeptShares\IT`)
3. Right-click the folder and select **Properties**
![image](https://github.com/user-attachments/assets/c726f229-5375-43a8-b922-bb115de1f0aa)

4. Go to the **Sharing** tab
5. Click **Advanced Sharing**
![image](https://github.com/user-attachments/assets/b7bb3f63-714a-410b-870c-4c2ca72dbbac)

6. Check the box for **"Share this folder"**
7. Set the **Share Name** (e.g., `IT`)
8. Click **Permissions** and:
   - Add `IT_Group`
   - Set the correct permission level (typically **Read** or **Full Control**)
![image](https://github.com/user-attachments/assets/bdcca81b-c2ea-4db3-a556-db177067b641)
![image](https://github.com/user-attachments/assets/160d16eb-287a-4452-9423-9f0a1dff9634)
![image](https://github.com/user-attachments/assets/7412f363-c41d-4a0a-81d1-dd898fe218fe)
![image](https://github.com/user-attachments/assets/a1ea7a79-0ce5-4d95-8841-11840e3634f4)


9. Click **OK** to close all windows

---

### 6. Test Group-Based Access

> **Definition**: Verifying access ensures the correct permissions are applied through group membership.

1. Log in as `it.admin`
2. Go to `File Explorer`
3. Access The on-prem server by clicking on `Network`
4. Access `\\SERVERNAME\IT` will be visible because `it.admin` was granted access.

This confirms folder permissions are enforced by group.
![image](https://github.com/user-attachments/assets/8248e74d-3bb8-42bc-af4e-373a052dbc7f)
![image](https://github.com/user-attachments/assets/c498b2f6-a53f-4ffb-8073-3fed15fa9f72)

---

## Conclusion

This guide demonstrated how to manage group-based access control in Active Directory (AD) using Windows Server 2022. By organizing users into security groups accordingly, administrators at **JWNLD** can efficiently manage access to departmental resources while maintaining a secure and scalable environment. This approach improves consistency, simplifies onboarding, and aligns with enterprise security best practices.

