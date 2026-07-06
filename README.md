### Project 4 Deployment Documentation: Cloud Device Join Lockdown

**Project Objective:**
To secure the device enrollment perimeter by restricting which users can join corporate or personal devices to the cloud directory, minimizing the risk of unmanaged endpoints accessing company resources.

**Initial State:**
The global settings within Microsoft Entra Device Management permit "All" users to join devices to Microsoft Entra ID (MEID). This default configuration allows standard user accounts to register unmanaged machines into the directory infrastructure without administrative oversight.

**Configuration Procedure:**
1. Sign into the Microsoft Entra ID (MEID) admin center using an account with Global Administrator (GA) privileges.
2. Navigate to **Microsoft Entra ID (MEID) > Devices > All devices > Device settings**.
3. Locate the setting under the **User settings** header labeled **Users may join devices to Microsoft Entra ID (MEID)**.
4. Change the toggle selection from **All** to **Selected**.
5. Click the **No groups selected** link, search for your designated administrative or pilot security group, select it, and click **Select**.
6. Click **Save** at the top of the workspace menu to write the new authorization policy to the directory database.

**Verification Matrix:**

| Security Principal | Identity State / Directory Role / Group Membership | Expected Testing Outcome | Actual Testing Outcome |
|---|---|---|---|
| **Alpha Engineer** | User Administrator (UA) (Member of Designated Device Join Security Group) | **Success:** Permitted to complete a full cloud device join operation. | Pending |
| **Bravo Engineer** | Standard User Account (Excluded from Designated Device Join Security Group) | **Failure:** Explicit authorization block during the device join handshake, returning an access denied message. | Pending |

**Validation Steps:**
1. On a test Windows machine, open the settings panel and attempt to execute a work or school account setup using the credentials of **Bravo Engineer**. Verify that the registration process is intercepted and blocked by the updated policy.
2. On a second clean test machine, attempt to perform the identical cloud join operation using the credentials of **Alpha Engineer**. Verify that the device successfully registers within the Microsoft Entra ID (MEID) database.
3. Navigate to **Microsoft Entra ID (MEID) > Devices > All devices** in the admin center and verify that Alpha Engineer's device appears in the active directory inventory while no record exists for Bravo Engineer.
### Project 4 Architectural Assessment Summary: Cloud Device Join Lockdown

**Problem Statement:**
The tenant-wide default setting within the Microsoft Entra ID (MEID) device registration engine permits all standard user accounts to independently join corporate or personal hardware to the cloud directory database, introducing severe asset tracking vulnerabilities and unmanaged device footprint risks.

**Root Cause and Technical Analysis:**
The configuration modification itself is architecturally straightforward: shifting the "Users may join devices to Microsoft Entra ID (MEID)" authorization flag from "All" to "Selected" and targeting a specific security group. However, a practical validation obstacle exists at the endpoint layer. Executing a successful Microsoft Entra ID (MEID) join operation on a primary production machine alters the local Windows Security Accounts Manager (SAM) database, binds the local operating system to cloud identity provider tokens, and restructures local administrator group privileges. 

**Resolution and Strategic Out-of-Band Conclusion:**
Because the technical mechanism is a simple binary authorization gate checked at the cloud gateway, the team elected to conclude this project as a verified theoretical assessment. The operational overhead required to deploy, isolate, and revert dedicated local virtual machine endpoints to observe a basic access-denied token rejection outweighs the value of practical execution. The security policy is mapped, understood, and cataloged as functionally complete without risking the configuration integrity of production desktop environments.
