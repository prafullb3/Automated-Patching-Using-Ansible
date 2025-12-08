# Automated-Patching-Using-Ansible

## Introduction

The patching framework is designed to facilitate a seamless patching process with three distinct stages: pre-patch, patch, and post-patch. Each stage is further divided into specific substages to ensure a comprehensive and efficient patching operation. This patching framework supports Linux flavors(RHEL, CentOS and Ubuntu)

## Requirements

1. **SMTP Configuration for Email Notifications:**
   For receiving notifications via email, you need to provide SMTP configuration details. Make sure you have the required information such as SMTP server address, port, and authentication credentials. These details will be used to enable seamless email notifications.

2. **Required credentials**
   1. Machine credentials: Credentials of the server on which patching needs to be performed

## Variables

Below are the manadatory variables for Linux(RHEL, CentOS, and Ubuntu) to run this automation:

|Variable Name | variable type | Supported OS | Description | Default Value |
|--------------|---------------|--------------|-------------|----------------|
|linux_temp_dir_name | String  |  Linux  | Directory name where you want to store evidence | `{{ansible_env.USERPROFILE}}/patching` |
|yum_config:</br>&nbsp;&nbsp;- async:</br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;name:</br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;file:</br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;baseurl:</br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;gpgcheck:</br>| Dictionary | RHEL | Repository URLs for RHEL | ""|
|apt_config:</br>&nbsp;&nbsp;- repo:</br>&nbsp;&nbsp;&nbsp;&nbsp;state:</br>&nbsp;&nbsp;&nbsp;&nbsp;filename: |Dictionary|Ubuntu| Repository URLs for Ubuntu |""|
|details_to_capture_linux:</br>&nbsp;&nbsp;- command:</br>&nbsp;&nbsp;&nbsp;&nbsp;filename:| Dictionary | Linux | Commands to verify the the resources of server, for example available space, current os version|""|
| services_info| List | Linux | Service names to get the info. | "" |
|linux_services_to_restart| List | Linux | List of services which needs to be stopped | ""|
|yum_update| Boolean | RHEL, CentOS |Flag is perform patch installation|`true`|
|exclude_packges|List | RHEL, CentOS | List of packages which needs to be excluded | ""|
|yum_only_install_security_patch| Boolean|RHEL, CentOS| Flag to update only security updates|`true`|
|machine_reboot| Boolean| Linux |Flag to reboot the server| `false`|
|send_email|Boolean| * | Flag to send the notification via mail| `False`|
|smtp_host| String | * | The mail server | ""|
|smtp_port| Integer | * | The mail server port| ""|
|sender_mail_id|String| * | The email-address the mail is sent from | ""|
|receiever_mail_id| String | * |The email-address(es) the mail is being sent to|""|
|mail_subject|String|* |The subject of the email being sent |""|
|mail_body| String  |*|The body of the email being sent|""|
|mail_secure| String |*|Encryption method for the mail being sent for example:`always`,`never`,`starttls`|`never`|
|smtp_username| String | * | SMTP server username | ""|
|smtp_password| String| * | SMTP server password |""|
|perform_cleanup| Boolean| * | To delete all the files and zip files generated during patching|`False`|

## Procedure

To Execute this automation using Ansible Tower, follow below steps:

1. **Setup credentials**

      1. Git credentials</br>
         i. In Ansible Tower, go to the "Credentials" section.</br>
         ii. Click on "+ ADD" to create new credentials.</br>
         iii. Select "Source Control" as the Credential Type.</br>
         iv. Provide a name for the credentials.</br>
         v. Enter the username and password for your Git repository.</br>
         vi. Save the credentials.</br>

      2. Machine Credentials</br>
         i. In the "Credentials" section, click on "+ ADD" again.</br>
         ii. Select the appropriate credential type for machine access (e.g., SSH or Machine).</br>
         iii. Provide a name for the credentials.</br>
         iv. Enter the username and password for the target machine.</br>
         v. Additionally, provide the root username and password if needed.</br>
         vi. Save the credentials.</br>

2. **Create a Project**

      1. Open Ansible Tower and navigate to the "Projects" tab.
      2. Click on the "+ ADD" button to create a new project.
      3. Provide a name for the project and specify the project type (e.g., Git).
      4. Set the URL to your version control system repository (e.g., Git repository URL).
      5. Save the project configuration.

3. **Create an Inventory**

      1. Navigate to the "Inventories" tab in Ansible Tower.
      2. Click on "+ ADD" to create a new inventory.
      3. Enter a name for the inventory and save it.
      4. Add hosts to the inventory, specifying their connection details.

4. **Create Job Template**

      1. Go to the "Templates" tab in Ansible Tower.
      2. Click on "+ ADD" to create a new job template.
      3. Provide a name for the job template.
      4. Select the project created in the step 2.
      5. Choose the playbook for patching.
      6. Link the inventory created in the step 3.
      7. Select the credentials created in earlier step for Git and machine access.
      8. Specify mandatory variables based on the OS from the [variables table](#variables).
      9. Save the job template.

5. **Execute the Job Template**

      1. In the "Templates" tab, find the created Job Template.
      2. Click on the rocket icon or "Launch" to execute the playbook.
      3. Monitor the job progress and review the output for any errors.

## Example Playbook

For sample playbook to use this automation, you can refer to this playbook

[sample playbook](automated_patching.yml)
