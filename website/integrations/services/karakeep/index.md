---
title: Integrate with Karakeep
sidebar_label: Karakeep
support_level: community
---

## What is Karakeep

> A self-hostable bookmark-everything app (links, notes and images) with AI-based automatic tagging and full-text search.
>
> -- https://karakeep.app/

## Preparation

The following placeholders are used in this guide:

- `karakeep.company` is the FQDN of the Karakeep installation.
- `authentik.company` is the FQDN of the authentik installation.

:::note
This documentation lists only the settings that you need to change from their default values. Be aware that any changes other than those explicitly mentioned in this guide could cause issues accessing your application.
:::

## authentik configuration

To support the integration of Karakeep with authentik, you need to create an application/provider pair in authentik.

### Create an application and provider in authentik

1. Log in to authentik as an administrator and open the authentik Admin interface.
2. Navigate to **Applications** > **Applications** and click **Create with Provider** to create an application and provider pair. (Alternatively you can first create a provider separately, then create the application and connect it with the provider.)

- **Application**: provide a descriptive name, an optional group for the type of application, the policy engine mode, and optional UI settings.
- **Choose a Provider type**: select **OAuth2/OpenID Connect** as the provider type.
- **Configure the Provider**: provide a name (or accept the auto-provided name), the authorization flow to use for this provider, and the following required configurations.
    - Note the **Client ID**,**Client Secret**, and **slug** values because they will be required later.
    - Set a `Strict` redirect URI to `https://karakeep.company/api/auth/callback/custom`.
    - Select any available signing key.
- **Configure Bindings** _(optional)_: you can create a [binding](/docs/add-secure-apps/flows-stages/bindings/) (policy, group, or user) to manage the listing and access to applications on a user's **My applications** page.

3. Click **Submit** to save the new application and provider.

## Karakeep configuration

In Karakeep, you'll need to add these environment variables:

```sh
NEXTAUTH_URL=https://karakeep.company
OAUTH_CLIENT_ID=<Client ID from authentik>
OAUTH_CLIENT_SECRET=<Client secret from authentik>
OAUTH_WELLKNOWN_URL=https://authentik.company/application/o/karakeep/.well-known/openid-configuration
OAUTH_PROVIDER_NAME=authentik
OAUTH_ALLOW_DANGEROUS_EMAIL_ACCOUNT_LINKING=true
# Optional: You can add this if you only want to allow login with Authentik
# DISABLE_PASSWORD_AUTH=true
# Optional but highly recommended:
# DISABLE_SIGNUPS=true
```

Finally, restart the Karakeep server and test your configuration.
