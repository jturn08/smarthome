# Push notifications from linux server to mobile (ex: Android & iOS)
This guide explains how to use free [https://ntfy.sh](https://ntfy.sh) service and Android or iOS mobile app to get push notifications from [OpenMediaVault](https://www.openmediavault.org) Linux media server events on your smartphone. In addition to sending email, OpenMediaVault supports running a custom script.  

## Overview
To get OpenMediaVault push notifications on your smartphone or tablet, you'll need to to install the free [https://ntfy.sh](https://ntfy.s) Android or iOS app, and then configure your OpenMediaVault server to send notifications to ntfy.sh service.  

## Install ntfy.sh Android or iOS app
Follow [instructions for installing ntfy Android or iOS app and subscribing to a topic from your phone](https://ntfy.sh/docs/subscribe/phone/).  
Choose a unique name for the topic you subscribe to as this is equivalent to combining username + conversation topic. **Note:** All notifications sent to ntfy.sh are publicly viewable. There's no password, and anyone subscribed to the topic will receive push notifications.  

## Configuring OpenMediaVault server to send notifications to ntfy.sh service
Create a custom [third party notification](https://docs.openmediavault.org/en/6.x/administration/general/notifications.html?highlight=notifications#events) script for OpenMediaVault in `/usr/share/openmediavault/notification/sink.d` folder (ex: `/usr/share/openmediavault/notification/sink.d/22ntfy`)  

The custom script must follow these rules
* Script filename must start with a number like this: `22pushnotification`
* Script filename does not have extension, otherwise it gets excluded  

1. Create script file `nano /usr/share/openmediavault/notification/sink.d/22ntfy`
2. Paste script contents below, set `TOPIC` variable to your desired topic name, and save the file. 

```bash
#!/bin/bash
TOPIC="mytopicname"
URL="https://ntfy.sh/${TOPIC}"

# Include only first 4 lines from message subject
MSG_CONTENT=$(head -n 4 "${OMV_NOTIFICATION_MESSAGE_FILE})")

send_message () {
    curl \
    -H "X-Title: ${OMV_NOTIFICATION_SUBJECT}" \
    -d "${MSG_CONTENT}" \
    "$URL"
}

send_message
```
3. Set the script file to be executable by running `chmod +x /usr/share/openmediavault/notification/sink.d/22ntfy`)