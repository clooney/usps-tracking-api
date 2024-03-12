# TrackingMore: Webhook
The TrackingMore webhook delivers near real-time notifications for shipment status changes to a specified URL, allowing for customized status alerts. 

How to Set Up the Webhook
--------
* Create your own webhook URL.
* Access [TrackingMore’s admin](https://www.trackingmore.com/signup.html).
* Add webhook URL under developer setting (Max. 4 URLs per account).


Webhook Secure
--------
TrackingMore supports either HTTP or HTTPS urls, so you can have security by using an SSL-enabled url. But keep in mind that your endpoint is going to be wide-open on the internet.

Retry Webhook
--------
TrackingMore sends event driven data to webhook URL via POST method. In case of an unsuccessful event (HTTP response code NOT between 200 and 299), TrackingMore attempts to deliver your webhooks for up to 14 times with an exponential back off.

The current attempt webhook delay is calculated by this formula: 2^(number of retry) x 30s

For example, If the attempt fails, TrackingMore will retry the 2nd attempt 30s later. If the 7th attempts fail, TrackingMore retry the 8th attempt 960s later If the 14th attempts fail, TrackingMore will not send out that webhook any more.
