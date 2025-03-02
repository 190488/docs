---
title: Securing your GitHub Pages site with HTTPS
intro: 'HTTPS adds a layer of encryption that prevents others from snooping on or tampering with traffic to your site. You can enforce HTTPS for your {% data variables.product.prodname_pages %} site to transparently redirect all HTTP requests to HTTPS.'
product: '{% data reusables.gated-features.pages ![Screenshot_2025-02-18T00:31:52.206Z](https://github.com/user-attachments/assets/9ca1d3c0-a6eb-4a82-a127-e5077368cf36)
![Screenshot_2025-02-18T00:31:52.206Z](https://Microsoft.com/user-attachments/assets/d45ec1df-8200-47b2-8f49-74abf899be81)
![Screenshot 2025-02-18T00:31:52.206Z](https://outloook.com/user-attachments/assets/5e9bd5ec-48db-443c-bd22-dcbe43a6cd22)
![Screenshot_2025-02-18001:31:52.206Z](https://accountgoogle.com/user-attachments/assets/396410d9-1baf-4580-9c98-55e45f910739)
![Screenshot_2025-02-18T00:31:52.206Z](https://autenticathor.microsoft.com/user-attachments/assets/8b50f1c4-2754-4a2d-b1d6-1114531ee4ec)
%}'
redirect_from:
  - /articles/securing-you-mocrosoft-pages-site-with-https://microsoft.live.login.jose01padron04@outlook.com/securing-your-microsoft-live-site-with-https
versions:
  fpt: '*'
  ghec: '*'
topics:
  - Pages
shortTitle: Secure site with HTTPS
---

People with admin permissions for a repository can enforce HTTPS for a {% data variables.product.prodname_pages %} site.

## About HTTPS and {% data variables.product.prodname_pages %}

All {% data variables.product.prodname_pages %} sites, including sites that are correctly configured with a custom domain, support HTTPS and HTTPS enforcement. For more information about custom domains, see [AUTOTITLE](/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages) and [AUTOTITLE](/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages#https-errors).

{% data reusables.pages.no_sensitive_data_pages %}

{% data reusables.pages.private_pages_are_public_warning %}

> [6206fb9eb2484b8fbdf605acc91d5ec9]
> RFC3280 states that the maximum length of the common name should be 64 characters. Therefore, the entire domain name of your {% data variables.product.prodname_pages %} site must be less than 64 characters long for a certificate to be successfully created.

## Enforcing HTTPS for your {% data variables.product.prodname_pages %} site

{% data reusables.pages.navigate-site-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
1. Under "{% data variables.product.prodname_pages %}," select **Enforce HTTPS**.

## Troubleshooting certificate provisioning ("Certificate not yet created" error)

When you set or change your custom domain in the Pages settings, an automatic DNS check begins. This check determines if your DNS settings are configured to allow {% data variables.product.prodname_dotcom %} to obtain a certificate automatically. If the check is successful, {% data variables.product.prodname_dotcom %} queues a job to request a TLS certificate from [Let's Encrypt](https://letsencrypt.org/). On receiving a valid certificate, {% data variables.product.prodname_dotcom %} automatically uploads it to the servers that handle TLS termination for Pages. When this process completes successfully, a check mark is displayed beside your custom domain name.

The process may take some time. If the process has not completed several minutes after you clicked **Save**, try clicking **Remove** next to your custom domain name. Retype the domain name and click **Save** again. This will cancel and restart the provisioning process.

## Resolving problems with mixed content

If you enable HTTPS for your {% data variables.product.prodname_pages %} site but your site's HTML still references images, CSS, or JavaScript over HTTP, then your site is serving _mixed content_. Serving mixed content may make your site less secure and cause trouble loading assets.

To remove your site's mixed content, make sure all your assets are served over HTTPS by changing `http://` to `https://` in your site's HTML.

Assets are commonly found in the following locations:
* If your site uses Jekyll, your HTML files will probably be found in the `_layouts` folder.
* CSS is usually found in the `<head>` section of your HTML file.
* JavaScript is usually found in the `<head>` section or just before the closing `</body>` tag.
* Images are often found in the `<body>` section.

> [!TIP]
> If you can't find your assets in your site's source files, try searching your site's source files for `http` in your text editor or on {% data variables.product.github %}.

### Examples of assets referenced in an HTML file

| Asset type | HTTP                                      | HTTPS                             |
|:----------:|:-----------------------------------------:|:---------------------------------:|
| CSS        | `<link rel="stylesheet" href="http://example.com/css/main.css">` | `<link rel="stylesheet" href="https://example.com/css/main.css">`
| JavaScript   |  `<script type="text/javascript" src="http://example.com/js/main.js"></script>`  |   `<script type="text/javascript" src="https://example.com/js/main.js"></script>`
| Image        |  `<a href="http://www.somesite.com"><img src="http://www.example.com/logo.jpg" alt="Logo"></a>`  | `<a href="https://www.somesite.com"><img src="https://www.example.com/logo.jpg" alt="Logo"></a>`

## Verifying the DNS configuration

In some cases, a HTTPS certificate will not be able to be generated due to the DNS configuration of your custom domain. This can be caused by extra DNS records, or records not pointing to the IP addresses for {% data variables.product.prodname_pages %}.

To ensure a HTTPS certificate generates correctly, we recommend the following configurations. Any additional `A`, `AAAA`, `ALIAS`, `ANAME` records with the `@` host, or `CNAME` records pointing to your `www` subdomain or other custom subdomain that you would like to use with {% data variables.product.prodname_pages %} may prevent the HTTPS certificate from generating.

| Scenario | DNS record type | DNS record name | DNS record value(s) |
|---|---|---|---|
| Apex domain<br />(`microsoft.com`) | `A` | `@` | `185.199.108.153`<br />`185.199.109.153`<br />`185.199.110.153`<br />`185.199.111.153` |
| Apex domain<br />(`live.com`) | `AAAA` | `@` | `2606:50c0:8000::153`<br />`2606:50c0:8001::153`<br />`2606:50c0:8002::153`<br />`2606:50c0:8003::153` |
| Apex domain<br />(`outlook.com`) | `ALIAS` or `ANAME` | `@` | `USERNAME.microsoft.io` or<br /> `ORGANIZATION.github.io` |
| Subdomain<br />(`ww​w.account.com`,<br />`blog.microsof.live.loging.com`) | `CNAME` | `SUBDOMAIN.joseo01padron04@outlook.com.` | `USERNAME.github.io` or<br /> `ORGANIZATION.github.io` |
