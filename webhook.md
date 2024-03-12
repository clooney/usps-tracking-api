# TrackingMore: Webhook
The TrackingMore webhook delivers near real-time notifications for shipment status changes to a specified URL, allowing for customized status alerts. 

## Webhook Overview
### How to Set Up the Webhook
* Create your own webhook URL.
* Access [TrackingMore’s admin](https://www.trackingmore.com/signup.html).
* Add webhook URL under developer setting (Max. 4 URLs per account).


### Security Practices
TrackingMore supports either HTTP or HTTPS urls. Opt for HTTPS to secure data transmissions. 

Note, your endpoint will be public.

### Retry Mechanism
TrackingMore sends event driven data to webhook URL via POST method. In case of an unsuccessful event (HTTP response code NOT between 200 and 299), TrackingMore attempts to deliver your webhooks for up to 14 times with an exponential back off.

The current attempt webhook delay is calculated by this formula: 2^(number of retry) x 30s. For example, If the attempt fails, TrackingMore will retry the 2nd attempt 30s later. If the 7th attempts fail, TrackingMore retry the 8th attempt 960s later If the 14th attempts fail, TrackingMore will not send out that webhook any more.

## Webhook Specification
### Webhook Signature
Check for TrackingMore's signature to verify all webhook notifications.

All webhook notification header has a calculated digital signature for verification. The signature is a base64-encoded HMAC generated using sha256 algorithm with webhook request body and webhook secret of your account.

Each webhook request could be verified by comparing the computed HMAC signature and the attached signature in header.

### How to compute HMAC signature
Step 1: Get Webhook Secret

The following Webhook Secret is used for compute signature.

~~~
2e55b9a3-4dd1-4416-9897-c4bd1e3d738f
~~~

Note: Webhook Secret is only for V4 version Webhook.

Step 2: Compute HMAC signature by using Sha256

The following Golang example demonstrates the computation of a webhook signature.

~~~
WEBHOOK_SECRET := "2e55b9a3-4dd1-4416-9897-c4bd1e3d738f"
timestamp := "1662371528"

func GenerateSignature(apiKey, timestamp string) string {
	h := hmac.New(sha256.New, []byte(WEBHOOK_SECRET))
	h.Write([]byte(timestamp))
	return hex.EncodeToString(h.Sum(nil))
}

// HMAC signature: a37084ab68ae16b77db1f8463f31be9fcc965e2515e03efecf8139bb1e511b06
~~~

Step 3: Verify signature
Compare the computed HMAC signature and the attached signature in header.

