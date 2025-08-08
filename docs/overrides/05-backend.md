# Backend Preparation

One of the objectives of this lab is to demonstrate how Webex Connect can retrieve and update relevant business information from backend systems such as databases, CRMs or EMRs.

You have access to a mock up database for every POD where you can provision your own customers

## Access your Customer Database

Go to this [link](https://appointment-management-production-095a.up.railway.app/database.html), use your POD number (for example _POD4_) and an attendee email address (for example _studentpod4@email.com_)

> NOTE: this attendee email address can be any, it won't be used later in the flow. It is used to keep POD's information isolated: you will have access only to your own customers, other lab attendees won't see your customer data. Do not share this email address with other lab attendees !!

## Create a customer

Click **Add New Customer**, type **Name**, **Email** and **Mobile Number**. **Phone** and **Company** are not mandatory and won't be used during the lab, you can add some values for demo purposes.

Example:

- Name = `Customer Pod4`
- Email = `customerpod4@email.com`
- Mobile Number = `12225556666`

**Email** and **Mobile Number** have to be real, they will be used later when we work on the Webex Connect Flow.

## Check your Database record

You should be able to get your new customer details using this API request:

```
curl --request GET \
  --url 'https://appointment-management-production-095a.up.railway.app/api/customers/search/?mobileNumber=12225556666&email=customerpod4%40email.com' \
  --header 'content-type: application/json' \
  --header 'x-pod-id: POD4' \
  --header 'x-user-email: studentpod4@email.com'

```

Replacing '12225556666' by your customer  **Mobile Number**, 'customerpod4%40email.com' by your customer **Email**, and studentpod4@email.com by your attendee email address.

You should get and answer like this:

```
{
   "podId":"POD4",
   "userEmail":"studentpod4@email.com",
   "adminMode":false,
   "searchCriteria":{
      "email":"customerpod4@email.com",
      "mobileNumber":"12225556666"
   },
   "searchScope":"user_customers",
   "matchingCustomers":[
      {
         "id":"791b0fe8-b5aa-4e18-8eb6-2076d3d20d8c",
         "name":"Customer Pod4",
         "email":"customerpod4@email.com",
         "phone":"",
         "mobileNumber":"12225556666",
         "company":"",
         "status":"active",
         "createdBy":"studentpod4@email.com",
         "createdAt":"2025-07-28T20:39:38.171Z",
         "updatedAt":"2025-07-28T20:39:38.171Z"
      }
   ],
   "totalMatches":1,
   "totalSearched":1
}
```


