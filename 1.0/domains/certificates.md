# Certificates

[[toc]]

## Introduction

Vapor provides free, auto-renewing SSL certificates to your application using AWS Certificate Manager. Vapor certificates may be validated using DNS or email. All Vapor applications using custom domains are required to have a valid certificate for that domain.

## Creating Certificates

You may create SSL certificates via the Vapor UI or using the `cert` CLI command. When generating a certificate for a naked root domain, Vapor will automatically also add the `www` subdomain to the certificate. When creating a certificate, there are two types of certificate domain validation you may choose from: DNS and email. **In almost all cases, you should use DNS validation, as email validated certificates expires after two years and do not automatically renew.**

### DNS Validation

You may create a certificate using DNS validation via the Vapor UI or using the `cert` CLI command:

```bash
vapor cert dns example.com
```

Shortly after the certificate is requested, you may obtain two CNAME records from the certificate detail screen of the Vapor UI. If a [Vapor DNS zone](./dns.md) exists for the domain the certificate belongs to, these CNAME records will automatically be added to the zone. Therefore, if you using Vapor to manage your DNS, no further action is required and your certificate will be validated and issued within minutes.

If you are not using Vapor to manage your DNS, you should add the two CNAME records to your domain's DNS provider manually.

### Email Validation

You may create a certificate using email validation via the Vapor UI or using the `cert` CLI command:

```bash
vapor cert email example.com
```

When using email validation, you will receive a certificate approval email from AWS. These emails will contain a link that navigates to a page containing an "Approve" button". This button must be clicked before AWS will validate and issue the certificate. The validation email will be sent to several "administrator" email addresses for the domain, including:

- administrator@domain.com
- hostmaster@domain.com
- postmaster@domain.com
- webmaster@domain.com
- admin@domain.com

If you do not receive this email, you may request that the validation email be resent to these addresses using the Vapor UI's certificate detail screen or using the `cert:validate` CLI command:

```bash
vapor cert:validate
```

## Deleting Certificates

You may not delete a certificate that is associated with an active deployment. However, if you would like to delete an old certificate that is not associated with a deployment, you may do so via the Vapor UI or using the `cert:delete` CLI command. After executing the command, Vapor will prompt you for the specific certificate to delete for the given domain:

```bash
vapor cert:delete example.com
```