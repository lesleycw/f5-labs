Agility 2018 Hands-on Lab Guide

Federation with F5

APM 330 Presented by: Steve Lyons, Chris Miller & Chas Lesley

What’s inside

`Welcome <#welcome>`__

`Lab Network Setup <#lab-network-setup>`__

`Timing for labs <#timing-for-labs>`__

`Authentication – Credentials <#authentication-credentials>`__

`Utilized Browsers <#utilized-browsers>`__

`General Notes <#general-notes>`__

`Acknowledgements <#acknowledgements>`__

`Lab 1 – SAML Service Provider (SP) Lab <#lab-1-saml-service-provider-lab>`__

`Lab 2 – SaaS SAML Identity Provider (IdP) Lab - OKTA <#lab-2-saas-saml-identity-provider-lab-with-okta>`__

`Lab 3 – oAuth and OpenID Connect Lab - Google <#lab-3-oauth-and-openid-connect-lab-with-google>`__

`Lab 4 – oAuth and Azure AD Lab <#lab-4-oauth-and-azuread-lab>`__

`Conclusion <#conclusion>`__

`Appendix <#appendix>`__

`Learn More <#learn-more>`__

Welcome
=======

Welcome to the 330 Access Policy Manager (APM) Federation Lab. The
following labs and exercises will instruct you on how to configure and
troubleshoot federation use cases based on the experience of field
engineers, support engineers and clients. This guide is intended to
complement lecture material provided during the 330 course as well as a
reference guide that can be referred to after the class as a basis for
configuring federation relationships in your own environment.

Lab Network Setup
-----------------

In the interest of focusing as much time as possible configuring and
performing lab tasks, we have provided some resources and basic setup
ahead of time. These are:

-  Cloud-based lab environment complete with Jump Host, Virtual BIG-IP
   and Lab Server

-  Duplicate Lab environments for each student for improved
   collaboration

-  The Virtual BIG-IP has been pre-licensed and provisioned with Access
   Policy Manager (APM)

-  Pre-staged configurations to speed up lab time, reducing repetitive
   tasks to focus on key learning elements.

If you wish to replicate these labs in your environment you will need to
perform these steps accordingly. Additional lab resources are provided
as illustrated in the diagram below:

|image0|

Timing for labs
---------------

The time it takes to perform each lab varies and is mostly dependent on
accurately completing steps. This can never be accurately predicted but
we strived to provide an estimate based on several people, each having a
different level of experience. Below is an estimate of how long it will
take for each lab:

+-----------------------------------------------------+----------------------+
| **Lab Description**                                 | **Time Allocated**   |
+=====================================================+======================+
| LAB 1 - SAML Service Provider (SP)                  | 25 minutes           |
+-----------------------------------------------------+----------------------+
| LAB 2 - SaaS SAML Identity Provider (IDP) (OKTA)    | 25 minutes           |
+-----------------------------------------------------+----------------------+
| LAB 3 - oAuth & OpenID Connect (Google)             | 25 minutes           |
+-----------------------------------------------------+----------------------+
| LAB 4 - oAuth and Azure AD Lab                      | 25 minutes           |
+-----------------------------------------------------+----------------------+

Authentication – Credentials
----------------------------

The following credentials will be utilized throughout this Lab guide.
All other credentials will be indicated at the time of use.

+--------------------------------------+---------------+----------------+
| **Credential Use**                   | **User ID**   | **Password**   |
+======================================+===============+================+
| BIG-IP Configuration Utility (GUI)   | admin         | admin          |
+--------------------------------------+---------------+----------------+
| BIG-IP CLI Access (SSH)              | root          | default        |
+--------------------------------------+---------------+----------------+
| Jump Host Access                     | f5student     | f5DEMOs4u      |
+--------------------------------------+---------------+----------------+

Utilized Browsers
-----------------

The preferred browser for this lab is Firefox. Shortcut links have been
provided to speed access to targeted resources and assist you in your
tasks. Except where noted, either browser can be used for all lab tasks.

General Notes
-------------

As noted previously, environment staging has been done to speed up lab
time, reducing repetitive tasks to focus on key learning elements. Where
possible steps that have been optimized have been called out with links
and references provided in the *Additional Information* section for
additional clarification. The intention being that the lab guide truly
serves as a resource guide for all your future federation deployments.

Acknowledgements 
-----------------

This lab is built upon the work of prior F5 Agility’s and the work of
many individuals behind the scenes in addition the 2018 Agility Lab
Team. Many thanks to the 2017 Agility Lab Team for the SAML & OAuth
Federation Labs, Lucas Thompson for his OAuth/OIDC Lab and our lab
testers Matt Harmon, Dave Lipowsky & Stu McMath.

Lab 1 SAML Service Provider Lab
======================================

The purpose of this lab is to configure and test a SAML Service
Provider (SP). Students will configure the various aspects of a SAML Service
Provider, import and bind to a SAML Identity Provider (iDP) and test
SP-Initiated SAML Federation.

Objective:
----------

-  Gain an understanding of SAML Service Provider(SP) configurations and
   its component parts

-  Gain an understanding of the access flow for SP-Initiated SAML

Lab Requirements:
-----------------

-  All Lab requirements will be noted in the tasks that follow

-  Estimated completion time: 25 minutes

Lab 1 Tasks:
-----------------

TASK 1 – Configure the SAML Service Provider (SP) 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Login to your lab provided **Virtual Edition BIG-IP**                                     |
|                                                                                              |
| 2. Begin by selecting: **Access Federation -> SAML Service Provider** ->                     |
|                                                                                              |
|    **Local SP Services**                                                                     |
|                                                                                              |
| 3. Click the **Create** button (far right)                                                   |
+----------------------------------------------------------------------------------------------+
| |image001|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. In the **Create New SAML SP Service**  dialogue box click **General Settings** in         |
|                                                                                              |
|    the left navigation pane and key in the following as shown:                               | 
|                                                                                              |
|    -  **Name**: **app.f5demo.com**                                                           | 
|                                                                                              |
|    -  **Entity ID**: **https://app.f5demo.com**                                              |
|                                                                                              |
|    *Note: The yellow box on Host will disappear when the Entity ID is entered.*              |
+----------------------------------------------------------------------------------------------+
| |image002|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. Click on the **Security Settings** in the left navigation menu.                           |
|                                                                                              |
| 6. Check the **Sign Authentication Request** checkbox                                        |
|                                                                                              |
| 7. Select **/Common/SAML.key** from drop down menu for the                                   |
|    **Message Signing Private Key**                                                           |
|                                                                                              |
| 8. Select **/Common/SAML.crt** from drop down menu for the                                   |
|    **Message Signing Certificate**                                                           |
|                                                                                              |
| 9. Click **OK** on the dialogue box                                                          |
+----------------------------------------------------------------------------------------------+
| |image003|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 2 – Configure the External SAML IDP Connector 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Click on the **Access** -> **Federation** -> **SAML Service Provider** ->                 |
|                                                                                              |  
|    **External IdP Connectors** or click on the **SAML Service Provider** tab in the         | 
|                                                                                              |
|    horizontal navigation menu andselect **External IdP Connectors**.                         |
|                                                                                              |
| 2. Click specifically on the **Down Arrow** next to the **Create** button (far right)        |
|                                                                                              |
| 3. Select **From Metadata** from the drop down menu                                          |
+----------------------------------------------------------------------------------------------+
| |image004|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+   
| 4. In the **Create New SAML IdP Connector** dialogue box, click **Browse** and select        |
|                                                                                              |
|    the **idp.partner.com-app\_metadata.xml** file from the Desktop of your jump host.        |
|                                                                                              |
| 5. In the **Identity Provider Name** field enter the following: **idp.partner.com**          | 
|                                                                                              |
| 6. Click **OK** on the dialogue box.                                                         |
|                                                                                              |
| *Note: The idp.partner.com-app\_metadata.xml was created previously. Oftentimes,*            |
|                                                                                              |
| *IdP providers will have a metadata file representing their IdP service. This can be*        | 
|                                                                                              |
| *imported to save object creation time as it has been done in this lab*                      |
+----------------------------------------------------------------------------------------------+
| |image005|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 3 – Bind the External SAML IDP Connector to the SAML SP 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Click on the **Local SP Services** from the **SAML Service Provider** tab in the          |
|                                                                                              |
|    horizontal navigation menu.                                                               |
|                                                                                              |
| 2. Click the **Checkbox** next to the previously created **app.f5demo.com** and select       |
|                                                                                              |
|    **Bind/Unbind IdP Connectors** button at the bottom of the GUI.                           | 
+----------------------------------------------------------------------------------------------+
| |image006|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. In the **Edit SAML IdP’s that use this SP** dialogue box click the **Add New Row** button |
|                                                                                              |
| 4. In the added row click the **Down Arrow** under **SAML IdP Connectors** and select the    |
|                                                                                              |
|    **/Common/idp.partner.com** SAML IdP Connector previously created.                        |
|                                                                                              |
| 5. Click the **Update** button and the **OK** button at the bottom of the dialogue box.      |
+----------------------------------------------------------------------------------------------+
| |image007|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. Under the **Access** -> **Federation** -> **SAML Service Provider** ->                    |
|                                                                                              |
|    **Local SP Services** menu you should now see the following (as shown):                   |
|                                                                                              |
|    -  **Name**: **app.f5demo.com**                                                           |
|                                                                                              |
|    -  **SAML IdP Connectors**: **idp.partner.com**                                           |
+----------------------------------------------------------------------------------------------+
| |image008|                                                                                   |
+----------------------------------------------------------------------------------------------+
 
