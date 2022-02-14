# Make Firefox automatically proxy to LIC on Mac or Linux

LIC's TheShed, SAP (Leave approval/request) and Jira require you to be on LIC Network. For Window user, LIC Workspace take care of that. But on Linux or MAC, you are on your own !!! This is more a problem now that we all work from home.

Work around: use SOCKS proxy into LIC network for those specific website.

## Requirement
* Firefox (because that the one I know)
* `ssh` login into `smokescreen.livestock.org.nz` 

## Setup Firefox with custom proxy setting.
* Create the following PAC file and save to `/absolute/path/of/your/choice/proxy.pac`:
```javascript
function FindProxyForURL(url, host) {
  if (    
    localHostOrDomainIs(host, "theshed.lic.co.nz") ||
    localHostOrDomainIs(host, "sap.livestock.org.nz") ||
    localHostOrDomainIs(host, "saperp.livestock.org.nz")
  ) {
    return "SOCKS 127.0.0.1:8080";
  } else {
    return "DIRECT";
  }
}
```
Note: `SOCKS 127.0.0.1:8080` this means that we are using local port `8080`.

* Firefox > Setting > Network setting 
* `Automatic proxy configuration URL` : `file:///absolute/path/of/your/choice/proxy.pac`

## Before going to LIC website 
Start your SOCKS5 Proxy with :
`ssh -D 8080 -l <your LIC 5 characters username> smokescreen.livestock.org.nz`

Then just navigate to Jira and Firefox should automatically proxy to your SOCKS server,while going directly for all other website

