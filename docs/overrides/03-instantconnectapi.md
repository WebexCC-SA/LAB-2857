# Webex Instant Connect API

Now, let's build Instant Connect (IC) meeting links programmatically.

## Get an Access Token

You will need a Webex API access token to make calls to the IC API. You have two options:

* Your personal token for development purposes.

* A bot token for production use.

In this lab we will use a bot token.

## Create your BOT
1. Go to https://developer.webex.com/docs/bots, log is as a Webex administrator in your org, and click on **Create a Bot**

2. Select your Bot name (for example 'Video Expert'), and an icon of your choice. 

3. Choose your bot username, this has to be unique. For example, you could do something 'lab-wx1-_yourusername_'

4. **App Hub Description**: we won't be publishing this bot to App Hub, but it is a good practice to add a meaningful description that will help you remember the purpose of the bot. For example, you can type something like '_This is the bot used for my wx1 lab 2857: 'Webex Connect with Instant Connect for video customer interactions_'

5. Click on **Add Bot** and **Copy your token!!** to a safe place, we will use later:

![Bot token](images/bot-token.png)

> Note: Remember that you need to use a Guest-to-Guest Webex Meeting site, as described in this lab Introduction

## Create the Meeting Links

This section will provide meeting links for host (expert) and guest (end customer) users. You will use curl for this:

1. Open a Terminal session

2. Copy/paste and run this command:

    ```
    curl --location 'https://mtg-broker-a.wbx2.com/api/v2/joseencrypt' \
    --header 'Content-Type: application/json' \
    --header 'Authorization: Bearer ZWJkZjc1Y2UtN2U3MS00MzkxLTljMzYtM2Q4MjNiYjczMDVmMjE2NjFjODctNDZm_P0A1_3e826b27-2854-4cef-90d7-47862c8eef5a' \
    --data '{
        "jwt": {
            "sub": "Instant Connect Meeting 5"
        },
        "aud": "a4d886b0-979f-4e2c-a958-3e8c14605e51",
        "provideShortUrls": true,
        "verticalType": "hc",
        "loginUrlForHost": false
    }'
    ```
    Body details:

   * `sub` (Subject) string value can be whatever you like as long as it is unique for each meeting.

   * `aud` indicates the audience for which the jwt is intended. In this case it is Cisco, and the value is always the same.

   * `jwt` with `sub` and `aud` are mandatory parameters, the rest are optional.

   * `provideShortUrls`: Default: `false`. If set to `true`, the response will have shortened data portions of the meeting URL. It will also contain a shortened base URL, you will learn later how to use this data.

   * `verticalType`: Default: `hc`. Currently takes two values, `gen` for general flow, and `hc` for healthcare flow.

   * `loginUrlForHost`: Default `true`. Relevant only if `provideShortUrls` is true. If set to `false`, the short URL for hosts will be non-login links which means the host won't have an option to login for the meeting.

    API response should be something like this:
    ```js
    {
        "host": [
            {
                "cipher": "eyJwMnMiOiJpNmZta3dp...cWl3ZGw2cjFuSkg0bEUj",
                "short": "oCVp2LD"
            }
        ],
        "guest": [
            {
                "cipher": "eyJwMnMiOiJEQVdaHOBS...RDTvlZ-aLLRdIMSmCwEc",
                "short": "ckmNR7I"
            }
        ],
        "baseUrl": "https://instant.webex.com/visit/"
    }
    ```
    > **Note:** Response has been formatted to make the documentation more understandable

    If you are not comfortable using the command line, there are other tools that you can use to create the request, such as Postman or Bruno, or https://httpie.io/app.

## Construct the Meeting URLs

Forming the meeting links is as simple as taking the `baseUrl` value and concatenating the values in `short`.

In this example, `https://instant.webex.com/visit/oCVp2LD` for the host and `https://instant.webex.com/visit/ckmNR7I` for the guest.

Now you only need to share the host URL with the expert providing support, and the guest URL with the end customer asking for support !!

This can be done by email, SMS, be integrated into some web portal, CRM, EMR, etc. In the next steps, you will learn how to do it in a digital channel like WhatsApp using Webex Connect.




