# Overview
This is my learning path for AWS. 

# Phases
1. Manual
   * The intent here is to get comfortable with AWS and the interface. 
   * I want to keep a very simple project (static content) in order to get familiar with with AWS and setup my blog. 
   * All the work here will be manual and I wont have any automation setup. 
   * The end goal is understanding the little bit of how AWS works before I move onto automation.
2. CI
   * Moving beyond manual process. Understand some of the security constraints around the containers. 
   * Introduce Least privledged access to ensure the CI system is not acting on the root account.
3. Forestry
   * Moving to ForestryIO is the end-game for my blog. 
   * Once I can manage content via ForestryIO then the blog is setup in a good place. I understand S3 well enough to move on and let ForestryIO manage the content, hiding a lot of the deployment details for the content.
4. Future - Java, Docker, DB, Lambda
   * TODO

# Phase 1 Manual

Inspiration 
http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html

1. Setup a Hugo Site
   * This blog is about AWS, so I won't dwell on this. Follow the Quickstart guide to get a blog up and running in Hugo.
   * https://gohugo.io/getting-started/quick-start/
2. Setup A Domain via Route 53
  
  First step was choosing a domain where I wanted to host the website. Out of all the providers out there, I chose Route 53 to keep everything in one place with AWS. Registereing the domain was easy enough.
  * Services > Route 53 > Register Domain
  
  The hardest part in this step was finding a domain that was available. I really wanted whalley.com (or .ca) so that I could use sub domains for myself, and the rest of my family. Sadly that wasn't available so after a few other tries, I realized the domain for my email was available (swhalley) so I was able to register that. There isn't anything fancy here. Find a domain that is free and signup.
  
  With Route 53, you are automatically signed up for a "hosted zone" which isn't in the free tier. Cost isn't much (about $0.50/month) so not a big deal. IT was a surprise to me that there was a charge above and beyond the registration feee for the domain. You would think this would just be hidden in the registration cost. The hosted zone is used to manage the DNS entries and route traffic. Nothing changed here.

3. Get the Hugo Site deployed to S3 
 * Create a new bucket with the name of my domain
 * Properties > Static Website Hosting 
 * Permissions > Bucket Policy
 
 This is where I got stuck the first time when I tried this alone. I have had experience with similar policies (firebase) but it was not clear at first glance that I needed to fill this in. All I got was an unauthorized response when I tried to access the website. I assumed setting up the domain as a static website host would take care of the permissions for me. Felt like this screen offered little help and should have been something that was part of the inital setup steps for the bucket. The setup I ended up using came after a google search and I found this.
 http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html

Copy and paste later and the site was operational.  

4. Link Route 53 to S3
DNS records and network traffic in general is not my strong point. I don't have any complicated routing or anything to do so I just need the domain to point to my bucket. This guide from AWS was helpful.
http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/RoutingToS3Bucket.html

5. Cloud Front - Https

# Phase 2 Automation via CI
1. IAM Setup
Getting this setup wasn't too bad. Setup a new group with S3 admin rights. Called this travis. Then created an API user. who can not login to console.
Concern - admin via api seems risky? can't I change this to only allow write of files, but not changing configs.
2. Access Keys 
Once the user was setup access keys were sitting and waiting
3. Travis CI
This was a pain to get hugo to build in travis. There was no content on the site. just the base site. No blogs or about pages. This caused the build in travis to fail. The error was a bit cryptic. Can find those in the Travis logs. 

Getting keys generated for travis were not too bad
```
brew install travis
touch .travis.yml
travis encrypt secret_access_key="secret access key from aws" --add deploy
```
then open up the file and add the rest of the config
```
deploy:
  provider: s3
  access_key_id: 
  bucket: swhalley.ca
  skip_cleanup: true
  region: ca-central-1
  local_dir: public
  secure: <encrypted key>
```

https://docs.travis-ci.com/user/deployment/s3/
https://docs.travis-ci.com/user/encryption-keys
https://docs.travis-ci.com/user/environment-variables/
