### API KEY AND API SECRET

<BR> 

In your views.py file, update your client using API_KEY and API_SECRET
```python
client = razorpay.Client(auth=("<YOUR_API_KEY>", "<YOUR_API_SECRET>"))
```

<br>

### Payment Links
<hr>
Payment Links are URLs that you can send to your customers through SMS and/or email to collect payments from them. Customers can click on the URL, which opens the payment request page, and complete the payment using any of the available payment methods.
There are two types of Payment Links:

1. Standard Payment Links: You can make payments using netbanking, cards, wallets, UPI and bank transfer payment methods using Standard Payment Links.
2. UPI Payment Links: You can select the UPI app of your choice to make payments using UPI Payment Links.



### Create Payment Link
```Python
import razorpay
client = razorpay.Client(auth=("YOUR_ID", "YOUR_SECRET"))

client.payment_link.create({
  "amount": 500,
  "currency": "INR",
  "description": "For XYZ purpose",
  "reference_id": "TS1989",
  "customer": {
    "name": "Gaurav Kumar",
    "email": "gaurav.kumar@example.com",
    "contact": "+919999999999"
  },
  "notify": {
    "sms": True,
    "email": True
  },
  "callback_url": "https://example-callback-url.com/",
  "callback_method": "get"
})
```

> **_Note_** : Payment Link will be valid for six months from the date of creation. Please note that the expire by date cannot exceed more than six months from the date of creation.

The query parameters will be added to url as

    https://example-callback-url.com/?razorpay_payment_id=pay_Fc8mUeDrEKf08Y&razorpay_payment_link_id=plink_Fc8lXILABzQL7M&razorpay_payment_link_reference_id=TSsd1989&razorpay_payment_link_status=partially_paid&razorpay_signature=b0ea302006d9c3da504510c9be482a647d5196b265f5a82aeb272888dcbee70e

<br>

### Verify Payment Link signature
<hr>
Verify the razorpay_signature parameter to validate that it is authentic and sent from Razorpay servers.

The razorpay_signature should be validated by your server.
<br>

- payment_link_id
- payment_link_reference_id
- payment_link_status
- razorpay_payment_id 
- razorpay_signature
> **_Note_** : All these values can be found as payload from request

```python
import razorpay
client = razorpay.Client(auth=("YOUR_ID", "YOUR_SECRET"))

client.utility.verify_payment_link_signature({
   'payment_link_id': payment_link_id,
   'payment_link_reference_id': payment_link_reference_id,
   'payment_link_status':payment_link_status,
   'razorpay_payment_id': razorpay_payment_id,
   'razorpay_signature': razorpay_signature
   })
```

<br>

### Fetch Payment Links
Fetch All
```python
import razorpay
client = razorpay.Client(auth=("YOUR_ID", "YOUR_SECRET"))
client.payment_link.all()
```

Fetch Specific Payment Links by ID
```python
import razorpay
client = razorpay.Client(auth=("YOUR_ID", "YOUR_SECRET"))
client.payment_link.fetch(paymentLinkId)
```
<br>

## Webhooks
<hr>
Webhooks are used to get notified about events related to the Razorpay payment flow such as orders, payments, settlements, disputes and workflow steps related to other Razorpay Products.

Webhooks (Web Callback, HTTP Push API or Reverse API) are one way that a web application can send information to another application in real-time when a specific event happens.

e.g order.paid, payment.failed, payment.captured, etc.

Razorpay Webhooks can be used to configure and receive notifications when a specific event occurs. When one of these events is triggered, we send an HTTP POST payload in JSON to the webhook's configured URL.

> **_Note_** : In webhook URLs, only port numbers 80 and 443 are currently allowed.

<br>

### Webhook Configuration 

Inside [Razorpay Dashboard](https://razorpay.com/) navigate to Settings > Webhooks > Add New Webhook.

1. Enter the URL where you want to receive the webhook payload when an event is triggered. We recommended using an HTTPS URL.
2. Enter a Secret for the webhook endpoint. The secret is used to validate that the webhook is from Razorpay. Do not expose the secret publicly(Secret is recommended, not compulsory. It doesn't need to be RAZORPAY_SECRET_KEY). 
3. In the Alert Email field, enter the email address to which the notifications should be sent in case of webhook failure. You will receive webhook deactivation notifications to this email address.
4. Select the required events from the list of Active Events.


> **_Note_** :
> - All webhook responses must return a status code in the range 2XX within a window of 5 seconds.If response codes other than this is received or the request times out, it is considered a failure. 
> - On failure, a webhook is re-tried at progressive intervals of time, defined in the exponential back-off policy, for 24 hours. If the failures continue for 24 hours, the webhook is disabled. 
> - Webhooks can only be delivered to public URLs

<br>

### Test Webhooks

1. On Localhost

- You cannot use localhost directly to receive webhook events as webhook delivery requires a public URL. 
- You can handle this by creating a tunnel to your localhost using tools such as ngrok or localtunnel. 

- Install [ngrok](https://ngrok.com/)
 : `sudo snap install ngrok`

- Create account 
    * Navigate to Dashboard > Getting Started > Your Authtoken and Copy the authtoken
    * Configure ngrok by writing <b>ngrok config [authtoken-value]</b> command in your terminal. 
    * Generate a url(Hosted url) by <b>ngrok http 8000</b>. This will give an url which has https, Use the URL endpoint generated in the webhook URL while setting up your webhooks in Razorpay Dashboard.
2. Deployed
- Set the url for Webhook url in Razorpay Dashboard (no need of tunneling)

### Validate Webhooks

- When your webhook secret is set, Razorpay uses it to create a hash signature with each payload. This hash signature is passed with each request under the X-Razorpay-Signature header that you need to validate at your end.

Webhook Example
- Let’s say you’ve registered to receive the payment.captured event and a customer clicks the “Pay” button in your app or website. A webhook between Razorpay and your app tells your app whether the customer’s payment is successful or not. After your webhook endpoint receives the payment.captured event, your webhook function can then run backend actions as per your logic. Using an API for this workflow is like calling the API every millisecond to ask, was the payment successful?

> **_NOTE_** : For event handler, the webhook has POST request which will be executed first and not the get request.

```python
import razorpay
client = razorpay.Client(auth=("YOUR_ID", "YOUR_SECRET"))
client.utility.verify_webhook_signature(webhook_body, webhook_signature, webhook_secret)
```
Parameters

* webhook_body : can be obtained from payload. The body must be passed in raw request format encoded with utf-8 as string

* webhook signature : Can be obtained from request.headers['X-Razorpay-Signature']

* webhook_secret : Enter you webhook secret that you entered while creating webhook on dashboard