TASK 4 – Configure the SAML SP Access Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Begin by selecting: **Access** -> **Profiles/Policies** -> **Access Profiles**            |
|    **(Per-Session Policies)**                                                                |
|                                                                                              |
| 2. Click the **Create** button (far right)                                                   |
+----------------------------------------------------------------------------------------------+
| |image009|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. In the **New Profile** window, key in the following as shown:                             |
|                                                                                              |
|    -  **Name**: **app.f5demo.com-policy**                                                    |
|                                                                                              |
|    -  **Profile Type**: **All** (from drop down)                                             |
|                                                                                              |
|    -  **Profile Scope**: **Profile** (default)                                               |
|                                                                                              |
| 4. Scroll to the bottom of the **New Profile** window to the **Language Settings**           |
|                                                                                              |
| 5. Select **English** from the **Factory Built-in Languages** menu on the right and click    |
|                                                                                              |
|    the **Double Arrow (<<)**, then click the **Finished** button.                            |
+----------------------------------------------------------------------------------------------+
| |image010|                                                                                   |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| 6. From the **Access** -> **Profiles/Policies** -> **Access Profiles**                       |
|    **(Per-Session Policies)**,                                                               |
|                                                                                              |
|    click the **Edit** link on the previously created **app.f5demo.com-policy** line.         |
+----------------------------------------------------------------------------------------------+
| |image011|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. In the **Visual Policy Editor** window for the **/Common/app.f5demo.com-policy**, click   |
|                                                                                              |
|    the **Plus (+) Sign** between **Start** and **Deny**.                                     |
|                                                                                              |
| 8. In the pop-up dialogue box select the **Authentication** tab and then click the **Radio** |
|                                                                                              | 
|    **Button** next to **SAML Auth**. Once selected click the **Add Item** button.            |
+----------------------------------------------------------------------------------------------+
| |image012|                                                                                   |
|                                                                                              |
| |image013|                                                                                   |
+----------------------------------------------------------------------------------------------+
  
+----------------------------------------------------------------------------------------------+
| 9. In the **SAML Auth** configuration window, select **/Common/app.f5demo.com** from the     |
|                                                                                              |
|    **SAML Authentication**, **AAA Server** drop down menu.                                   |
|                                                                                              | 
| 10. Click the **Save** button at the bottom of the configuration window.                     |  
+----------------------------------------------------------------------------------------------+
| |image014|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 11. In the **Visual Policy Editor** select the **Deny** along the **Successful** branch      |
|                                                                                              |
|    following the **SAML Auth**                                                               |
|                                                                                              |
| 12. From the **Select Ending** dialogue box select the **Allow Radio Button** and then       |
|                                                                                              |
|    click **Save**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image015|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 13. In the **Visual Policy Editor** click the **Apply Access Policy** (top left) and close   |
|                                                                                              |
|    the **Visual Policy Editor**.                                                             |
|                                                                                              |
| *Note: Additional actions can be taken in the Per Session policy (Access Policy). The lab*   |
|                                                                                              |
| *is simply completing authentication. Other access controls can be implemented based on the* |
|                                                                                              |
| *use case*                                                                                   |
+----------------------------------------------------------------------------------------------+
| |image016|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 5 – Create the SP Virtual Server & Apply the SP Access Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Begin by selecting: **Local Traffic** -> **Virtual Servers**                              |
|                                                                                              |
| 2. Click the **Create** button (far right)                                                   |   
+----------------------------------------------------------------------------------------------+
| |image017|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. In the **New Virtual Server** window, key in the following as shown:                      |
|                                                                                              |
|    -  **Name**: **app.f5demo.com**                                                           |
|                                                                                              |
|    -  **Destination Address/Mask**: **10.1.10.100**                                          |
|                                                                                              |
|    -  **Service Port**: **443**                                                              |
|                                                                                              |
|    -  **HTTP Profile:** **http** (drop down)                                                 |
|                                                                                              |
|    -  **SSL Profile (client):** **app.f5demo.com-clientssl**                                 |
|                                                                                              |
|    -  **Source Address Translation:**  **Auto Map**                                          |
|                                                                                              |
| 4. Scroll to the **Access Policy** section                                                   |
|                                                                                              |
|    -  **Access Profile**: **app.f5demo.com-policy**                                          |
|                                                                                              |
|    -  **Per-Request Policy:** **saml\_policy**                                               |
|                                                                                              |
| 5. Scroll to the Resource section                                                            |
|                                                                                              |
|    -  **Default Pool**: **app.f5demo.com\_pool**                                             |
|                                                                                              |
| 6. Scroll to the bottom of the configuration window and click **Finished**                   |
|                                                                                              |
| *Note: The use of the Per-Request Policy is to provide header injection and other controls.* |
|                                                                                              |
| *These will be more utilized later in the lab.*                                              |
+----------------------------------------------------------------------------------------------+
| |image018|                                                                                   |
|                                                                                              |
| |image019|                                                                                   | 
+----------------------------------------------------------------------------------------------+

TASK 6 – Test the SAML SP
~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Using your browser from the Jump Host click on the provided bookmark or navigate to       |
|                                                                                              |
|    https://app.f5demo.com . The SAML SP that you have just configured.                       |
+----------------------------------------------------------------------------------------------+
| |image020|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Did you successfully redirect to the IdP?                                                 |
|                                                                                              |
| 3. Login to the iDP, were you successfully authenticated? (use credentials provided in the   |
|                                                                                              |
|    Authentication Information section at the beginning of this guide)                        |
|                                                                                              |
|    -  **Username**: **user**                                                                 |
|                                                                                              |
|    -  **Password**: **Agility1**                                                             |
|                                                                                              |
| 4. After successful authentication, were you returned to the SAML SP?                        |
|                                                                                              |
| 5. Were you successfully authenticated (SAML)?                                               |
|                                                                                              |
| 6. Review your **Active Sessions** (**Access Overview** -> **Active Sessions**)              |
|                                                                                              |
| 7. Review your Access Report Logs (**Access** -> **Overview Access Reports**)                |
+----------------------------------------------------------------------------------------------+
| |image021|                                                                                   |
+----------------------------------------------------------------------------------------------+

Lab 2 SaaS SAML Identity Provider Lab with OKTA
====================================================

The purpose of this lab is to configure and test a SaaS SAML Identity
Provider. Students will configure a SaaS based SAML Identity Provider
(in this case OKTA) and import and bind to a SAML Service Provider and
test IdP-Initiated and SP-Initiated SAML Federation.

Objective:
----------

-  Gain an understanding of integrating a SaaS SAML Identity
   Provider(IdP)

-  Gain an understanding of the access flow for IdP-Initiated SAML

Lab Requirements:
-----------------

-  All Lab requirements will be noted in the tasks that follow

-  Estimated completion time: 25 minutes

Lab 2 Tasks:
------------

TASK 1 – Sign Up for OKTA Developer Account 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| *Note: The following steps provide instruction for setting up an OKTA developer account. If* |
|                                                                                              |
| *you already have one, you may elect to use that account. Understand, however, that the*     |
|                                                                                              |
| *instructions below may need to be modified to match your environment.*                      |
|                                                                                              |
| 1. Sign Up for an OKTA developer account by navigating to:                                   |
|                                                                                              |
|  **https://developer.okta.com/signup/** and using a VALID email and click **Get Started**    |
|                                                                                              |
| 2. Additional instructions will be sent to the email address provided.                       |     
+----------------------------------------------------------------------------------------------+
| |image26|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Following the instructions received from the generated email, sign on to the OKTA         |
|                                                                                              |
|    development environment with your provided, temporary password.                           |
+----------------------------------------------------------------------------------------------+
| |image27|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Enter a **New Password** and the **Repeat New Password**                                  |
|                                                                                              |
| 5. Use the drop down to select a **Forgot Password Question** and provide the Answer         |
|                                                                                              |
| 6. Click a **Security Image**                                                                |
|                                                                                              |
| 7. Click **Create My Account**                                                               |
+----------------------------------------------------------------------------------------------+
| |image28|                                                                                    |
+----------------------------------------------------------------------------------------------+
 
