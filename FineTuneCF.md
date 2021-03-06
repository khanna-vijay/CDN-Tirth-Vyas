## Fine Tune your CloudFront distribution further

**Reduce number of HTTP 404s using Custom Error Response/Page**
-	In your CloudFront distribution, navigate to Errors tab. In the HTTP error code, notice how it is configured to send HTTP 200 OK response back to the client. Amazon CloudFront is going to mask HTTP 404 error with HTTP 200OK and serve clients a static page (200.htmlo) instead. 

**Using Custom Domain Name**
-	If you have a valid domain registered with a DNS provider, you can add your custom domain (Alternate Domain) to the CNAME/Alternate field in the CloudFront setting. 

**Free HTTPS/SSL Integration**
-	You can make use of SSL certificate in your CloudFront distribution to secure your requests for your custom domain name. For example, www.example.com is a custom domain for your website that is fronted by Amazon CloudFront. You can make use of Amazon ACM service to get a free SSL certificate. SSL certificates issued through Amazon ACM are free. If you have a custom third-party certificate, you can easily import it in ACM, and use it with your Amazon CloudFront distribution.

**Using AWS WAF/Shield to mitigate DDoS attacks**
-	Amazon WAF/Shield integration with Amazon CloudFront is simple. You can block variety of requests based on your requirements. AWS Shield can protect your website from DDOS attacks. 
