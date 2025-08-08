# Create and share Instant Connect Meeting Links

## Create Meeting Links

Similarly to what we did previously in the **Webex Instant Connect API** section , here we will create Webex Instant Connect meeting links.  However, instead of using curl we will use an **HTTP Request** node. 

1. Add a new **HTTP Request** node to the canvas, and connect it to the  **Delay** node orange dot

- Configure the node as shown below:

    ![Creating Meeting Links](images/create-meeting-links.png)

    Here you have some of the values that you can copy-paste:

    - Endpoint URL:
        ```
        $(mtg-broker-url)/api/v2/joseencrypt
        ```
    - Body: 
        ```
        {
            "jwt": {
            "sub": "Video Call with your practitioner - $(n2.whatsapp.timestamp)"
            },
            "aud": "a4d886b0-979f-4e2c-a958-3e8c14605e51",
            "provideShortUrls": "true",
            "verticalType": "hc",
            "loginUrlForHost": false
        }
        ````

        With the option `verticalType` = `hc` , Instant Connect user interface will be automatically customized for a Healthcare use case.

- Rename the node to 'Create meeting links' and **Save** it

3. Add a **Data Parser** node to the canvas, and connect it to the previous **HTTP Request** node

- To get the value for **Input: Import Data From**, chooe the `http.response.body` variable from the `Create Meeting Links` **HTTP Request** node configured in the previous step:

![Data Parser node setup](images/data-parser-ic.png)

> **Note:** Node numbers may be different.

- For the **Sample Body**, you can use the response body you got with curl, ot use the http request node _Test_ option.

    Here you have on example that you can copy and paste:
    ```
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

- Click on **Parse**

- In this case you do not need to **Select** and **IMPORT** all the variables, `$.baseUrl`, `$.host[0].short` and `$.guest[0].short` are enough.

    Set the **OUTPUT VARIABLE NAMES** as shown below:

    ![alt text](images/data-parser-ic-vars.png)

    And **Save** the node

4. Next, we will build variables containing the host and guest URLs. We can use again the embedded capabilities of Webex Connect nodes for this:

- Edit the  **Data Parse** node we just created, and go to the **Transition Actions (Optional)** tab.

- As we did in **Webex Connect Flow: Meeting Scheduling** section, add an Action and create two **Custom Variables**: `guestURL` and `hostURL`.

    The URL values are created by concatenating the `baseUrl` and the values in the `short` and `guest` keys on node leave.
        
![Variable Creation](images/create-vars-ic-links.png)

   > **Note:** Node numbers may be different.

## Send link to customer

1. Copy any of the SMS nodes in the flow, copy and paste it, and connect it to the previous **Data Parser** node

2. Change the **Message** to:
    ```
    Hi $(customerName), you can join your video appointment in this link: $(guestURL)
    ```

3.  Rename the node to 'Offer guest URL to customer' and **Save** it


## Send link to expert

Next we will use the [Webex messaging API](https://developer.webex.com/docs/api/v1/messages/create-a-message) to send a message from your Webex bot to the expert.

1. Choose your expert

    Go to Custom Variables, and type the expert email address. The Webex message will be sent to this address

    In a real project, normally there will be a process or system in place to choose the best expert for a given customer or interaction. In this case, the expert is a static value.

2. Add an **HTTP Request** node to the canvas, and connect it to the previous **SMS** node.

    - Configure the node as shown below:

        ![HTTP Request for Webex](images/http-request-webex.png)

        Here you have some of the values that you can copy-paste:

        - Body:
            ```
                {
                    "toPersonEmail":"$(expertEmail)",
                    "text":"$(customerName) is waiting for you. Please click on this link to join the video session: $(hostURL)"
                } 
            ```
    
    - Rename the node to 'Offer host URL to expert' and **Save** it

    Mission accomplished! Learn how to integrate Webex Connect with Instant Connect to create the Instant Connect Meetings Links, and share them with the customer and the expert.

    ## SECTION FOR NON US ATTENDEES

    If you do not have a US mobile number, you can still test your flow.

    1. Go to the _Channel Selection_ **Branch Node**, and connect the 'Webex' outcome to the **HTTP Request Node** named _Reminder-Webex Channel_   


        ![Webex Option](images/webex-option-1.png)

    The Webex AI Agent has been added in this WEbex flow branch for you, you do not need to to it again.