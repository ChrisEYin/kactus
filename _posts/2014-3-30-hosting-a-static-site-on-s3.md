---
title: Hosting a Static Site on S3
layout: post
tags:
- code
---

I've had this setup with [my personal site](http://www.chrisyin.com) for some time now, but I recently had to setup another static site and I had to look through my old setup and Google around a bit and try to figure out how to do it again. Figured I would write it down this time.

1. Buy A Domain
2. Create a Bucket on S3
3. Upload Content
4. Configure Bucket
5. Point Domain to Bucket

--

**Buy A Domain** 

There's a ton of places to do this (people seem to use [Namecheap](https://www.namecheap.com) a lot), but I stick with Godaddy because all my domains are already here, and they keep giving me [$0.99 domains](http://www.godaddy.com/deals2/). Anything works.

**Create a Bucket on S3**

If you don't have an S3 account, go to [AWS](http://aws.amazon.com) and sign up. It's free for 5 GB storage, 20K Get Requests, 2K Put Requests, and 15GB of data transfer out each month for one year. For most, it will be free but if you think you'll do more than that, their pricing is [here](http://aws.amazon.com/s3/pricing/). 

Once you're in the S3 console, click 'Create Bucket' in the top left hand corner. All your files will be stored here. Name your bucket 'www.yourdomain.com'. For Region, just leave the default 'US Standard' and then click 'Create'.

![createbucket](/images/staticsite/createbucket.png)

**Upload Content**

Once you have your bucket, time to upload. You can do either via UI or command line. 

To upload via UI, click Upload in the top left hand corner and select or drag folders in. Once your files have been uploaded, right click the files and select 'Make Public'.

![makepublic](/images/staticsite/makepublic.png)

The easier and faster way is to use [s3cmd](http://s3tools.org/s3cmd), a command line tool for S3. Once you setup and configure s3cmd with your access keys, find your bucket and upload with below.

```
$ s3cmd ls # find your bucket in the list
$ s3cmd sync --acl-public folder_name/ s3://www.domain.com

# example for folder '_site' and bucket www.chrisyin.com
$ s3cmd sync --acl-public _site/ s3://www.chrisyin.com
```

**Configure Bucket**

Return to the main S3 console and highlight your bucket (but don't go into it). Select 'Properties' in the top right hand corner. The 2 sections we need to configure are 'Permissions' and 'Static Website Hosting'. 

![bucketproperties](/images/staticsite/bucketproperties.png)

In Permissions, click 'Add More Permissions', select 'Everyone' and check 'View Permissions'. Click Save in the bottom right.

![everyonepermissions](/images/staticsite/everyonepermissions.png)

Then open up 'Static Website Hosting'. In here, select 'Enable website hosting', define your index/404 files, and then click Save. There's also URL next to 'Endpoint:'' - that is the URL for your static site. If you click on it, it should show your the site. If that's working, now we'll point your domain to that URL.

![index404](/images/staticsite/index404.png)

**Point Domain to Bucket**

Go into your domain registrar. Everyone's steps now are going to be different depending oin your registrar. If you use Godaddy, click on 'My Account' --> 'My Domains', select your domain and then 'Launch'.

Under the domain info and next to 'Settings', select 'DNS Zone File' and then click 'Edit' -- change CNAME record with Host:www and point it to your Endpoint URL.

![cname](/images/staticsite/cname.png)

Then click back into the main setting and then for forwarding, set your forwarding to *www.domain.com*. Done!

Now both *domain.com* & *www.domain.com* should work for your site.





