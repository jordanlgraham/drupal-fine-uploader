drupal-fine-uploader
====================

Here are the steps required to install and configure the Fine Uploader library on your server:

FineUploader configuration


Tutorial: http://blog.fineuploader.com/2013/08/16/fine-uploader-s3-upload-directly-to-amazon-s3-from-your-browser/


1. Create bucket in Amazon 
2. Create a bucket policy / or edit existing
			{
				"Version": "2008-10-17",
				"Statement": [
					{
						"Sid": "Stmt1397069460111",
						"Effect": "Allow",
						"Principal": {
							"AWS": "*"
						},
						"Action": "s3:*",
						"Resource": "arn:aws:s3:::trivids_streaming/*"
					}
				]
			}

		For upload only, change Action statement to s3:putObject
		To restrict to a certain user, change Principal tag:
			e.g.:	"AWS":"arn:aws:iam::147272306770:user/vishnu"



3. Edit CORS configuration

		<code><?xml version="1.0" encoding="UTF-8"?>
		<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
		    <CORSRule>
		        <AllowedOrigin>*</AllowedOrigin>
		        <AllowedMethod>POST</AllowedMethod>
		        <AllowedMethod>PUT</AllowedMethod>
		        <MaxAgeSeconds>3000</MaxAgeSeconds>
		        <ExposeHeader>ETag</ExposeHeader>
		        <AllowedHeader>*</AllowedHeader>
		    </CORSRule>
		</CORSConfiguration></code>


	Can restrict access by changing the parameter for AllowedOrigin - I think this is domain specific

4. Create IAM group / user
	a. Create group first
	b. Create permissions policy
			<code>{
			  "Version":"2012-10-17",
			  "Statement":[{
			     "Effect":"Allow",
			     "Action":"s3:PutObject",
			     "Resource":"arn:aws:s3:::trivids_streaming/*"
			   }]
			}</code>
	c. create user (select "users" in left sidebar menu)
	d. show user security credentials; download credentials as we can't get secret key again for that user
	e. add user to group
	f. use oplicy generator - select
		AWS Service: Amazon S3
		Actions: DeleteObject, GetObject and PutObject
		Amazon Resource Name: arn:aws:s3:::trivids_streaming/*

		click "add stement"
		click "continue"
		click "apply policy"

5. Clone fine uploader repo onto your machine
		"git clone https://github.com/Widen/fine-uploader.git"
6. Add node.js to your server if it's not already installed

Install the dependencies:

sudo apt-get install g++ curl libssl-dev apache2-utils
sudo apt-get install git-core (unless you already have it)
Run the following commands:

git clone git://github.com/ry/node.git
cd node
./configure
make
sudo make install

7. Navigate into the fine-uploader directory
8. npm install
9. Install grunt: sudo npm install -g grunt-cli
10. grunt build
11. Compiled files are in fine-uploader/_build
12. Move sample assets files into server 
	@TODO: figure out what's in those files
13. Get AWS-SDK
	a. git clone https://github.com/amazonwebservices/aws-sdk-for-php.git
	b. mv aws-sdk-for-php aws
	c. mv aws (into the folder from #12 so that it's even with index.html)
14. Customize index.html
	a. endpoint: replace with your bucket (e.g. trivids_streaming.s3.amazonaws.com)
	b. accessKey: replace with Access Key ID from credentials you downloaded in step 4-d
	c. edit s3/sign.php
		i.    Replace $serverPrivateKey with Secret Access Key downloaded in step 4-d
		ii.   Replace $serverPublicKey with Access Key Id downloaded in step 4-d
		iii.  Replace $expectedBucketName with bucket (e.g. trivids_streaming)
		iv. 	Edit $expectedMaxSize if necessary
15. Move stylesheet from _build folder so that it's even with index.html









