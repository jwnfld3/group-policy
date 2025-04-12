# Active Directory Group Management – Windows Server 2022

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
4. Create:
   - `Groups`
   - `Users`
   - `Departments` (optional)

This keeps Active Directory (AD) organized and enables delegation or GPO targeting.

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

---

### 3. Create Users and Add to Groups

> **Definition**: Users inherit access permissions from their group membership, streamlining access control.

1. In `Users` Organizational Unit (OU) > **New > User**:
   - `it.admin`
   - `hr.jane`
   - `finance.john`  
2. Add each user to the matching group (`IT_Group`, etc.)

This supports role-based access control (RBAC).

---

### 4. Create Shared Folders and Set NTFS Permissions

> **Definition**: *New Technology File System (NTFS) Permissions* control file and folder access on Windows volumes.

1. Create folder: `D:\DeptShares`  
2. Inside, create:
   - `IT`
   - `HR`
   - `Finance`  
3. Right-click each folder > **Properties > Security > Edit > Add**  
4. Assign Modify permissions to corresponding groups

This ensures that only appropriate users can access departmental folders.

---

### 5. Test Group-Based Access

> **Definition**: Verifying access ensures the correct permissions are applied through group membership.

1. Log in as `it.admin`  
2. Access `\\SERVERNAME\DeptShares\IT` — should succeed  
3. Access `\\SERVERNAME\DeptShares\Finance` — should be denied

This confirms folder permissions are enforced by group.

---

### 6. (Optional) Map Network Drives via Group Policy

> **Definition**: *Group Policy Preferences (GPP)* can automatically map drives based on user group membership.

1. Open **Group Policy Management Console (GPMC)**  
2. Create Group Policy Object (GPO): `MapDeptDrives`  
3. Go to:  
   `User Configuration > Preferences > Windows Settings > Drive Maps`  
4. Add mapped drive:
   - **Location**: `\\SERVERNAME\DeptShares\IT`
   - **Label**: `IT Share`
   - **Action**: Update  
5. Under **Common > Item-level targeting**, add:
   - **Security Group = IT_Group**

This maps network drives dynamically based on group membership.

---

### 7. (Optional) Clean Up

> **Definition**: Cleaning up removes temporary objects and ensures a tidy environment.

- Delete test users, groups, or Group Policy Objects (GPOs)  
- Remove test folders or permissions  

This maintains a clean and secure Active Directory (AD) environment.

---

## Conclusion

This guide demonstrated how to manage group-based access control in Active Directory (AD) using Windows Server 2022. By organizing users into security groups and assigning New Technology File System (NTFS) permissions accordingly, administrators at **JWNLD** can efficiently manage access to departmental resources while maintaining a secure and scalable environment. This approach improves consistency, simplifies onboarding, and aligns with enterprise security best practices.

