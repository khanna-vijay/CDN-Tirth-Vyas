## Serving custom content to end users based on the device type

**Prepare our backend (S3 origin) for this lab**

-	In this case, we are using an S3 bucket as our origin. We’re going to create two folders (prefixes) inside the root of the bucket, “mobile” and “desktop”.
-	Download CloudFront Map images to your local machine from: 
  https://github.com/stuffbyt/re-Invent2019BuildersSession/blob/master/CloudFrontMapSmaller.png
  and https://github.com/stuffbyt/re-Invent2019BuildersSession/blob/master/CloudFrontMap.png
-	Upload CloudFrontMapSmaller.png into your mobile folder and CloudFrontMap.png to your "desktop" folder. 
-	Now, navigate inside your S3 bucket’s "mobile" folder and rename the image CloudFrontMapSmaller.png to CloudFrontMap.png so that both images have the same name. 
-	Now, using Lambda@Edge, we’re going to serve custom images to desktop users and mobile users based on the User-Agent header. 
-	Note that CloudFrontSmaller.png is a compressed image for mobile websites. 
-	What’s awesome about this solution is that you can keep the same front end facing URL for two different images.

**Create a Lambda Function**

-	Login to your AWS Console and in the search bar, look for Lambda: https://console.aws.amazon.com/console/home?region=us-east-1
-	Click on Create Function and then select Author from Scratch
-	Give your function a name and select runtime nodejs 10.x 
-	For Permissions, select, Use an existing role. Choose the role that you created in previous exercise 
-	Now that your function is created, go ahead and copy the code from and paste it in the editor: https://github.com/stuffbyt/re-Invent2019BuildersSession/blob/master/DeviceDetection.js
-	Once the code has been successfully copied, go ahead and save your code. To save your code version, click on “Save” button on the top rightmost screen.

**Update your CloudFront Distribution**

-	Switch to Amazon CloudFront and select your distribution. Click on Behaviors tab. 
-	Select Default Cache Behavior (*)
-	Scroll down to Cache Based on Selected Request Headers and select, Whitelist
o	CloudFront-Is-Desktop-Viewer
o	CloudFront-Is-Mobile-Viewer
-	Scroll down and select: Yes, Edit.

**Deploy your Lambda code to CloudFront edge locations**

-	If you scroll up from the code editor, you should see Designer field where you would see an option to add a trigger. 
-	Click on Deploy to Lambda@Edge and select Amazon CloudFront as a trigger. In this step, you would have to select your CloudFront distribution ID that you can find in the CloudFormation stack's output tab. 
-	In the Cache Behavior, select * (That’s the default cache behavior)
-	In the CloudFront event, we will select Origin Request. We want to send reqeusts to different folders in the backend depending on the user-agent header. 
-	Acknowledge the message and click on Deploy.
-	Once the function is deployed, give it a couple of minutes and then test the HTTP Response Headers using CURL utility. 

**Time to test our solution**

• *Passing Mobile device user-agent to mimic a mobile device from our machine*:
curl -v https://dvd8yendmgqle.cloudfront.net/CloudFrontMap.png -H "User-Agent: Mozilla/5.0 (Android 7.0; Mobile; rv:54.0) Gecko/54.0 Firefox/54.0" >/dev/null

•	*For desktop view, perform the following curl*
curl -v https://dvd8yendmgqle.cloudfront.net/CloudFrontMap.png >/dev/null


**How do we know the solution we just built works?**

Examine HTTP response headers for the curl commands you just issued.

In the curl command you issued for mobile devices, you should see the following Content-Length header:

*content-length: 52004* ~52KB

In the curl command for desktop i.e. the one without the explict User-Agent, you should see the following Content-Length header:

*Content-Length: 162637* ~162KB

What all this means is that for the same URL, you're getting served different images (smaller and regular sized) based on the User-Agent in the request. 