TASK 2– OKTA Classic UI 
~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. For the purposes of the lab and SAML development, we will be using the OKTA Classic UI    |
|                                                                                              |
|   which provides access to SAML configurations. *(Note: At lab publication, the Developer*   |
|                                                                                              |
|   *Console did not have SAML resources.)*                                                    |
|                                                                                              |     
| 2. In the top, left hand corner click the **<>** & select **Classic UI** from the drop down. |
+----------------------------------------------------------------------------------------------+
| |image29|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 3 – Enable OKTA Multi-Factor Authentication [OPTIONAL]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| *Note: Enabling MFA will require a Smart Device with the appropriate OKTA client for your OS*|
|                                                                                              |
| *The step can be skipped if you prefer to just use UserID/Password*                          |
|                                                                                              |
| 1. Click **Security** from the top navigation, then click **Multifactor**                    |
+----------------------------------------------------------------------------------------------+
| |image30|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Check **OKTA Verify**                                                                     |
|                                                                                              |
| 3. Ensure that **Enable Push Verification** & (optionally) that                              |
|                                                                                              |
|    **Require TouchID for OKTA Verify** is checked.                                           |
|                                                                                              |
| 4. Click **Save**                                                                            |
+----------------------------------------------------------------------------------------------+
| |image31|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 4 – Build SAML Application - OKTA 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. In the main menu, click **Applications** in the top navigation.                           |
+----------------------------------------------------------------------------------------------+
| |image32|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Click **Create New App** in the **Add Application Menu**                                  |
+----------------------------------------------------------------------------------------------+
| |image33|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. In the **Create a New Application Integration** dialogue box, select **Web** from the     |
|                                                                                              |
|    drop down for **Platform**.                                                               |
|                                                                                              |
| 4. Select the **SAML 2.0** radio button for **Sign on Method** and click **Create**.         |
+----------------------------------------------------------------------------------------------+
| |image34|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. In the **Create SAML Integration** screen, enter **app.f5demo.com** for the **App Name**. |
|                                                                                              |
| 6. Leave all other values show and click **Next**.                                           |
+----------------------------------------------------------------------------------------------+
| |image35|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. In the **Edit SAML Integration** screen, enter the following values                       |
|                                                                                              |
| 8. In the **SAML Setting** section                                                           |
|                                                                                              |
|   -  **Single Sign on URL:** **https://app.f5demo.com/saml/sp/profile/post/acs**             |
|                                                                                              |
|   -  **Audience URI (SP Entity ID):** **https://app.f5demo.com**                             |
|                                                                                              |
| 9. Leave all other values as default and click **Next**.                                     |
+----------------------------------------------------------------------------------------------+
| |image36|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 10. In the **Create SAML Integration** screen, select the:                                   |
|                                                                                              |
|    **“I’m an OKTA customer adding an internal app”** radio button for                        |
|                                                                                              |
|    **Are you a customer or partner?**                                                        |
|                                                                                              |
| 11. In the resulting expanded window, select:                                                |
|                                                                                              |
|    **“This is an internal app that we have created”** for **App Type**                       |
|                                                                                              |
|    and click **Finish**.                                                                     |
+----------------------------------------------------------------------------------------------+
| |image37|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 12. In the resulting application screen for **app.f5demo.com**, navigate to the              |
|                                                                                              |
|    **SAML 2.0 section**.                                                                     |
|                                                                                              |
| 13. Right Click the **Identity Provider Metadata** hyperlink and click **Save Link As …**    |
|                                                                                              |
| 14. Save the **metadata.xml** to your jumphost desktop. We will be using it in a later step  |
|                                                                                              |
|     in the Lab.                                                                              |
+----------------------------------------------------------------------------------------------+
| |image38|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 5 – Add User to SAML Application 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Within the **app.f5demo.com** application screen, Click **Assignments** then **Assign**   |
+----------------------------------------------------------------------------------------------+
| |image39|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. In the **Assign app.f5demo.com to People** dialogue box, select your **User ID** and then |
|                                                                                              |
|    click **Done**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image40|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 6 – Add Multi-Factor Authentication Sign-On Policy [OPTIONAL]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| **[OPTIONAL]**                                                                               |
|                                                                                              |
| 1. Within the **app.f5demo.com** application screen, Click **Sign On**                       |
+----------------------------------------------------------------------------------------------+
| |image41|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[OPTIONAL]**                                                                               |
|                                                                                              |
| 2. Scroll down to the **Sign On Policy** section and click **Add Rule**                      |
+----------------------------------------------------------------------------------------------+
| |image42|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[OPTIONAL]**                                                                               |
|                                                                                              | 
| 3. In the **Add Sign On Rule** dialogue box, enter **MFA** for the **Rule Name**.            |
|                                                                                              |
| 4. Scroll down to the **Actions** section.                                                   |
+----------------------------------------------------------------------------------------------+
| |image43|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[OPTIONAL]**                                                                               |
|                                                                                              |
| 5. In the **Actions** section, under **Access**, check the box for **Prompt for factor**.    |
|                                                                                              |
| 6. Ensure **Every Sign On** radio button is selected.                                        |
|                                                                                              |
| 7. Click **Save**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image44|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[OPTIONAL]**                                                                               |
|                                                                                              |
| 8. Review and verify the completed **Sign On Policy**.                                       |
+----------------------------------------------------------------------------------------------+
| |image45|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 7 – Create the External IDP Connector
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Login to your lab provided **Virtual Edition BIG-IP**                                     |
|                                                                                              |
| 2. Begin by selecting: **Access** -> **Federation** -> **SAML Service Provider** ->          |
|                                                                                              |
|    **External IdP Connectors**.                                                              |
+----------------------------------------------------------------------------------------------+
| |image46|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. In the **External IdP Connectors** screen, click the **downward arrow** next to the word  |
|                                                                                              |
|    **Create** on the **Create** button (right side)                                          |
|                                                                                              |
| 4. Select **From Metadata** from the drop down menu                                          |
+----------------------------------------------------------------------------------------------+
| |image47|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. In the **Create New SAML IdP Connector** dialogue box, use the **Browse** button to       |
|                                                                                              |
|    select the **metadata.xml** from the desktop (created in Task 4).                         | 
|                                                                                              |
| 6. Name the **Identity Provider Name**: **OKTA\_SaaS-iDP**.                                  |
|                                                                                              |
| 7. Click **OK**.                                                                             |
+----------------------------------------------------------------------------------------------+
| |image48|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 8 – Change the SAML SP Binding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Begin by selecting: **Access** -> **Federation** -> **SAML Service Provider** ->          |
|                                                                                              |
|    **External IdP Connectors**                                                               |
|                                                                                              |
| 2. Select the checkbox next to **app.f5demo.com** and click **Bind\\UnBind IdP Connectors**  |
+----------------------------------------------------------------------------------------------+
| |image49|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Delete/Remove the existing binding                                                        |
|                                                                                              |
| 4. Click **Add New Row** and use the following values                                        |
|                                                                                              |
|    -  **SAML IdP Connectors:** **/Common/OKTA\_SaaS-iDP**                                    |
|                                                                                              |
|    -  **Matching Source:** **%{session.server.landinguri}**                                  |
|                                                                                              |
|    -  **Matching Value:** /*                                                                 |
|                                                                                              |
| 5. Click **Update** then **OK**.                                                             |
+----------------------------------------------------------------------------------------------+
| |image50|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 9 – Apply Access Policy Changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Click the **Apply Access Policy** link in the top left corner of the Admin GUI            |
+----------------------------------------------------------------------------------------------+
| |image51|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Ensure **app.f5demo.com-policy** is checked and click **Apply**                           |
+----------------------------------------------------------------------------------------------+
| |image52|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 10 – Test Access to the app.f5demo.com application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| *Note: Those who enabled MFA access will be required to activate their second factor for*    |
|                                                                                              |
| *application access.*                                                                        |
|                                                                                              |
| 1. Follow the necessary prompts as directed.                                                 |
+----------------------------------------------------------------------------------------------+
| |image53|                                                                                    |
| |image54|                                                                                    |
| |image55|                                                                                    |'
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Using your browser from the Jump Host click on the provided bookmark or navigate to:      |
|                                                                                              |
|    https://app.f5demo.com                                                                    |
+----------------------------------------------------------------------------------------------+
| |image56|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Did you successfully redirect to the OKTA SaaS IdP?                                       |
|                                                                                              |
| 4. Login to the iDP, were you successfully authenticated? Were you prompted for MFA          |
|                                                                                              |
|    if configured?                                                                            |
|                                                                                              |
| 5. After successful authentication, were you returned to the SAML SP?                        |
|                                                                                              |
| 6. Were you successfully authenticated (SAML)?                                               |
|                                                                                              |
| 7. Review your **Active Sessions** (**Access Overview** -> **Active Sessions**).             |
|                                                                                              |
| 8. Review your Access Report Logs (**Access Overview** -> **Access Reports**).               |
+----------------------------------------------------------------------------------------------+
| |image57|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. Destroy your Active Session by nagivating to **Access Overview** -> **Active Sessions**   |
|                                                                                              |
|    Select the checkbox next to your session and click the **Kill Selected Session** button.  |
+----------------------------------------------------------------------------------------------+
| |image58|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 10. Close your browser and logon to your **https://dev-<Dev-ID>.oktapreview.com** account.   |
|                                                                                              |
|    Click on your **app.f5demo.com** application for IDP initiated Access.                    |
|                                                                                              |
| 11. After successful authentication, were you returned to the SAML SP?                       |
|                                                                                              |
| 12. Were you successfully authenticated (SAML)?                                              |
|                                                                                              |
| 13. Review your **Active Sessions** (**Access Overview** -> **Active Sessions**).            |
|                                                                                              |
| 14. Review your Access Report Logs (**Access Overview** -> **Access Reports**).              |
+----------------------------------------------------------------------------------------------+
| |image59|                                                                                    |
+----------------------------------------------------------------------------------------------+

Lab 3 oAuth and OpenID Connect Lab with Google
==============================================

The purpose of this lab is to better understand the F5 use cases OAuth2
and OpenID Connect by deploying a lab based on a popular 3rd party
login: Google. Google supports OpenID Connect with OAuth2 and JSON Web
Tokens. This allows a user to securely log in, or to provide a secondary
authentication factor to log in. Archive files are available for the
completed Lab 2.

Objective:
----------

-  Gain a better understanding of the F5 use cases OAuth2 and OpenID
   Connect.

-  Develop an awareness of the different deployment models that OAuth2,
   OpenID Connect and JSON Web Tokens (JWT) open up

Lab Requirements:
-----------------

-  All Lab requirements will be noted in the tasks that follow

-  Estimated completion time: 25 minutes

Lab 3 Tasks:
------------

TASK 1 – Setup Google’s API Credentials 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| *Note: If you do not have Google/gMail account, you will need to set one up. Navigate to:*   |
|                                                                                              |
| * https://console.developers.google.com/apis/credentials & follow the directions for setup.* |
+----------------------------------------------------------------------------------------------+ 
| |image60|                                                                                    |
| |image61|                                                                                    |
+----------------------------------------------------------------------------------------------+ 
   
+----------------------------------------------------------------------------------------------+
| 1. Navigate to https://console.developers.google.com/apis/credentials and log in with your   |
|                                                                                              |
|    developer account.                                                                        |
+----------------------------------------------------------------------------------------------+
| |image62|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. You will be redirected to the Google API’s screen. If you are previously familiar with    |
|                                                                                              |
|    Google API’s you can create a new Project.                                                |
|                                                                                              |
| 3. If you have not been you will be prompted to create a New Project.                        |
|                                                                                              |
| 4. Click **Create** in the dialogue box provided.                                            |
+----------------------------------------------------------------------------------------------+
| |image64|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. In the **New Project** window, provide a **Project Name**. The suggested value is:        |
|                                                                                              |
|    **F5 Federation oAuth**                                                                   |
|                                                                                              |
| *Note: If you have exceeded your project quota you may have to delete a project or*          |
|                                                                                              |
| *create a new account*                                                                       |
+----------------------------------------------------------------------------------------------+
| |image65|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. In the next screen, select **OAuth Client ID** for the **Credentials** type and           |
|                                                                                              |
|    click **Create Credentials**                                                              |
+----------------------------------------------------------------------------------------------+
| |image66|                                                                                    |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| 7. If you have not previously accepted a Consent Screen you may be prompted to do so.        |
|                                                                                              |
|    Click **Configure Consent Screen**.                                                       |
+----------------------------------------------------------------------------------------------+
| |image67|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. On the **OAuth Consent Screen** tab, enter the **email address** of your developer        |
|                                                                                              |
|    account (pre-populated) for the **Email Address**.                                        |
|                                                                                              |
| 9. For the **Product Name Shown to Users** enter **app.f5demo.com**.                         |
|                                                                                              |
| 10. Click **Save**.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image68|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 11. In the **Create OAuth Client ID*** screen select or enter the following values:          |
|                                                                                              |
|  -  **Application Type:** **Web Application**                                                |
|                                                                                              |
|  -  **Name**: **app.f5demo.com**                                                             |
|                                                                                              |
|  -  **Authorized JavaScript Engine:** **https://app.f5demo.com**                             |
|                                                                                              |
|  -  **Authorized Redirect URIs:** **https://app.f5demo.com/oauth/client/redirect**           |
|                                                                                              |
| 12. Click **Create**.                                                                        |
+----------------------------------------------------------------------------------------------+
| |image69|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 13. In the **OAuth Client** pop-up window copy and paste your **Client ID** and              |
|                                                                                              |
|    **Client Secret** in Gedit text editor provided on your desktop.                          |
+----------------------------------------------------------------------------------------------+
| |image70|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 2 – Setup F5 OAuth Provider 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **OAuth Provider** by navigating to **Access** -> **Federation** ->            |
|                                                                                              |
|   **OAuth Client/Resource Server** -> **Provider** and clicking **Create**.                  |
+----------------------------------------------------------------------------------------------+
| |image71|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Using the following values to complete the OAuth Provider                                 |
|                                                                                              |
| -  **Name:** **Google\_Provider**                                                            |
|                                                                                              |
| -  **Type:** **Google**                                                                      |
|                                                                                              |
| -  **Trusted Certificate Authorities:** **ca-bundle.crt**                                    |
|                                                                                              |
| -  **Allow Self-Signed JWK Config:**  **checked**                                            |
|                                                                                              |
| -  **Use Auto-discovered JWT:** **checked**                                                  |
|                                                                                              |
| 3. Click **Discover**.                                                                       |
|                                                                                              |
| 4. Accept all other defaults.                                                                |
|                                                                                              |
| 5. Click **Save**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image72|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 3 – Setup F5 OAuth Server (Client) 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **OAuth Server (Client)** by navigating to **Access** -> **Federation** ->     |
|                                                                                              |
|    **OAuth Client/Resource Server** -> **OAuth Server** and clicking **Create**.             |
+----------------------------------------------------------------------------------------------+
| |image73|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Using the following values to complete the OAuth Provider                                 |
|                                                                                              |
| -  **Name:** **Google\_Server**                                                              |
|                                                                                              |
| -  **Mode:** **Client**                                                                      |
|                                                                                              |
| -  **Type:** **Google**                                                                      |
|                                                                                              |
| -  **OAuth Provider:** **Google\_Provider**                                                  |
|                                                                                              |
| -  **DNS Resolver:** **proxy\_dns\_resolver**                                                |
|                                                                                              |
| -  **Client ID:** **<your client id>**                                                       |
|                                                                                              |
| -  **Client Secret:** **<your client secret>**                                               |
|                                                                                              |
| -  **Client’s Server SSL Profile Name:** **serverssl**                                       |
|                                                                                              |
| 3. Click **Finished**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image74|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 4 – Setup F5 Per Session Policy (Access Policy) 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **Per Session Policy** by navigating to **Access Profile/Policies** ->         |
|                                                                                              |
|    **Access Profiles (Per Session Policies)** and clicking **Create**.                       |
+----------------------------------------------------------------------------------------------+
| |image75|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. In the **New Profile** dialogue window enter the following values                         |
|                                                                                              |
| -  **Name:** **Google\_OAuth**                                                               |
|                                                                                              |
| -  **Profile Type:** **All**                                                                 |
|                                                                                              |
| -  **Profile Scope:** **Profile**                                                            |
|                                                                                              |
| -  **Language:** **English**                                                                 |
|                                                                                              |
| 3. Click **Finished**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image76|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Click **Edit** link on for the **Google\_OAuth** Access Policy.                           |
+----------------------------------------------------------------------------------------------+
| |image77|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. In the **Google\_OAuth** Access Policy, click the “\ **+**\ ” between **Start** & **Deny**|
|                                                                                              |
| 6. Click the **Authentication** tab in the events window.                                    |
|                                                                                              |
| 7. Scroll down and click the radio button for **OAuth Client**.                              |
|                                                                                              |
| 8. Click **Add**.                                                                            |
+----------------------------------------------------------------------------------------------+
| |image78|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. In the ***OAuth\_Client*** window enter the following values as shown:                    |
|                                                                                              |
| -  **Server:** **/Common/Google\_Server**                                                    |
|                                                                                              |
| -  **Grant Type:** **Authorization code**                                                    |
|                                                                                              |
| -  **OpenID Connect:** **Enabled**                                                           |
|                                                                                              |
| -  **OpenID Connect Flow Type:** **Authorization code**                                      |
|                                                                                              |
| -  **Authentication Redirect Request:** **/Common/GoogleAuthRedirectRequest**                |
|                                                                                              |
| -  **Token Request:** **/Common/GoogleTokenRequest**                                         |
|                                                                                              |
| -  **Refresh Token Request:** **/Common/GoogleTokenRefreshRequest**                          |
|                                                                                              |
| -  **OpenID Connect UserInfo Request:** **/Common/GoogleUserinfoRequest**                    |
|                                                                                              |
| -  **Redirection URI:** **https://%{session.server.network.name}/oauth/client/redirect**     |
|                                                                                              |
| -  **Scope:** **openid profile email**                                                       |
|                                                                                              |
| 10. Click **Save**.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image79|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 11. Click on the **Deny** link, in the **Select Binding**, select the **Allow** radio button |
|                                                                                              |
|    and click **Save**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image80|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 12. Click on the ***Apply Access Policy*** link in the top left-hand corner.                 |
|                                                                                              |
| *Note: Additional actions can be taken in the Per Session policy (Access Policy).*           |
|                                                                                              |
| *The lab is simply completing authorization. Other access controls can be implemented based* |
|                                                                                              |
| *on the use case*.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image81|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 5 – Associate Access Policy to Virtual Server 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Navigate to **Local Traffic** -> **Virtual Servers** -> **Virtual Server List** and click |
|                                                                                              |
|    on the **app.f5demo.com** Virtual Server link.                                            |
|                                                                                              |
| 2. Scroll to the **Access Policy** section.                                                  |
+----------------------------------------------------------------------------------------------+
| |image82|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Use the **Access Profile** drop down to change the **Access Profile** to **Google\_Auth** |
|                                                                                              |
| 4. Use the **Per-Request Policy** drop down to change the **Per-Request Policy** to          |
|                                                                                              |
|    **Google\_oauth\_policy**                                                                 |
|                                                                                              |
| 5. Scroll to the bottom of the **Virtual Server** configuration and click **Update**         |
+----------------------------------------------------------------------------------------------+
| |image83|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 6 – Test app.f5demo.com
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Navigate in your provided browser to **https://app.f5demo.com**                           |
+----------------------------------------------------------------------------------------------+
| |image84|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Authenticate with the account you established your Google Developer account with.         |
+----------------------------------------------------------------------------------------------+
| |image85|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Did you successfully redirect to the Google?                                              |
|                                                                                              |
| 4. After successful authentication, were you returned to the app.f5demo.com?                 |
|                                                                                              |
| 5. Did you successfully pass your OAuth Token?                                               |
+----------------------------------------------------------------------------------------------+
| |image86|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 7 – Per Request Policy Controls
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. In the application page for **https://app.f5demo.com** click the **Admin Link** shown     |
+----------------------------------------------------------------------------------------------+
| |image87|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. You will receive an **Access to this page is blocked** (customizable) message with a      |
|                                                                                              |
|    reference. You have been blocked because you do not have access on a per request basis.   |                                                                      
|                                                                                              |
| 3. Press the **Back** button in your browser to return to **https://app.f5demo.com**.        |
+----------------------------------------------------------------------------------------------+
| |image88|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Navigate to **Local Traffic** -> **iRules** -> **Datagroup List** and click on the        |
|                                                                                              |
|    **Allowed\_Users** datagroup.                                                             |
|                                                                                              |
| 5. Enter your **Google Account** used for this lab as the **String** value.                  |
|                                                                                              |
| 6. Click **Add** then Click **Update**.                                                      |
|                                                                                              |
| *Note: We are using a DataGroup control to minimize lab resources and steps. AD or LDAP*     |
|                                                                                              |
| *Group memberships, Session variables, other user attributes and various other access*       |
|                                                                                              |
| *control mechanisms can be used to achieve similar results.*                                 |
+----------------------------------------------------------------------------------------------+
| |image89|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. You should now be able to successfully to access the Admin Functions by clicking on the   |
|                                                                                              |
|    **Admin Link**.                                                                           |
|                                                                                              | 
| *Note: Per Request Policies are dynamic and do not require the same “Apply Policy” action as*|
|                                                                                              |
| *Per Session Policies.*                                                                      |
+----------------------------------------------------------------------------------------------+
| |image90|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. To review the Per Request Policy, navigate to **Access Profiles/Policies** ->             |
|                                                                                              |
|   **Per Request Policies** and click on the **Edit** link for the **Google\_oauth\_policy**. |
+----------------------------------------------------------------------------------------------+
| |image91|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. The various Per-Request-Policy actions can be reviewed                                    |
|                                                                                              |
| *Note: Other actions like Step-Up Auth controls can be performed in a Per-Request Policy.*   |
+----------------------------------------------------------------------------------------------+
| |image92|                                                                                    |
+----------------------------------------------------------------------------------------------+

