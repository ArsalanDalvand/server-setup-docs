---
title: "Social login"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/social-login/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/social-login"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Social login"
---
# Social login

Social login with Redis Cloud.

            Redis Cloud
          

Redis Cloud supports the following social logins:

[GitHub](https://github.com/)
[Google](https://accounts.google.com/)

Note:
  If your Google and GitHub accounts share an email address (such as Gmail), you can use either one to sign in to the same Redis Cloud account.

## Create a new account with social login

To set up Google login:

Select Google from the [sign-in](https://cloud.redis.io/) screen.
Sign in with the Google account you would like to use.
Select Confirm on the Sign in with Google prompt.

To set up GitHub login:

Select Github from the [sign-in](https://cloud.redis.io/) screen.
Sign in with the Github account you would like to use.
Select Authorize on the Authorize Redis Okta Auth Github prompt.

Note:
  If the email address associated with your GitHub account is not public, you will see an error message that your sign in attempt failed. You need to [make your email address public](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences) on GitHub before you try again.

## Migrate an existing account to social login

If you already have a Redis Cloud account that requires an email address and password to sign in, you can migrate your existing account to use a social login associated with that same email address instead.

Warning:
  Once you migrate your account to use social login, you cannot revert to your old email/password sign in method.

To migrate your account to Google social login:

Select Google on the [sign-in](https://cloud.redis.io/) screen.
Choose your preferred account from the list.
A confirmation prompt will display and warn that you cannot revert to your old sign in method if you proceed with the migration to social login.
Select Confirm to continue migration.

To migrate your account to GitHub social login:

Select Github on the [sign-in](https://cloud.redis.io/) screen.
Select Authorize on the Authorize Redis Okta Auth Github prompt.
Enter your GitHub password to Confirm access. A confirmation prompt warns that you cannot revert to your old sign-in method if you proceed with the migration to social login.
Select Confirm to continue migration.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page