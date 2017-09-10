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
1. Setup a Hugo Site
2. Setup A Domain via Route 53
3. Get the Hugo Site deployed to S3 
4. Cloud Front - Https

# Phase 2 Automation via CI
1. IAM Setup
2. Access Keys 
3. Travis CI