TASK 8 – Review OAuth Results 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Review your Active Sessions (**Access** -> **Overview** -> **Active Sessions**).          |
|                                                                                              |
| 2. You can review Session activity or session variable from this window or kill the          |
|                                                                                              |
|    selected Session.                                                                         |
+----------------------------------------------------------------------------------------------+
| |image93|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Review your Access Report Logs (**Access** -> **Overview** -> **Access Reports**).        |
+----------------------------------------------------------------------------------------------+
| |image94|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. In the **Report Parameters window** click **Run Report**.                                 |
+----------------------------------------------------------------------------------------------+
| |image95|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. Look at the **SessionID** report by clicking the **Session ID** Link.                     |
+----------------------------------------------------------------------------------------------+
| |image96|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. Look at the **Session Variables** report by clicking the **View Session Variables** link. |
|                                                                                              |
|    Pay attention to the OAuth Variables.                                                     |
|                                                                                              |
| *Note: Any of these session variables can be used to perform further actions to improve*     |
|                                                                                              |
| *security or constrain access with logic in the Per-Session or Per Request VPE policies or*  |
|                                                                                              |
| *iRules/iRulesLX.*                                                                           |
+----------------------------------------------------------------------------------------------+
| |image97|                                                                                    |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| 7. Review your Access Report Logs (**Access** -> **Overview** -> **OAuth Reports** ->        |
|                                                                                              |
|    **Client/Resource Server**).                                                              |
+----------------------------------------------------------------------------------------------+
| |image98|                                                                                    |
+----------------------------------------------------------------------------------------------+

