{
"title": "Configure HTTP strict transport security",
  "linkTitle": "Configure HTTP strict transport security",
  "weight": 150,
  "date": "2021-02-26",
  "description": "Create an HSTP profile and configure it for HTTP services."
}
HTTP Strict Transport Security (HSTS) is a browser security mechanism, implemented by way of an HTTP `Strict-Transport-Security` response header, which allows the connection with a website through secure connections (HTTPS) only.

HSTS enforces security on applications by mitigating attacks such as man-in-the-middle, insecure link referencing, and invalid certificates.

## Add an HSTS profile

To configure HSTS, you must first add an HSTS profile in Policy Studio.

1. In Policy Studio, click **Environment Configuration > Libraries > HSTS Profiles**.
2. Right-click **HSTS Profiles**, and select **Add an HSTS Profile**.
3. Configure the following settings:

   * **Name**: Unique name of the HSTS profile. Defaults to HTTP Strict Transport Security.
   * **Enable HSTS**: Specifies whether HSTS processing is enabled for the profile. Defaults to enabled.
   * **HSTS Parameters:**

     * **max-age (seconds)**: Specifies the time, in seconds, that the browser should remember that a site is only to be accessed using HTTPS.
     * **includeSubDomains**: Specifies this rule applies to all of the site's subdomains as well. Defaults to enabled.
4. Click **OK**.

## Configure an HSTS profile

The HSTS profile is not configured in SSL interfaces by default. To enable it, perform the following steps.

1. In Policy Studio, create a new project **From existing configuration** and browse the directory of nodemanager config (apigateway/conf/configs.xml)
2. Click **Environment Configuration > Listeners**.
3. Select the interface you wish to configure, for example, **Node Manager > Management Services**.
4. Select **Ports**.
5. Right-click the HTTPS interface you wish to update, and click **Edit**.
6. Click the **Advanced** tab, and under **HSTS Settings**, select the HSTS profile you wish to use to protect all endpoints serviced by this interface.
7. Click **OK**.

![To enable HSTS profile](/Images/docbook/images/general/hsts5.png)

**NOTE**: To apply the configuration changes done in policy studio for nodemanager you need to copy the config files of your project's folder to apigateway/conf folder.

To enable HSTS profile for other SSL ports under API Gateway listeners, in policy studio, create a project from "**New Project from an API Gateway instance**" and follow the above steps from 2 to 7. Deploy the configuration.

Any changes made to HSTS profile settings needs a deployment to API Gateway. When you open the UI in a browser all the responses from the interface contains “`Strict-Transport-Security`” with max-age and includeSubDomains.

{{% alert title="Note" %}}
When HSTS is enabled, it is for the entire domain regardless of any other ports configured on the listeners. Any non-SSL ports that exist in the configuration (listeners) will no longer work from the browser after an HSTS-protected interface has been invoked from the browser.

HSTS is made redundant when self-signed certificates are employed.
{{% /alert %}}

By default, the following ports use self-signed certificates:

* API Manager UI(8075)
* API Manager traffic (8065)
* API Gateway Manager UI (8090)
* API Gateway Analytics (8040)
* Client Application Registry (8089)