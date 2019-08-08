# AWS admin user + API access key

Ok let's create an AWS admin user with the AWS **IAM console** :

![](../.gitbook/assets/image%20%28176%29.png)

We'll give AWS console acces to this ZeFerby guy, so he needs a _**console password**_.

And we want him to be able to use SDK-based and third-party tools like _CloudBerry Explorer for Amazon S3_, so he'll need  _**programmatic access**_ as well.

Finally this guy is only a temporary user in one of my accounts, so i give him a simple password that will not live long...

![](../.gitbook/assets/image%20%28105%29.png)

Clicking on "**Next: Permissions**", we arrive at this screen where we click the "**Attach existing policies directly**" box, and we check the ✅ mark on the **AdministratorAccess** listed policy :

![](../.gitbook/assets/image%20%2863%29.png)

Let's go on with "**Next: Tags**". I don't use tags here, so I click "**Next: Review**" :

![](../.gitbook/assets/image%20%28185%29.png)

Here I check the details are what I want and click "**Create user**"...

![Newly created administrator IAM User](../.gitbook/assets/image%20%2831%29.png)

Now I **download** the \(ugly\) **credentials.csv** file for reference \(it contains the password and the programmatic Access "Key ID" + "Secret Key"\) **before clicking "Close" !**



### Adding / Removing access methods : 

You can **edit existing IAM Users** to add / remove / edit /renew access methods : console access with password, programmatic access key, etc...