Lab 4 oAuth and AzureAD Lab
==============================

The purpose of this lab is to familiarize the Student with the using APM
in conjunction with Microsoft Azure AD. Microsoft Active Directory
Domain Services is offered by Microsoft Azure as a cloud service. This
can be used together with OpenID to log in to APM.

Objective:
----------

-  Gain an understanding of additional F5 OAuth features

-  Deploy a working configuration using F5 APM and Microsoft Azure AD

Lab Requirements:
-----------------

-  All lab requirements will be noted in the tasks that follow

-  Estimated completion time: 25 minutes

Lab 4 Tasks:
------------

TASK 1 – Create/Review New Application Registration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| *Note: The following steps in this task can just be "REVIEWED". As setting up a free Azure*  |
|                                                                                              |
| *account requires the entry of billing information, setting up an account and performing the*|
|                                                                                              |
| *steps below is a [REVIEW] task. For those desiring to set up an account refer to the*       |
|                                                                                              |
| *"APPENDIX: Setting up an Azure Development Account". For those with existing accounts*      |
|                                                                                              |
| *these steps may be followed if desired. For all others, simply review the steps in*         |
|                                                                                              |
| *Task1 and proceed to Task 2.*                                                               |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 1. Log into the Microsoft Azure Dashboard and click  **Azure Active Directory** in the left  |
|                                                                                              |
|    navigation menu.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image99|                                                                                    |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 2. Click on **App Registration** on the resulting menu and then                              |
|                                                                                              |
|    **New Application Registration** on the flyout menu.                                      |
+----------------------------------------------------------------------------------------------+
| |image100|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 3. In the pop menu for **Create App Registration**, enter the following values               | 
|                                                                                              |
| -  **Name:** **app.f5demo.com**                                                              |
|                                                                                              |
| -  **Application Type:** **Web App /API**                                                    |
|                                                                                              |
| -  **Sign On URL:** **https://app.f5demo.com**                                               |
|                                                                                              |
| 4. Click **Create**.                                                                         |
+----------------------------------------------------------------------------------------------+
| |image101|                                                                                   |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 5. In the resulting **app.f5demo.com Registered App** window, note & copy the                |
|                                                                                              |
|    **Application ID**. This will be used in a later setup step                               |
|                                                                                              |
| 6. Click **Settings**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image102|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 7. In the **Settings** flyout panel, click **Keys**                                          |
+----------------------------------------------------------------------------------------------+
| |image103|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 8. In the **Keys** flyout panel, enter the following values                                  |
|                                                                                              |
| -  **Description:** **app.f5demo.com**                                                       |
|                                                                                              |
| -  **Expires:** **In 2 Years**                                                               |
|                                                                                              |
| 9. Click **Save**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |image104|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 10. Note the message provided by Azure in the **Keys** panel.                                |
|                                                                                              |
| 11. Copy the ***Key Value*** for use in a later setup step.                                  |
+----------------------------------------------------------------------------------------------+
| |image105|                                                                                   |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 12. In the **Settings** flyout panel, click **Reply URL**.                                   |
+----------------------------------------------------------------------------------------------+
| |image106|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 13. In the **Reply URL** flyout panel, enter                                                 |
|                                                                                              |
|     **https://app.f5demo.com/oauth/client/redirect**                                         |
|                                                                                              |
| 14. Click **Save**.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image107|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 15. In the **Settings** flyout panel, click **Required Permissions**                         |
|                                                                                              |
| 16. In the **Required Permissions** flyout panel, click **Grant Permissions**                |
+----------------------------------------------------------------------------------------------+
| |image108|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 17. The following **Required Permissions** dialogue box may appear.                          |
|                                                                                              |
| 18. Click **Yes** to proceed.                                                                |
+----------------------------------------------------------------------------------------------+
| |image109|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 19. In the **Required Permissions** flyout panel, click **Windows Azure Active Directory**.  |
|                                                                                              |
| 20. In the **Enable Access** flyout panel, ensure the **Sign In and Read User Profile**.     |
|                                                                                              |
|     permission is checked.                                                                   |
|                                                                                              |
| 21. Click **Save**.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image110|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **[REVIEW]**                                                                                 |
|                                                                                              |
| 22. In the **Registered Application** panel, click **Manifest**.                             |
|                                                                                              |
| 23. In the **Edit Manifest** flyout panel, edit the **groupMembershipClaims** line (line 7)  |
|                                                                                              |
|     from **null** to **“All”** (note quotes are required).                                   |
|                                                                                              |
| 24. Click **Save**.                                                                          |
|                                                                                              |
| *Note: You can also update groupMembershipClaims to be "SecurityGroup".*                     |
+----------------------------------------------------------------------------------------------+
| |image111|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 2 – Create OAuth Request
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **OAuth Request** by navigating to **Access** -> **Federation** ->             |
|                                                                                              |
|    **OAuth Client/Resource Server** -> **Request** and clicking **Create**                   |
+----------------------------------------------------------------------------------------------+
| |image112|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Use the following values to create the Request                                            |
|                                                                                              |
| -  **Name:** **Azure\_AD\_Token**                                                            |
|                                                                                              |
| -  **HTTP Method:** **POST**                                                                 |
|                                                                                              |
| -  **Type:** **token-request**                                                               |
|                                                                                              |
| 3. Create the following Request Parameters using the Parameter Type drop down:               |
|                                                                                              |
| -  **Parameter Type:** **client-id**                                                         |
|                                                                                              |
| -  **Parameter Value:** **client\_id** (notice \_ )                                          |
|                                                                                              |
| -  **Parameter Type:** **client-secret**                                                     |
|                                                                                              |
| -  **Parameter Value:** **client\_secret** (notice \_ )                                      |
|                                                                                              |
| -  **Parameter Type:** **grant-type**                                                        |
|                                                                                              |
| -  **Parameter Value:** **grant\_type** (notice \_ )                                         |
|                                                                                              |
| -  **Parameter Type:** **redirect-uri**                                                      |
|                                                                                              |
| -  **Parameter Value:** **redirect\_uri** (notice \_ )                                       |
|                                                                                              |
| -  **Parameter Type:** **custom**                                                            |
|                                                                                              |
| -  **Parameter Name:** **resource**                                                          |
|                                                                                              |
| -  **Parameter Value:** **dd4bc4c7-2e90-41c9-9c41-b7eab5ab68b7**                             |
|                                                                                              |
| 4. Click **Finished**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image113|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 3 – Create OAuth Provider
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **OAuth Provider** by navigating to **Access** -> **Federation** ->            |
|                                                                                              |
|    **OAuth Client/Resource Server** -> **Provider** and clicking **Create**.                 |
+----------------------------------------------------------------------------------------------+
| |image114|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Use the following values to create the Request                                            |
|                                                                                              |
| -  **Name**: **f5demo\_AzureAD\_Provider**                                                   |
|                                                                                              |
| -  **Type**: **AzureAD**                                                                     |
|                                                                                              |
| -  **OpenID URI:** (replace **\_tennantID\_** with the following tenantID                    |
|                                                                                              |
|    **f5agilitydemogmail.onmicrosoft.com** )                                                  |
|                                                                                              |
| Resulting URI should be as follows:                                                          |
|                                                                                              |
| https://login.windows.net/f5agilitydemogmail.onmicrosoft.com/.well-known/openid-configuration|
|                                                                                              |
| 3. Click **Discover**.                                                                       |
|                                                                                              |
| 4. Click **Finished**.                                                                       |
|                                                                                              |
| *Note: if using another account you can find you TenantID by navigating to the*              |
|                                                                                              |
| *"Azure Portal" and clicking "Azure Active Directory". The tenant ID is the*                 |
|                                                                                              |
| *"default directory" as shown. The full name of the TenantID will be your*                   |
|                                                                                              |
| *"TenantID.onmicrosoft.com"*                                                                 |
+----------------------------------------------------------------------------------------------+
| |image115|                                                                                   |
|                                                                                              |
| |image116|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 4 – Create OAuth Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **OAuth Server (Client)** by navigating to **Access** -> **Federation** ->     |
|                                                                                              |
|    **OAuth Client/Resource Server** -> **OAuth Server*** and clicking **Create**.            |
+----------------------------------------------------------------------------------------------+
| |image117|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Using the following values to complete the OAuth Provider                                 |
|                                                                                              |
| -  **Name:** **f5demo\_AzureAD\_Server**                                                     |
|                                                                                              |
| -  **Mode:** **Client**                                                                      |  
|                                                                                              |
| -  **Type:** **AzureAD**                                                                     |
|                                                                                              |
| -  **OAuth Provider:** **f5demo\_AzureAD\_Provider**                                         |
|                                                                                              |
| -  **DNS Resolver:** **proxy\_dns\_resolver**                                                |
|                                                                                              |
| -  **Client ID:** **dd4bc4c7-2e90-41c9-9c41-b7eab5ab68b7**                                   |
|                                                                                              |
| -  **Client Secret:**  **YqHbzTosdBxdaGl9A/hXCs1ex1HWi+BTUSkgcfhbTwA=**                      |
|                                                                                              |
| -  **Client’s Server SSL Profile Name:** **serverssl-insecure-compatible**                   |
|                                                                                              |
| 3. Click **Finished**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image118|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 5 – Setup F5 Per Session Policy (Access Policy) 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Create the **Per Session Policy** by navigating to **Access** -> **Profile/Policies** ->  |
|                                                                                              |
|    **Access Profiles (Per Session Policies)** and clicking **Create**.                       |
+----------------------------------------------------------------------------------------------+
| |image119|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. In the **New Profile** dialogue window enter the following values                         |
|                                                                                              |
| -  **Name:** **AzureAD\_OAuth**                                                              |
|                                                                                              |
| -  **Profile Type:** **All**                                                                 |
|                                                                                              |
| -  **Profile Scope:** **Profile**                                                            |
|                                                                                              |
| -  **Language:** **English**                                                                 |
|                                                                                              |
| 3. Click **Finished**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image120|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Click **Edit** link on for the **AzureAD\_OAuth** Access Policy                           |
+----------------------------------------------------------------------------------------------+
| |image121|                                                                                   |
+----------------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------------+
| 5. In the **AzureAD\_OAuth** Access Policy, click the “\ **+**\ ” between **Start** & **Deny**|
|                                                                                               |
| 6. Click the **Authentication** tab in the events window.                                     |
|                                                                                               |
| 7. Scroll down and click the radio button for **OAuth Client**.                               |
|                                                                                               |
| 8. Click **Add**.                                                                             |
+-----------------------------------------------------------------------------------------------+
| |image122|                                                                                    |
+-----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. In the ***OAuth\_Client*** window enter the following values as shown:                    |
|                                                                                              |
| -  **Server:** **/Common/f5demo\_AzureAD\_Server**                                           |
|                                                                                              |
| -  **Grant Type:** **Authorization code**                                                    |
|                                                                                              |
| -  **OpenID Connect:** **Enabled**                                                           |
|                                                                                              |
| -  **OpenID Connect Flow Type:** **Authorization code**                                      |
|                                                                                              |
| -  **Authentication Redirect Request:** **/Common/AzureADAuthRedirectRequest**               |
|                                                                                              |
| -  **Token Request:** **/Common/Azure\_AD\_Token**                                           |
|                                                                                              |
| -  **Refresh Token Request:** **/Common/AzureADTokenRefreshRequest**                         |
|                                                                                              |
| -  **OpenID Connect UserInfo Request:** **None**                                             |
|                                                                                              |
| -  **Redirection URI:** **https://%{session.server.network.name}/oauth/client/redirect**     |
|                                                                                              |
| 10. Click **Save**.                                                                          |
+----------------------------------------------------------------------------------------------+
| |image123|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 11. Click on the **Deny** link, in the **Select Binding**, select the **Allow** radio button |
|                                                                                              |
|    and click **Save**.                                                                       |
+----------------------------------------------------------------------------------------------+
| |image124|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 12. Click on the **Apply Access Policy** link in the top left-hand corner.                   |
|                                                                                              |
| *Note: Additional actions can be taken in the Per Session policy (Access Policy). The lab*   |
|                                                                                              |
| *is simply completing authorization. Other access controls can be implemented based*         |
|                                                                                              |
| *on the use case.*                                                                           |
+----------------------------------------------------------------------------------------------+
| |image125|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 6 – Associate Access Policy to Virtual Server 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Navigate to **Local Traffic** -> **Virtual Servers** -> **Virtual Server List** and       |
|                                                                                              |
|    click on the **app.f5demo.com** Virtual Server link                                       |
|                                                                                              |
| 2. Scroll to the **Access Policy** section.                                                  |
+----------------------------------------------------------------------------------------------+
| |image126|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Use the **Access Profile** drop down to change the **Access Profile** to                  |
|                                                                                              |
|    **AzureAD\_OAuth**.                                                                       |
|                                                                                              |
| 4. Use the **Per-Request Policy** drop down to change the **Per-Request Policy** to          |
|                                                                                              |
|    **AzureAD\_oauth\_policy**.                                                               |
|                                                                                              |
| 5. Scroll to the bottom of the **Virtual Server** configuration and click **Update**.        |
+----------------------------------------------------------------------------------------------+
| |image127|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 7 – Test app.f5demo.com
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Navigate in your provided browser to **https://app.f5demo.com**                           |
+----------------------------------------------------------------------------------------------+
| |image128|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Authenticate with the following AzureAD account:                                          |
|                                                                                              |
| -  **Username:** **demouser@f5agilitydemogmail.onmicrosoft.com**                             |
|                                                                                              |
| -  **Password:** **f5d3m0u$3r**                                                              |
+----------------------------------------------------------------------------------------------+
| |image129|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Did you successfully redirect to the AzureAD?                                             |
|                                                                                              |
| 4. After successful authentication, were you returned to the app.f5demo.com?                 |
|                                                                                              |
| 5. Did you successfully pass your OAuth Token?                                               |
+----------------------------------------------------------------------------------------------+
| |image130|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 8 – Per Request Policy Controls
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. As in the prior lab, you can experiment with Per Request Policy controls. In the          |
|                                                                                              |
| application page for **https://app.f5demo.com** click the **Admin Link** shown.              |
+----------------------------------------------------------------------------------------------+
| |image131|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. You will receive an **Access to this page is blocked** (customizable) message with a      |
|                                                                                              |
|    reference. You have been blocked because you do not have access on a per request basis.   |
|                                                                                              |
| 3. Press the **Back** button in your browser to return to **https://app.f5demo.com**.        |
+----------------------------------------------------------------------------------------------+
| |image132|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Navigate to **Local Traffic** -> **iRules** -> **Datagroup List** and click on the        |
|                                                                                              |
|    **Allowed\_Users** datagroup.                                                             |
|                                                                                              |
| 5. Enter your **demouser@f5agilitydemogmail.onmicrosoft.com** used for this lab as the       |
|                                                                                              |
|    **String** value.                                                                         |
|                                                                                              |
| 6. Click **Add** then Click **Update**.                                                      |
|                                                                                              |
| *Note: We are using a DataGroup control to minimize lab resources and steps. AD or LDAP      |
|                                                                                              |
| *Group memberships, Session variables, other user attributes and various other access*       |
|                                                                                              |
| *control mechanisms can be used to achieve similar results.*                                 |
+----------------------------------------------------------------------------------------------+
| |image133|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. You should now be able to successfully to access the Admin Functions by clicking on the   |
|                                                                                              |
|    Admin Link.                                                                               |
|                                                                                              |
| *Note: Per Request Policies are dynamic and do not require the same “Apply Policy” action*   |
|                                                                                              |
| *as Per Session Policies*.                                                                   |
+----------------------------------------------------------------------------------------------+
| |image134|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. To review the Per Request Policy, navigate to ***Access** -> **Profiles/Policies** ->     |
|                                                                                              |
|    **Per Request Policies** and click on the Edit link for the **AzureAD\_oauth\_policy**.   |
+----------------------------------------------------------------------------------------------+
| |image135|                                                                                   |
+----------------------------------------------------------------------------------------------+
 
