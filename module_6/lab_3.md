# A view of your running EC2 instance Details page after completing the Lab 3 tasks

![](./pics/Screenshot%202023-09-01%20at%203.54.05%20PM.png)

# A view Lambda Monitor view after your Lambda function has been completed and invoked in the Lambda Activity.

![](./pics/Screenshot%202023-09-01%20at%204.17.47%20PM.png)

# A view of your Elastic Beanstalk Configuration (Task 2 - Step 14) from the Elastic Beanstalk Activity.

![](./pics/Screenshot%202023-09-01%20at%204.26.48%20PM.png)

EC2 allows users to rent virtual servers in the cloud, giving them access to different applications. Elastic Beanstalk allows users to upload their application and AWS will manage deployment. And Lambda allows users to run code without the need for dedicated servers. Instead of continuously running, Lambda executes code in response to events.

In the lab we demoed the ability to host a website through a virtual server using EC2. For Elastic Beanstalk we uploaded a tomcat file containing an application and it was easily displayed. We also used lambda to automatically turn off a specific instance every minute. This concept could be used for automation within monitoring and controlling different commands at certain times or events. So EC2 gives a virtual server to manage how a user wants, Elastic Beanstalk handles infrastructure tasks allowing for just application uploading, and Lambda only executes when specific events occur. 

EC2 is IaaS, Elastic Beanstalk is Paas, Lambda is Paas