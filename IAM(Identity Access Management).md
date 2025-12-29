AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. IAM provides the infrastructure necessary to control authentication and authorization for your AWS accounts.

![[Pasted image 20251217111049.png]]


-> For owner account have a full permission
-> for IAM user have zero permission
## how to create a IAM :
go to user  and create user 
![[WhatsApp Image 2025-12-17 at 11.17.50.jpeg]]


set permission 

![[Pasted image 20251217111943.png]]

using this create the user.

### Creating and attaching policies to users, groups, and roles:

Creating and attaching policies to users, groups, and roles in AWS is pretty straightforward.
#### Creating a policy:

- To create a policy, click “Policies” in the IAM console left-hand menu and click “Create Policy.”
- You can define your policy using the visual editor or the JSON tab. The visual editor is easier if you’re not familiar with JSON. For example, to create a policy that allows read-only access to S3, you can select the “S3 service” and then choose the “read-only” actions.
- Once you’ve defined the policy, click “Review policy.” Then, give your policy a name and description and click “Create policy.” At this point, your custom policy should be created.
![[Pasted image 20251217112237.png]]

#### Attaching policies to users

- If you want to attach a policy to a user, go to “Users” in the IAM console and select the user to whom you want to attach the policy. 
- Click on the “Permissions” tab and then click “Add permissions.”

Select “Attach policies directly,” find the policy you created (or any existing policy you want to attach) and choose it.

![[Pasted image 20251217112830.png]]

- Go to “Roles” in the IAM console and select the role to which you want to attach a policy.
- Click on the “Permissions” tab and then click ”Add permissions.” This step is slightly different from what we’ve encountered previously. You’re shown the current permission policies and the complete list of other policies you can attach.