+----------------------------------------------------------------------------------------------+
| 9. The various Per-Request-Policy actions can be reviewed.                                   |
|                                                                                              |
| *Note: Other actions like Step-Up Auth controls can be performed in a Per-Request Policy*    |
+----------------------------------------------------------------------------------------------+
| |image136|                                                                                   |
+----------------------------------------------------------------------------------------------+

TASK 9 – Review OAuth Results 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Refer to the instructions and screen shots below:

+----------------------------------------------------------------------------------------------+
| 1. Review your Active Sessions (**Access** -> **Overview** -> **Active Sessions**).          |
|                                                                                              |
| 2. You can review Session activity or session variable from this window or kill the          |
|                                                                                              |
|    selected Session.                                                                         |
+----------------------------------------------------------------------------------------------+
| |image137|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Review your Access Report Logs (**Access** -> **Overview** -> **Access Reports**).        | 
+----------------------------------------------------------------------------------------------+
| |image138|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. In the **Report Parameters window** click **Run Report**.                                 |
+----------------------------------------------------------------------------------------------+
| |image139|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. Look at the **SessionID** report by clicking the **Session ID** Link.                     |
+----------------------------------------------------------------------------------------------+
| |image140|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. Look at the **Session Variables** report by clicking the **View Session Variables** link. |
|                                                                                              |
|    Pay attention to the OAuth Variables.                                                     |
|                                                                                              |
| *Note: Any of these session variables can be used to perform further actions to improve*     |
|                                                                                              |
| *security or constrain access with logic in the Per-Session or Per Request VPE policies*     |
|                                                                                              |
| *or iRules/iRulesLX.*                                                                        |
+----------------------------------------------------------------------------------------------+
| |image141|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. Review your Access Report Logs (**Access** -> **Overview** -> **OAuth Reports** ->        |  
|                                                                                              |
|    **Client/Resource Server**).                                                              |
+----------------------------------------------------------------------------------------------+
| |image142|                                                                                   |
+----------------------------------------------------------------------------------------------+

Conclusion
==========

Thank you for your participation in the 330 Access Policy Manager (APM)
Federation Lab. This Lab Guide has highlighted several notable features
of SAML Federation. It does not attempt to review all F5 APM Federation
features and configurations but serves as an introduction to allow the
student to further explore the BIG-IP platform and Access Policy Manager
(APM), its functions & features.

Appendix
========

The following are for informational purposes only

Setting up an AzureAD Developer Account
---------------------------------------

Refer to the information below:

+----------------------------------------------------------------------------------------------+
| 1. Navigate to the following URL to begin the process then follow the prompts as shown       |
|                                                                                              |
|    https://azure.microsoft.com/en-us/free/                                                   |
|                                                                                              |
| 2. The following images show the general flow to setup a free developer account              |
|                                                                                              |
| *Note: This process may change as dictated by Microsoft*                                     |
+----------------------------------------------------------------------------------------------+
| |image143|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 3. Initial Setup                                                                             |
+----------------------------------------------------------------------------------------------+
| |image144|                                                                                   |
| |image145|                                                                                   |
|                                                                                              |
| |image146|                                                                                   | 
| |image147|                                                                                   | 
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. About You                                                                                 |
+----------------------------------------------------------------------------------------------+
| |image148|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 5. Identity Verification by Phone                                                            |
+----------------------------------------------------------------------------------------------+
| |image149|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. Identity Verification by Card                                                             |
+----------------------------------------------------------------------------------------------+
| |image150|                                                                                   |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. Agreement                                                                                 |
+----------------------------------------------------------------------------------------------+
| |image151|                                                                                   |
+----------------------------------------------------------------------------------------------+

