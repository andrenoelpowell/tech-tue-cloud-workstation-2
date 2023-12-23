# tech-tue-cloud-workstation-2
This terraform script deploys an Ubuntu Workstation with minimal additional software
installed. An example use case if a temporary sandbox system for surfing potentially
dangerous websites. _NOTE: Don't break the law, as AWS Terms of Service still apply
and this is not exactly covert._

## DEPLOYMENT
Git clone the repository and provision the cloud workstation by running the following
commands, one at a time:

```
git clone  https://github.com/andrenoelpowell/tech-tue-cloud-workstation-2.git
```
and then

```
cd tech-tue-cloud-workstation-2
```

Next modify the `terraform.tfvars` contents with the settings that are appropriate
for your AWS Account.
Need to update with:

Create a new IAM User

Navigate to the IAM Service in the AWS Web Console
Click on Users and then Add user
Create a new user named "tech-tuesday" and click only the "Programmatic acess" checkbox.
Click the blue Next: Permissions button.
Click the Attach existing policies directly button and click the checkbox to the right of the "AdministratorAccess policy." Click the blue Next: Tags button.
Click the blue Create user button.
Copy the Access key ID to a temporary text file (Don't even save this file.)
Click the Show hyperlink to expose the Secret Access key. Copy this to the temporary text file as well. Click the Close button.

Configure the User Profile in the CloudShell

Navigate back to the AWS CloudShell. Click to plant your curser in the Cloudshell terminal.
Enter the command aws configure --profile myterraform into the CloudShell and press enter.
Next paste in the Access key ID from the temporary text file and then the Secret Access Key.
Enter us-east-1 for the Default region name and json for the Default output format.
Test it by running the following command in the terminal: aws sts get-caller-identity
Did you get the results you expected? This command shows the user that you logged into the web console with and not the user that we just created.

Let's run it again, with the profile specified: aws sts get-caller-identity --profile myterraform

Now run these commands:

```
terraform init
terraform apply
```

Obviously, this assumes that you have both `git` and `terraform` installed.

## USAGE
The cloud workstation can be accessed via either SSH or RDP. The necessary information
will be an output when the script is complete.


## ADDITIONAL NOTES
The `workstation1.sh` is passed as user data to the instance and runs the first time
the system boots. The script generates a log entry for almost every command as it runs
and sends it to `/tmp/first-boot.log`

Since the script installs updates and some software, it may take a while. I like to watch
the first boot script by watching its log when connected via SSH. Just run:

```
tail -f /tmp/first-boot.log
```

Otherwise, if you are connected via SSH, you will get a system message when the boot
script has completed:

```
NOTICE: First Boot Setup Has Completed
```

After that point it is ready for an Remote Desktop (RDP) Connection. Use an RDP client,
such as Microsoft Terminal Services Client (MSTSC) to RDP to the IP address that is output
by the Terraform script.

**DON'T FORGET TO CHANGE THE RDP PASSWORD AFTER LOGON**
