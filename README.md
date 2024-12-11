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

![r1](https://github.com/user-attachments/assets/2be36129-98cd-4400-a4d3-87470aedd9f4)

4. Set the following permissions for each folder (share the folder):
   - **Folder**: “read-access”, **Group**: “Domain Users”, **Permission**: “Read”
   - **Folder**: “write-access”, **Group**: “Domain Users”, **Permission**: “Read/Write”
   - **Folder**: “no-access”, **Group**: “Domain Admins”, **Permission**: “Read/Write”
   - **(Skip accounting for now)**

![r2](https://github.com/user-attachments/assets/442fe0ff-3312-4f64-aced-07433a332f87)

## Attempt to Access File Shares as a Normal User

5. On **Client-1**, navigate to the shared folder (`start`, `run`, `\\dc-1`).
6. Try to access the folders you created. Which folders can you access? Which folders can you create content in? Does this make sense?

![r3](https://github.com/user-attachments/assets/10407c2d-968b-415c-930d-58c62b0feab6)
   
   *Observation*: Logged in as a user (`kax.mib`), I can access both the **read-access** and **write-access** folders because I allowed it from the DC-1 VM as an admin (`jane_admin`). However, I can only create/save/write files in the **write-access** folder and not in **read-access** due to the permissions set.

## Create an “ACCOUNTANTS” Security Group, Assign Permissions, and Test Access

7. Go back to **DC-1**, in **Active Directory**, and create a security group called “ACCOUNTANTS”.

![r4](https://github.com/user-attachments/assets/6bc226a9-3111-48f9-a69b-045df16df9ed)

8. On the **accounting** folder, set the following permissions:
   - **Folder**: “accounting”, **Group**: “ACCOUNTANTS”, **Permission**: “Read/Write”

![r5](https://github.com/user-attachments/assets/973a0c71-f341-48ca-80af-4104a43503bf)

9. On **Client-1**, try to access the **accountants** folder. It should fail.

![r6](https://github.com/user-attachments/assets/1be0886b-5632-42aa-bb05-f89bc156f707)

10. **Log out** of Client-1 as `<someuser>`.
11. On **DC-1**, make the user a member of the “ACCOUNTANTS” security group.

![r7](https://github.com/user-attachments/assets/e7c4b88d-9816-4bad-97d8-e52a81c0a88b)

12. Sign back into **Client-1** as `<someuser>` and try to access the “accounting” share on `\\DC-1`.

![r8](https://github.com/user-attachments/assets/bb0c638e-84c7-4ab4-bdae-c3f5feeea464)
   
---

## Key Takeaways

- **Created Network File Shares**: Set up various network file shares with specific permissions and tested access based on user roles.
- **Configured Share Permissions**: Applied different types of permissions (`Read`, `Read/Write`) to folders and shared them over the network.
- **Tested File Access Control**: Tested the effect of file share permissions by attempting access with a normal user account and verifying what folders could be accessed or written to.
- **Security Group Management**: Created a new security group ("ACCOUNTANTS") and assigned permissions to a folder, then added a user to that group to grant access.
- **Troubleshooting File Access**: Verified the role of security groups in controlling access to shared resources and tested the effect of group membership on permissions.
- **Hands-on Experience**: This project provided hands-on experience with file share configurations, user permissions, and group-based access control in a networked environment.

This lab improved my skills in managing network file shares, understanding user access control, and using security groups to enforce permission policies, which are essential skills for a Cloud Support Engineer.
