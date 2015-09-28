# [Splunk Yammer Modular Alert](https://github.com/oxo42/SplunkYammerAlert)

## Overview

This is a Splunk Modular Alert for posting alerts to a Yammer group.

## Dependencies

* Splunk 6.3+
* Supported on Windows, Linux, MacOS, Solaris, FreeBSD, HP-UX, AIX
* Active Office 365 Yammer subscription

## Setup

* Download the release
* `$SPLUNK_HOME/bin/splunk install yammer_alerts.spl -update 1`
* Restart Splunk

## Office 365 Setup

* Create a user for Splunk to send as the alerts
* Ensure that user can post to all the groups that you want

## Yammer Auth token

_Okay, this is horrible, but it works.  There is an [issue on GitHub](https://github.com/oxo42/SplunkYammerAlert/issues/1) to fix this._

### Yammer

Because I haven't yet created a proper auth process in Splunk, you'll have to work around it.  We'll create a client application then manually go through the process of getting a `code` then the `token`.  These instructions are taken from the [Yammer Dev Docs - Test Token](https://developer.yammer.com/docs/test-token)

1. Create a [Yammer Client Application](https://www.yammer.com/client_applications)
  * Set the `Redirect URI` to https://www.yammer.com/
2. Make a note of the `Client ID` and `Client Secret`
3. Paste this URL into your browser where the `client_id` is the value obtained above
```
https://www.yammer.com/oauth2/authorize?client_id=[client_id]&redirect_uri=https://www.yammer.com/
```
4. Copy the `code` parameter from the URL created
5. Paste this URL into your browser, where `client_id`, `client_secret` are obtained in step 3 above and the `code` comes from step 4
```
https://www.yammer.com/oauth2/access_token.json?client_id=[client_id]&client_secret=[client_secret]&code=[code]
```
6. From the JSON object returned, copy the `token` field

### Splunk GUI

1. Settings -> Alert Actions -> Yammer Alerts -> Setup Yammer Alerting
2. Paste the token from step 6 above into the text box

### Splunk Config File

Edit or create the file `$SPLUNK_HOME/etc/apps/yammer_alerts/local/alert_actions.conf`

```ini
[yammer]
param.token = <token from yammer.com>
```

## Using

Perform a search in Splunk and then navigate to : Save As -> Alert -> Trigger Actions -> Add Actions -> Yammer Alerts

On this dialogue you can enter the `group_id` to post to and `body` to send.

For the body field, token substitution can be used just the same as for [email alerts](http://docs.splunk.com/Documentation/Splunk/latest/Alert/Setupalertactions#Tokens_available_for_email_notifications)

## Logging

Browse to: Settings -> Alert Actions -> Yammer Alerts -> View Log Events

Or you can search directly in Splunk

```
index=_internal sourcetype=splunkd component=sendmodalert action="yammer"
```

## Troubleshooting

1. Is your `group_id` correct?
2. Are your alerts actually firing?
3. Is your auth token correct?

## Contact

This project was initiated by John Oxley, john.oxley@gmail.com