Learn More
==========

    The following are additional resources included for reference and
    assistance with this lab guide and other APM tasks.

Links & Guides
--------------

-  **Access Policy Manager (APM) Operations Guide:**
       https://support.f5.com/content/kb/en-us/products/big-ip_apm/manuals/product/f5-apm-operations-guide/_jcr_content/pdfAttach/download/file.res/f5-apm-operations-guide.pdf

-  **Access Policy Manager (APM) Authentication & Single Sign on
   Concepts:**
   https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0.html

-  **SAML:**

   -  **Introduction:**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/28.html#guid-28f26377-6e10-42c9-883a-3ac65eab9092

   -  **F5 SAML IdP (Identity Provider with Portal):**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/29.html#guid-42e93e4b-e4fc-4c3d-ae53-910641d5755c

   -  **F5 SAML IdP (Identity Provider without Portal):**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/30.html#guid-39ffed07-65f2-40b8-85ae-c80073cc4e82

   -  **F5 SAML SP (Service Provider):**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/31.html#guid-be2cf224-727e-4a0f-aa68-676fdedba37b

   -  **F5 Federation iApp (Includes o365):**
      https://www.f5.com/pdf/deployment-guides/saml-idp-saas-dg.pdf

   -  **F5 o365 Deployment Guide:**
      https://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf

-  **OAuth**

   -  **OAuth Overview:**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/35.html#guid-c1b617a7-07b5-4ad6-9b84-29d6ecd789b4

   -  **OAuth Client & Resource Server:**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/36.html#guid-c6db081e-e8ac-454b-84c8-02a1a282a888

   -  **OAuth Authorization Server:**
      https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/37.html#guid-be8761c9-5e2f-4ad8-b829-871c7feb2a20

   -  **Troubleshooting Tips**

      -  **OAuth Client & Resource Server:**
         https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/36.html#guid-774384bc-cf63-469d-a589-1595d0ddfba2

      -  **OAuth Authorization Server:**
         https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-sso-13-0-0/37.html#guid-8b97b512-ec2b-4bfb-a6aa-1af24842ee7a

-  **Kerberos**

   -  **Kerberos AAA Object**: (*See Reference section below*)

   -  **Kerberos Constrained Delegation:**
      http://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf

-  **Two-factor Integrations/Guides** (**Not a complete list**)

   -  **RSA Integration:**
          https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-12-1-0/6.html#conceptid

   -  **DUO Security: **

      -  https://duo.com/docs/f5bigip

      -  https://duo.com/docs/f5bigip-alt

   -  **SafeNet MobilePass:**
          http://www.safenet-inc.com/resources/integration-guide/data-protection/SafeNet_Authentication_Service/SafeNet_Authentication_Service__RADIUS_Authentication_on_F5_BIG-IP_APM_Integration_Guide

   -  **Google Authenticator:**
          https://devcentral.f5.com/articles/two-factor-authentication-with-google-authenticator-and-apm

-  **Access Policy Manager (APM) Deployment Guides:**

   -  **F5 Deployment Guide for Microsoft Exchange 2010/2013:**
          https://f5.com/solutions/deployment-guides/microsoft-exchange-server-2010-and-2013-big-ip-v11

   -  **F5 Deployment Guide for Microsoft Exchange 2016:**
          https://f5.com/solutions/deployment-guides/microsoft-exchange-server-2016-big-ip-v11-v12-ltm-apm-afm

   -  **F5 Deployment Guide for Microsoft SharePoint 2010/2013:**
          https://f5.com/solutions/deployment-guides/microsoft-sharepoint-2010-and-2013-new-supported-iapp-big-ip-v114-ltm-apm-asm-aam

   -  **F5 Deployment Guide for Microsoft SharePoint 2016:**
          https://f5.com/solutions/deployment-guides/microsoft-sharepoint-2016-big-ip-v114-v12-ltm-apm-asm-afm-aam

   -  **F5 Deployment Guide for Citrix XenApp/XenDesktop:**
          https://f5.com/solutions/deployment-guides/citrix-xenapp-or-xendesktop-release-candidate-big

   -  **F5 Deployment Guide for VMWare Horizon View:**
          https://f5.com/solutions/deployment-guides/vmware-horizon-view-52-53-60-62-70-release-candidate-iapp-big-ip-v11-v12-ltm-apm-afm?tag=VMware

   -  **F5 Deployment Guide for Microsoft Remote Desktop Gateway
          Services:**
          https://f5.com/solutions/deployment-guides/microsoft-remote-desktop-gateway-services-big-ip-v114-ltm-afm-apm

   -  **F5 Deployment Guide for Active Directory Federated Services:**
          https://f5.com/solutions/deployment-guides/microsoft-active-directory-federation-services-big-ip-v11-ltm-apm

    Notes:

+----------------------------------------------------------------------------------------------+
| F5 Networks, Inc. \| f5.com                                                                  |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| US Headquarters: 401 Elliott Ave W, Seattle, WA 98119 \| 888-882-4447                        |
|                                                                                              |
| Americas: info@f5.com                                                                        |
|                                                                                              |
| Asia-Pacific: apacinfo@f5.com                                                                |
|                                                                                              |
| Europe/Middle East/Africa: emeainfo@f5.com                                                   |
|                                                                                              |
| Japan: f5j-info@f5.com                                                                       |
|                                                                                              |
| ©2017 F5 Networks, Inc. All rights reserved. F5, F5 Networks, and the F5 logo are trademarks |
|                                                                                              |
| of F5 Networks, Inc. in the U.S. and in certain other countries. Other F5 trademarks are     |
|                                                                                              |
| identified at f5.com. Any other products, services, or company names referenced herein may   |
|                                                                                              |
| be trademarks of their respective owners with no endorsement or affiliation, express or      |
|                                                                                              |
| implied, claimed by F5. These training materials and documentation are F5 Confidential       |
|                                                                                              |
| Information and are subject to the F5 Networks Reseller Agreement. You may not share these   |
|                                                                                              |
| training materials and documentation with any third party without the express written        |
|                                                                                              |
| permission of F5.                                                                            |
+----------------------------------------------------------------------------------------------+
.. |image0| image:: media/image2.png
   :width: 6.96097in
   :height: 4.46512in
.. |image001| image:: media/image001.png
   :width: 4.5in
   :height: 0.74in
.. |image002| image:: media/image002.png
   :width: 4.5in
   :height: 3.37in
.. |image003| image:: media/image003.png
   :width: 4.5in
   :height: 3.38in
.. |image004| image:: media/image004.png
   :width: 4.5in
   :height: 0.73in
.. |image005| image:: media/image005.png
   :width: 4.5in
   :height: 3.37in
.. |image006| image:: media/image006.png
   :width: 4.5in
   :height: 1.15in
.. |image007| image:: media/image007.png
   :width: 4.5in
   :height: 2.28in
.. |image008| image:: media/image008.png
   :width: 4.5in
   :height: 0.96in
.. |image009| image:: media/image009.png
   :width: 4.5in
   :height: 1.69in
.. |image010| image:: media/image010.png
   :width: 4.5in
   :height: 2.94in
.. |image011| image:: media/image011.png
   :width: 4.5in
   :height: 0.80in
.. |image012| image:: media/image012.png
   :width: 4.5in
   :height: 1.12in
.. |image013| image:: media/image013.png
   :width: 4.5in
   :height: 4.00in
.. |image014| image:: media/image014.png
   :width: 4.5in
   :height: 1.48in
.. |image015| image:: media/image015.png
   :width: 4.5in
   :height: 1.12in
.. |image016| image:: media/image016.png
   :width: 4.5in
   :height: 1.54in
.. |image017| image:: media/image017.png
   :width: 4.5in
   :height: 1.29in
.. |image018| image:: media/image018.png
   :width: 4.5in
   :height: 5.46in
.. |image019| image:: media/image019.png
   :width: 4.5in
   :height: 2.13in
.. |image020| image:: media/image020.png
   :width: 4.5in
   :height: 1.01in
.. |image021| image:: media/image021.png
   :width: 4.5in
   :height: 1.93in
.. |image22| image:: media/image24.png
   :width: 3.12075in
   :height: 1.43714in
.. |image23| image:: media/image25.png
   :width: 3.15875in
   :height: 2.08094in
.. |image24| image:: media/image26.png
   :width: 3.54320in
   :height: 0.50920in
.. |image25| image:: media/image27.png
   :width: 3.48173in
   :height: 2.62090in
.. |image26| image:: media/image28.png
   :width: 3.65172in
   :height: 1.65644in
.. |image27| image:: media/image29.png
   :width: 1.37770in
   :height: 1.79508in
.. |image28| image:: media/image30.png
   :width: 3.20000in
   :height: 6.06388in
.. |image29| image:: media/image31.png
   :width: 3.56825in
   :height: 0.33742in
.. |image30| image:: media/image32.png
   :width: 3.40491in
   :height: 1.18125in
.. |image31| image:: media/image33.png
   :width: 2.92025in
   :height: 3.13718in
.. |image32| image:: media/image34.png
   :width: 3.52521in
   :height: 1.67580in
.. |image33| image:: media/image35.png
   :width: 3.42355in
   :height: 1.57385in
.. |image34| image:: media/image36.png
   :width: 3.32812in
   :height: 1.90502in
.. |image35| image:: media/image37.png
   :width: 3.31187in
   :height: 2.30806in
.. |image36| image:: media/image38.png
   :width: 3.24757in
   :height: 2.40753in
.. |image37| image:: media/image39.png
   :width: 3.31528in
   :height: 1.80296in
.. |image38| image:: media/image40.png
   :width: 3.33472in
   :height: 3.67040in
.. |image39| image:: media/image41.png
   :width: 3.27878in
   :height: 0.92046in
.. |image40| image:: media/image42.png
   :width: 3.12813in
   :height: 1.69163in
.. |image41| image:: media/image43.png
   :width: 3.22523in
   :height: 0.52038in
.. |image42| image:: media/image44.png
   :width: 3.23750in
   :height: 1.20928in
.. |image43| image:: media/image45.png
   :width: 3.29965in
   :height: 2.28233in
