# Splunk Yammer Modular Alert

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

_I am pretty sure this is the wrong way to do things.  I don't know how to control OAuth2 from within Splunk.  There is an [issue on
GitHub](https://github.com/oxo42/SplunkYammerAlert/issues/1) to fix this._

### Yammer

1. Visit the  [Yammer Developer Portal](https://developer.yammer.com/docs/messagesjson)
2. Try one of the sample APIs
     * This forces you to log into Yammer via OAuth2 and exposes the token.
3. Click on the icon of the key and copy it


### Splunk GUI

1. Settings -> Alert Actions -> Yammer Alerts -> Setup Yammer Alerting
2. Paste the token into the text box

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
