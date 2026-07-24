# Domain-Setup

# Azure + GoDaddy Custom Domain Setup

Pre-requisites: 
* A working website, with web address and IP Address of Web Application.
  * Azure and AWS offers some free hosting options. They both provide SSL Certifcates. 

## GoDaddy Side 
* Sign into GoDaddy Account
* Create your domain
* Select `DNS`
* Edit A Record
  * Input the IP Address of your site (information found on Azure website) 
* Edit or Create CNAME
  * Input the web address of your site. Note: this can take 24-hours to propagate
* Go to fowarding - this step ensures the naked site work 
  * Recommend `https://`
  * Enter your domain name with the `www`
  * Select `Permanent (301)`
 
## Azure Side
* Select Custome Domain
  * Select Add - input the GoDaddy Domain as an Alias
 
# GitHub Pages + WHC Custom Domain Setup

This guide explains how to connect a custom domain from WHC to a GitHub Pages. GitHub requires DNS records for the root domain and the `www` subdomain, while WHC is the place where those DNS records are created and edited.

## Prerequisites

Before starting, make sure the GitHub Pages site already works on its default `username.github.io` address and that the domain is actively managed in WHC. The domain should also be using WHC nameservers if DNS is being edited in WHC's DNS Zone Editor.

## WHC side

Log in to WHC and open the domain's DNS management area through **Client Area → Domains → Domain Management Tools → DNS Zone Editor**. This is where the DNS records for the domain should be added, changed, or removed.

### Add root domain A records

For the apex or root domain (the version without `www`), add four `A` records using `@` as the host value and these GitHub Pages IP addresses as the targets:

- `185.199.108.153`
- `185.199.109.153`
- `185.199.110.153`
- `185.199.111.153`

These IPs are published by GitHub for GitHub Pages and are not unique per account or repository.

### Add the www CNAME record

For the `www` version of the domain, create a `CNAME` record with:

- Host: `www`
- Target: `username.github.io`

GitHub's documentation says the `www` subdomain should point to the GitHub Pages default domain rather than back to the custom domain itself.

### Remove conflicting records

Remove old or conflicting `A`, `AAAA`, or `CNAME` records for the same hostnames if they point somewhere other than GitHub Pages, because extra DNS records can prevent GitHub from validating the domain or issuing an HTTPS certificate. Avoid wildcard DNS records like `*` because GitHub warns they can create takeover risk and interfere with safe custom-domain use.

### Optional TXT verification record

If GitHub provides a TXT record for domain verification, add it in WHC exactly as shown in GitHub's instructions. GitHub recommends leaving the TXT record in place after verification to help protect the domain against takeover issues.

## GitHub side

In the GitHub repository that publishes the site, go to **Settings → Pages** and enter the custom domain in the Pages configuration. This tells GitHub which domain to serve for the Pages site and may automatically create or update the repository's `CNAME` file depending on the publishing method.

### Set the custom domain

Choose the preferred live domain and enter it exactly in the **Custom domain** field in GitHub Pages settings. If the goal is to use the `www` version as the main address, enter the `www` domain there and make sure the corresponding `www` CNAME record exists in WHC.

### Enable HTTPS

Once GitHub detects the DNS correctly and provisions the certificate, enable **Enforce HTTPS** in the Pages settings. If HTTPS is unavailable, GitHub notes that extra DNS records or incorrect DNS configuration can block certificate generation.

### Re-add the domain if provisioning gets stuck

If the domain is correctly configured in DNS but HTTPS or validation remains stuck, remove the custom domain from GitHub Pages settings, save, then add it back again to trigger a fresh validation and certificate provisioning attempt.

## Recommended DNS layout

| Host | Type | Value |
|---|---|---|
| `@` | `A` | `185.199.108.153` |
| `@` | `A` | `185.199.109.153` |
| `@` | `A` | `185.199.110.153` |
| `@` | `A` | `185.199.111.153` |
| `www` | `CNAME` | `username.github.io`|
| Verification host | `TXT` | Value provided by GitHub, if requested. Found in general settings - pages |

## Order of operations

1. Confirm the GitHub Pages site already works on the default GitHub Pages URL.
2. In WHC, add the four root `A` records and the `www` `CNAME` record.
3. Remove conflicting or leftover parking records for the same hosts.
4. In GitHub Pages settings, enter the custom domain.
5. Add the optional GitHub TXT verification record if prompted. (recommended)
6. Wait for DNS propagation and for GitHub to complete domain validation.
7. Enable **Enforce HTTPS** once it becomes available.

## Troubleshooting

If the site still shows a registrar parking page or an old hosting page, DNS propagation may still be in progress or a conflicting record may still exist. GitHub notes that DNS changes can take up to 24 hours to propagate, and in practice some resolvers may take longer.

If the root domain works but `www` does not, the most common cause is that the `www` record is not pointing to `username.github.io` correctly. If HTTP works but HTTPS does not, GitHub may still be waiting to validate the domain and issue the certificate, or extra DNS records may be interfering with HTTPS provisioning.