.. |image44| image:: media/image46.png
   :width: 3.35967in
   :height: 2.30729in
.. |image45| image:: media/image47.png
   :width: 3.33229in
   :height: 1.96747in
.. |image46| image:: media/image48.png
   :width: 3.23750in
   :height: 2.11911in
.. |image47| image:: media/image49.png
   :width: 3.46226in
   :height: 0.55215in
.. |image48| image:: media/image50.png
   :width: 3.34429in
   :height: 2.45452in
.. |image49| image:: media/image51.png
   :width: 3.55668in
   :height: 0.58963in
.. |image50| image:: media/image52.png
   :width: 3.55455in
   :height: 1.80809in
.. |image51| image:: media/image53.png
   :width: 3.50192in
   :height: 0.74026in
.. |image52| image:: media/image54.png
   :width: 3.46558in
   :height: 1.04152in
.. |image53| image:: media/image55.png
   :width: 1.18229in
   :height: 1.49240in
.. |image54| image:: media/image56.png
   :width: 1.01568in
   :height: 1.50521in
.. |image55| image:: media/image57.png
   :width: 1.07076in
   :height: 1.47917in
.. |image56| image:: media/image58.png
   :width: 3.47185in
   :height: 0.49895in
.. |image57| image:: media/image59.png
   :width: 3.22424in
   :height: 2.40961in
.. |image58| image:: media/image60.png
   :width: 2.23039in
   :height: 2.36979in
.. |image59| image:: media/image61.png
   :width: 3.49268in
   :height: 1.22650in
.. |image60| image:: media/image62.png
   :width: 1.37500in
   :height: 1.42298in
.. |image61| image:: media/image63.png
   :width: 1.83333in
   :height: 1.44662in
.. |image62| image:: media/image64.png
   :width: 3.61350in
   :height: 0.25904in
.. |image63| image:: media/image65.png
   :width: 1.32012in
   :height: 1.27746in
.. |image64| image:: media/image66.png
   :width: 3.45577in
   :height: 1.25767in
.. |image65| image:: media/image67.png
   :width: 3.08125in
   :height: 1.94452in
.. |image66| image:: media/image68.png
   :width: 3.16458in
   :height: 1.63370in
.. |image67| image:: media/image69.png
   :width: 3.18021in
   :height: 1.10982in
.. |image68| image:: media/image70.png
   :width: 2.88720in
   :height: 2.00521in
.. |image69| image:: media/image71.png
   :width: 3.28125in
   :height: 2.26534in
.. |image70| image:: media/image72.png
   :width: 3.33125in
   :height: 1.39217in
.. |image71| image:: media/image73.png
   :width: 3.43558in
   :height: 1.07255in
.. |image72| image:: media/image74.png
   :width: 3.49738in
   :height: 4.78430in
.. |image73| image:: media/image75.png
   :width: 3.58125in
   :height: 0.63905in
.. |image74| image:: media/image76.png
   :width: 3.38575in
   :height: 2.95455in
.. |image75| image:: media/image77.png
   :width: 3.59729in
   :height: 0.47370in
.. |image76| image:: media/image78.png
   :width: 3.58653in
   :height: 2.84049in
.. |image77| image:: media/image79.png
   :width: 3.55864in
   :height: 0.65031in
.. |image78| image:: media/image80.png
   :width: 3.64514in
   :height: 1.52147in
.. |image79| image:: media/image81.png
   :width: 3.59509in
   :height: 1.58711in
.. |image80| image:: media/image82.png
   :width: 3.55215in
   :height: 1.16329in
.. |image81| image:: media/image83.png
   :width: 3.53374in
   :height: 1.34193in
.. |image82| image:: media/image84.png
   :width: 3.50234in
   :height: 2.68712in
.. |image83| image:: media/image85.png
   :width: 3.49738in
   :height: 1.72209in
.. |image84| image:: media/image86.png
   :width: 3.57570in
   :height: 0.25694in
.. |image85| image:: media/image87.png
   :width: 3.24109in
   :height: 2.82822in
.. |image86| image:: media/image88.png
   :width: 3.16168in
   :height: 2.42702in
.. |image87| image:: media/image89.png
   :width: 2.86751in
   :height: 2.21224in
.. |image88| image:: media/image90.png
   :width: 2.80941in
   :height: 1.35399in
.. |image89| image:: media/image91.png
   :width: 3.15971in
   :height: 2.33461in
.. |image90| image:: media/image92.png
   :width: 3.40586in
   :height: 1.10658in
.. |image91| image:: media/image93.png
   :width: 3.42307in
   :height: 1.50171in
.. |image92| image:: media/image94.png
   :width: 3.45192in
   :height: 1.33345in
.. |image93| image:: media/image95.png
   :width: 3.59450in
   :height: 1.52876in
.. |image94| image:: media/image96.png
   :width: 2.06848in
   :height: 1.53438in
.. |image95| image:: media/image97.png
   :width: 3.52761in
   :height: 0.80655in
.. |image96| image:: media/image98.png
   :width: 3.64074in
   :height: 1.05961in
.. |image97| image:: media/image99.png
   :width: 3.62160in
   :height: 1.84971in
.. |image98| image:: media/image100.png
   :width: 3.60694in
   :height: 2.16776in
.. |image99| image:: media/image101.png
   :width: 3.53540in
   :height: 2.21472in
.. |image100| image:: media/image102.png
   :width: 3.57743in
   :height: 1.86503in
.. |image101| image:: media/image103.png
   :width: 3.51729in
   :height: 1.82209in
.. |image102| image:: media/image104.png
   :width: 3.50084in
   :height: 1.84049in
.. |image103| image:: media/image105.png
   :width: 3.46012in
   :height: 2.15172in
.. |image104| image:: media/image106.png
   :width: 3.44880in
   :height: 1.32496in
.. |image105| image:: media/image107.png
   :width: 3.48404in
   :height: 1.95989in
.. |image106| image:: media/image108.png
   :width: 3.42975in
   :height: 1.95950in
.. |image107| image:: media/image109.png
   :width: 3.40893in
   :height: 1.22224in
.. |image108| image:: media/image110.png
   :width: 3.34969in
   :height: 1.17463in
.. |image109| image:: media/image111.png
   :width: 3.10354in
   :height: 1.37929in
.. |image110| image:: media/image112.png
   :width: 3.21285in
   :height: 2.38037in
.. |image111| image:: media/image113.png
   :width: 3.49868in
   :height: 1.73941in
.. |image112| image:: media/image114.png
   :width: 3.57223in
   :height: 0.49387in
.. |image113| image:: media/image115.png
   :width: 3.51822in
   :height: 4.58896in
.. |image114| image:: media/image116.png
   :width: 3.50920in
   :height: 1.09553in
.. |image115| image:: media/image117.png
   :width: 3.48005in
   :height: 4.92024in
.. |image116| image:: media/image118.png
   :width: 3.39641in
   :height: 2.21472in
.. |image117| image:: media/image119.png
   :width: 3.58282in
   :height: 0.63933in
.. |image118| image:: media/image120.png
   :width: 3.52761in
   :height: 3.06445in
.. |image119| image:: media/image77.png
   :width: 3.74792in
   :height: 0.49354in
.. |image120| image:: media/image121.png
   :width: 3.52888in
   :height: 2.83435in
.. |image121| image:: media/image122.png
   :width: 3.52578in
   :height: 0.74560in
.. |image122| image:: media/image123.png
   :width: 3.50738in
   :height: 1.47828in
.. |image123| image:: media/image124.png
   :width: 3.56442in
   :height: 1.69631in
.. |image124| image:: media/image125.png
   :width: 3.46736in
   :height: 1.11639in
.. |image125| image:: media/image126.png
   :width: 3.55208in
   :height: 1.27646in
.. |image126| image:: media/image84.png
   :width: 3.50234in
   :height: 2.68712in
.. |image127| image:: media/image127.png
   :width: 3.54283in
   :height: 0.94203in
.. |image128| image:: media/image86.png
   :width: 3.57570in
   :height: 0.25694in
.. |image129| image:: media/image128.jpeg
   :width: 3.53525in
   :height: 1.87225in
.. |image130| image:: media/image129.png
   :width: 2.93452in
   :height: 2.37741in
.. |image131| image:: media/image130.png
   :width: 2.82477in
   :height: 2.26623in
.. |image132| image:: media/image90.png
   :width: 2.80941in
   :height: 1.35399in
.. |image133| image:: media/image91.png
   :width: 3.15971in
   :height: 2.33461in
.. |image134| image:: media/image92.png
   :width: 3.40586in
   :height: 1.10658in
.. |image135| image:: media/image131.png
   :width: 3.47790in
   :height: 1.47860in
.. |image136| image:: media/image132.png
   :width: 3.44664in
   :height: 0.99351in
.. |image137| image:: media/image133.png
   :width: 3.08095in
   :height: 1.31035in
.. |image138| image:: media/image134.png
   :width: 2.18483in
   :height: 1.62069in
.. |image139| image:: media/image135.png
   :width: 1.99074in
   :height: 0.45516in
.. |image140| image:: media/image136.png
   :width: 1.98052in
   :height: 0.89862in
.. |image141| image:: media/image137.png
   :width: 2.64361in
   :height: 2.43384in
.. |image142| image:: media/image138.png
   :width: 3.56993in
   :height: 1.64660in
.. |image143| image:: media/image139.png
   :width: 2.84352in
   :height: 1.33129in
.. |image144| image:: media/image140.png
   :width: 1.65644in
   :height: 1.35621in
.. |image145| image:: media/image141.png
   :width: 1.53374in
   :height: 1.34629in
.. |image146| image:: media/image142.png
   :width: 1.55828in
   :height: 1.56560in
.. |image147| image:: media/image143.png
   :width: 1.38650in
   :height: 1.55496in
.. |image148| image:: media/image144.png
   :width: 2.00614in
   :height: 2.21876in
.. |image149| image:: media/image145.png
   :width: 2.79693in
   :height: 1.78723in
.. |image150| image:: media/image146.png
   :width: 2.42294in
   :height: 2.73846in
.. |image151| image:: media/image147.png
   :width: 3.32514in
   :height: 1.16922in
