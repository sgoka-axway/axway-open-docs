{
"title": "Configure HTTP Strict Transport Security",
  "description": "Add a HSTP profile, and configure for HTTP services."
}
HSTS-enabled web sites declare themselves accessible only via secure connections and for users to be able to direct their user agents to interact with given sites only over secure connections. This HSTS policy is declared by websites via the Strict-Transport-Security HTTP response header field.

When HSTS is configured, the webserver sends the special HTTP response header called "Strict-Transport-Security" to the client. This header includes the max-age attribute, which specifies a duration of time in seconds. Optionally, the directive 'includeSubdomains' can be included to indicate that this policy should apply to all sub-domains as well. After a browser receives this header over a secure HTTPS connection (without any certificate errors), the browser makes new requests to the application over secure HTTPS connections for the duration of time specified in the header. Any links to the server over insecure HTTP connections are automatically rewritten to HTTPS before the request is made. HSTS prevents a user from ignoring certificate errors, as it prevents the user from connecting to a website with an invalid certificate.

Applications that do not utilize the "HTTP Strict-Transport-Security" policy are more susceptible to man-in-the-middle attacks via SSL stripping, which occurs when an attacker transparently downgrades a victim's communication with the server from HTTPS to HTTP. Once this is accomplished, the attacker gains the ability to view and potentially modify the victim's traffic, exposing sensitive information, and gaining access to unauthorized functionality.

## Add a HSTS profile

To configure HSTS in API Gateway, API Manager, Analytics and Client Application Registry, a HSTS profile must first be created in Policy Studio.

1. In Policy Studio, navigate to Environment Configuration > Libraries > HSTS Profiles.
2. Right-click, select 'Add a HSTS Profile', and configure the settings.

Configure the following settings:

* **Name**: Unique name of the HSTS profile. Defaults to HTTP Strict Transport Security.
* **Enable HSTS**: Specifies whether HSTS processing is enabled for the profile. Enabled by default.
* **HSTS Parameters:**

    * max-age(seconds): Specifies the time, in seconds, that the browser should remember that a site is only to be accessed using HTTPS.
    * includeSubDomains: Specifies this rule applies to all of the site's     subdomains as well. Enabled by default.

## Configure HSTS Profile for SSL Interfaces

For SSL Interfaces a HSTS profile is not configured by default, thus disabling the feature. To enable HSTS, perform the following steps.

Create a new project “From existing configuration” and browse the directory of nodemanager config (apigateway/conf/configs.xml)

1. In Policy Studio, navigate to Environment Configuration > Listeners.
2. Select the interface you wish to configure, e.g. Node Manager > Management Services.
3. Select ports.
4. Right-click the HTTPS interface you wish to update, and select 'Edit'.
5. Navigate to the 'Advanced' tab.
6. Under 'HSTS Settings', select the HSTS profile you wish to use to protect all endpoints serviced by these interface.
   For example:

![To enable HSTS profile](/Images/docbook/images/general/hsts5.png)

**NOTE:** To apply configuration changes done in policy studio for nodemanager you need to copy the project folder config files to the apigateway/conf files.

For API Manager, Client Application Registry, Analytics , we need to add the HSTS profile in the same way as node manager port but under the Gateway ports they are on (for example, API Gateway > API Portal > Ports).

![To enable HSTS profile for API Gateway ports](/Images/docbook/images/general/hsts2.png)

Deploy the updated configuration to API Gateway after changing any HSTS profile settings. When you open the UI in a browser all the responses from the interface contains “Strict-Transport-Security" header with max-age and includeSubDomains.

**NOTE:** When HSTS is enabled, it is enabled for the domain irrespective of ports on listeners being configured. Any non-SSL ports that exist in the configuration (listeners) will no longer work from the browser once a HSTS-protected interface has been invoked from the browser.

**NOTE:** HSTS is made redundant when self-signed certificates are employed. By default the following ports use self-signed certificates: API Manager UI(8075), API Manager traffic (8065), API Gateway Manager UI (8090), Analytics (8040) and Client Application Registry (8089).