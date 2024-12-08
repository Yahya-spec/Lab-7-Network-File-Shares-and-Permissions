<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Network File Shares and Permissions using Active Directory in the Cloud (Azure)</h1>
In this hands-on lab, I will configure multiple network file shares and assign tailored permissions to a user account previously created in earlier labs. This exercise will focus on controlling access to shared resources by defining specific permissions based on the user's role, and ensuring proper security measures are implemented across the network.

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Remote Desktop</li>
  <li>Active Directory Domain Services</li>
  <li>PowerShell</li>
</ul>

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 (21H2)</li>
</ul>

## Create Some Sample File Shares with Various Permissions

1. **Log into DC-1** as your domain admin account (`mydomain.com\jane_admin`).
2. **Log into Client-1** as a normal user (`mydomain\<someuser>`).
3. On **DC-1**, create 4 folders on the `C:\` drive: “read-access”, “write-access”, “no-access”, “accounting”.

   ![image](https://github.com/user-attachments/assets/d9531b1c-9d69-4e52-9f41-a5528532213e)

4. Set the following permissions for each folder (share the folder):
   - **Folder**: “read-access”, **Group**: “Domain Users”, **Permission**: “Read”
   - **Folder**: “write-access”, **Group**: “Domain Users”, **Permission**: “Read/Write”
   - **Folder**: “no-access”, **Group**: “Domain Admins”, **Permission**: “Read/Write”
   - **(Skip accounting for now)**

   ![image](https://github.com/user-attachments/assets/970aaaea-5d3f-45c1-a5a4-9beace95f48c)

## Attempt to Access File Shares as a Normal User

5. On **Client-1**, navigate to the shared folder (`start`, `run`, `\\dc-1`).
6. Try to access the folders you created. Which folders can you access? Which folders can you create content in? Does this make sense?

   ![image](https://github.com/user-attachments/assets/89a1ebd8-8da3-42c8-b67b-5c7de4854c2f)
   
   *Observation*: Logged in as a user (`kax.mib`), I can access both the **read-access** and **write-access** folders because I allowed it from the DC-1 VM as an admin (`jane_admin`). However, I can only create/save/write files in the **write-access** folder and not in **read-access** due to the permissions set.

## Create an “ACCOUNTANTS” Security Group, Assign Permissions, and Test Access

7. Go back to **DC-1**, in **Active Directory**, and create a security group called “ACCOUNTANTS”.

   ![image](https://github.com/user-attachments/assets/00ae3dfc-1614-4dd5-87c8-ef98d933a170)

8. On the **accounting** folder, set the following permissions:
   - **Folder**: “accounting”, **Group**: “ACCOUNTANTS”, **Permission**: “Read/Write”

   ![image](https://github.com/user-attachments/assets/80aabe8d-c4a8-4a3d-8163-f05a215970a2)

9. On **Client-1**, try to access the **accountants** folder. It should fail.

   ![image](https://github.com/user-attachments/assets/5dd2347c-3df5-4557-a107-cfde13cccb98)

10. **Log out** of Client-1 as `<someuser>`.
11. On **DC-1**, make the user a member of the “ACCOUNTANTS” security group.

   ![image](https://github.com/user-attachments/assets/6c2eb1da-def7-4814-89f3-9f33e45fac5b)

12. Sign back into **Client-1** as `<someuser>` and try to access the “accounting” share on `\\DC-1`.

   ![image](https://github.com/user-attachments/assets/802f17f4-8d0b-4ef5-8a2e-a43e05d4c20b)
   
---

## Key Takeaways

- **Created Network File Shares**: Set up various network file shares with specific permissions and tested access based on user roles.
- **Configured Share Permissions**: Applied different types of permissions (`Read`, `Read/Write`) to folders and shared them over the network.
- **Tested File Access Control**: Tested the effect of file share permissions by attempting access with a normal user account and verifying what folders could be accessed or written to.
- **Security Group Management**: Created a new security group ("ACCOUNTANTS") and assigned permissions to a folder, then added a user to that group to grant access.
- **Troubleshooting File Access**: Verified the role of security groups in controlling access to shared resources and tested the effect of group membership on permissions.
- **Hands-on Experience**: This project provided hands-on experience with file share configurations, user permissions, and group-based access control in a networked environment.

This lab improved my skills in managing network file shares, understanding user access control, and using security groups to enforce permission policies, which are essential skills for a Cloud Support Engineer.
