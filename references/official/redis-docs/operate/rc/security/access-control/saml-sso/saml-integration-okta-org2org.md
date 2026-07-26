---
title: "Okta SAML integration guide (Org2Org)"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/saml-sso/saml-integration-okta-org2org/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/saml-sso/saml-integration-okta-org2org"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Okta SAML integration guide (Org2Org)"
---
# Okta SAML integration guide (Org2Org)

This integration guide shows how to set up Okta as a SAML single sign on provider for your Redis Cloud account.

            Redis Cloud
          

Specific identity provider details may be different than shown in this guide. Always consult your [identity provider's docs](https://help.okta.com/en-us/Content/Topics/Security/Identity_Providers.htm) for the latest information.

This guide shows how to configure [Okta](https://help.okta.com/en-us/Content/Topics/Security/Identity_Providers.htm) as a SAML single sign-on identity provider (IdP) for your Redis Cloud account.

This guide shows how to use the Org2Org application template. You can also use the [[operate/rc/security/access-control/saml-sso/saml-integration-okta-generic|Generic]] application template.

To learn more about Redis Cloud support for SAML, see [[operate/rc/security/access-control/saml-sso|SAML single sign-on]].

Before completing this guide, you must [[operate/rc/security/access-control/saml-sso#verify-domain|verify ownership of any domains]] you want to associate with your SAML setup.

## Step 1: Set up your identity provider

### Create the Okta SAML integration application

Create an Okta "Org2Org" SAML integration appliction.

Sign in to the Okta admin console.

From the left menu, select Applications.

Select Browse App Catalog.

Locate and select Okta Org2Org.

[](/docs/latest/images/rc/saml/okta_saml_1.png)

Once you have found the application, click "Add".

[](/docs/latest/images/rc/saml/okta_saml_2.png)

Enter this field for the Org2Org application General Settings section and select Next:

Application label: `Redis Cloud`

[](/docs/latest/images/rc/saml/okta_saml_3.png)

Enter the following fields in the Sign-On Options > Attributes section:

Name: `redisAccountMapping`
Name Format: `Basic`
Value: `appuser.redisAccountMapping`

Warning:
  To ensure the role mapping will not take effect, don't skip entering `appuser.redisAccountMapping` in the Value field.

[](/docs/latest/images/rc/saml/okta_saml_4.png)

Next, select View Setup Instructions. A new browser window opens, providing the information needed to configure the IdP in Redis Cloud.

[](/docs/latest/images/rc/saml/okta_saml_5.png)

Scroll down to section 6 in the page, and note the following information:

IdP Issuer URI
IdP Single Sign-On Url
IdP Signature Certificate: Click the link and download the certificate to your hard drive

[](/docs/latest/images/rc/saml/okta_saml_6.png)

Once you capture the information, close the window, return to the Okta admin console, and select Done.

### Modify the application user profile

In the left menu, select Directory > Profile Editor, then select Redis Cloud User.

[](/docs/latest/images/rc/saml/okta_saml_7_customer.png)

Select Add Attribute to add a custom attribute to the user profile and specify the Redis Cloud role.

[](/docs/latest/images/rc/saml/okta_saml_7_5_customer.png)

Add this information for the new custom attribute:

Data type: `string array`
Display name: `redisAccountMapping`
Variable nam: `redisAccountMapping`
Description: `redisAccountMapping`
Attribute required: `Yes`
Group priority: `Combine values across groups`

[](/docs/latest/images/rc/saml/okta_saml_app_int_11.png)

Once you add the attribute, it appears in the list of profile attributes.

[](/docs/latest/images/rc/saml/okta_saml_9.png)

Add a Redis Cloud icon to the application because it's easier for users to identify the application. Select the pencil icon on the application logo and upload a Redis image using these steps:

[](/docs/latest/images/rc/saml/okta_saml_10_customer.png)
[](/docs/latest/images/rc/saml/okta_saml_11_customer.png)

## Step 2: Create a group and assign the application

Now that our SAML IdP is configured, create an Okta group and assign the Redis Cloud application.

### Create the group

In the left menu, select Directory > Groups, then select Add group.

[](/docs/latest/images/rc/saml/okta_saml_group_1.png)

Enter Name and Description.

[](/docs/latest/images/rc/saml/okta_saml_group_2.png)
[](/docs/latest/images/rc/saml/okta_saml_group_3.png)

### Assign users to the group

Select the group, then select Assign people.

[](/docs/latest/images/rc/saml/okta_saml_group_4.png)

For each user you want to add to the group, highlight the user in the table and select +. You can also add all users by selecting Add all. After you add all the users to your group, select Save.

[](/docs/latest/images/rc/saml/okta_saml_group_5.png)

### Assign application to the group

Now that your group is populated with its users, assign the SAML integration application to your group.

From the menu, select Applications > Applications > Redis Cloud. Then, select Assign to groups.

[](/docs/latest/images/rc/saml/okta_saml_group_6.png)

In the Redis Cloud User Group, select Assign.

[](/docs/latest/images/rc/saml/okta_saml_group_7.png)

Now, define the Redis account mapping string default for this group and select Save and go back. The key-value pair consists of the lowercase role name (owner, member, manager, billing_admin, or viewer) and your Redis Cloud Account ID found in the [[operate/rc/accounts/account-settings|account settings]]. Select "Done".

[](/docs/latest/images/rc/saml/okta_saml_group_8.png)

The mapping field is now defined as a default for each member of the group.

[](/docs/latest/images/rc/saml/okta_saml_group_9.png)

### Editing the mapping field for the group

To modify the Redis mapping field, select the pencil icon of the Redis Cloud group in the "Redis Cloud" application screen.

[](/docs/latest/images/rc/saml/okta_saml_group_10.png)

You can modify the mapping field for the whole group on the edit screen that appears.

[](/docs/latest/images/rc/saml/okta_saml_group_11.png)

### Editing the mapping field for a specific user

To override the Redis mapping field at an individual user level, select the People menu, then select the pencil icon of the person whos field you want to modify.

[](/docs/latest/images/rc/saml/okta_saml_group_12.png)

Set the user's Assignment master to `Administrator` to enable group policy overrides. Select Save.

[](/docs/latest/images/rc/saml/okta_saml_group_13.png)

The user's Type is set to `Individual`.

[](/docs/latest/images/rc/saml/okta_saml_group_14.png)

On the screen that appears, select the pencil icon of the user to modify the Redis mapping field.

[](/docs/latest/images/rc/saml/okta_saml_group_15.png)
[](/docs/latest/images/rc/saml/okta_saml_group_16.png)

## Step 3: Configure SAML support in Redis Cloud

Now that you have a test IdP server and your user group ready, configure support for SAML in Redis Cloud.

### Sign in to Redis Cloud

Sign in to your account on the [Redis Cloud console](https://cloud.redis.io/#/login).

### Activate SAML in access management

To activate SAML, you must have a local user (or social sign-on user) with the owner role. If you have the correct permissions, the Single Sign-On tab is enabled.

Add the information you saved previously in the setup form (step 1), including:

Issuer (IdP Entity ID): IdP Issuer URI
IdP server URL: IdP Single Sign-On Url
Assertion signing certificate: Drag and drop the file you downloaded to disk in the form text area.

[](/docs/latest/images/rc/saml/sm_saml_1.png)

Select Enable and wait a few seconds for the status to change. Then, download the service provider (SP) metadata. Save the file to your local hard disk.

[](/docs/latest/images/rc/saml/sm_saml_3.png)

Open the file in any text editor. Save the following text from the metadata:

EntityID: Unique name of the service provider (SP)

[](/docs/latest/images/rc/saml/sm_saml_4.png)

Location: Location of the assertion consumer service

[](/docs/latest/images/rc/saml/sm_saml_5.png)

Return to Okta, select Applications > Redis Cloud > General, then select Edit.

[](/docs/latest/images/rc/saml/sm_saml_6.png)

Update this information in Advanced Sign-on Settings.

Hub ACS URL: Use the information that you copied for Location.
Audience URI: Use the information that you copied for EntityID.

[](/docs/latest/images/rc/saml/sm_saml_7.png)

Select Save.

### IdP-initiated SSO

To use IdP-initiated SSO with identity providers, set the RelayState parameter to URL `https://cloud.redis.io/#/login/?idpId=<ID>`.

Note:
  Replace `<ID>` so it matches the AssertionConsumerService Location URL ID (the content after the last forward slash "/"). To learn more about configuring service provider applications, see your identity provider's documentation.

### Return to Redis Cloud console

Return to Redis Cloud console and select Activate.

[](/docs/latest/images/rc/saml/sm_saml_8.png)

A popup appears, explaining that, to test the SAML connection, you need to log in with Okta credentials of the user defined in the Redis Cloud group. This user is part of the group to which you assigned the Redis Cloud application. Select Continue to go to the Okta login screen.

The Okta login screen appears. Enter the credentials and select Sign In.

[](/docs/latest/images/rc/saml/sm_saml_10.png)

If everything is configured correctly, you will see the the Redis Cloud console screen. Your local account is now considered a SAML account.

To log in to the Redis Cloud console from now on, click on Sign in with SSO.

[](/docs/latest/images/rc/button-sign-in-sso.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page