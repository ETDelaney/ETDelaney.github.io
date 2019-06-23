---
layout: post
title: Getting started with the Google Cloud Platform
---

I had the opportunity to attend a course that would introduce me to practical aspects of cloud computing - from the building of small test models in small virtual machines to deploying such models and making them accessible to anyone - within the framework of the Google Cloud Platform. The session was taught by Benoit Dherin, a machine learning solutions engineer at Google's Advanced Solutions Lab (ASL). These posts will serve mainly as documentation for my own purposes with no audience in mind except for myself. Nonetheless, others may find these notes useful. 


![_config.yml]({{ site.baseurl }}/images/GCP-snip.PNG)




### Steps to getting started:
1. To begin with, a user should make sure he or she is in incogito mode to ensure the correct the correct account (and not a personal account) is billed.


2. The user should then log into gmail using the following setup: <firstname>.<lastname>@asl.apps-eval.com
  where '<' and '>' are not included in the adderss.
  
  
3. In another incognito window, visit **http://events.qwiklabs.com** and sign-up on QwikLabs using the email address above


4. Go back to you email address to confirm and log into QwikLabs

![_config.yml]({{ site.baseurl }}/images/qwiklabs-login.png)


5. You should be able to find a course called ML Immersion


6. And then you should be able to find: **Lab 0: Getting started in Qwiklabs**


7. Press the green button 'Start Lab' and now you should see you have 40'ish days to complete the lab:
  From this point forward, **NEVER PRESS THE RED BUTTON 'End Lab'** - which will result in the destruction of the lab and you will everything that you have progressed on within the GCP.
  
  
8. After you have started the lab, you will have received a username, password, and Project ID - following the instructions, you will eventually be led to going to the Google Cloud Platform at: **http://console.cloud.google.com** and log into the cloud using that username and password provided by QwikLabs

![_config.yml]({{ site.baseurl }}/images/qwiklabs-login-to-GCP.png)


9. Now that you have logged into the GCP, make sure that you have your project is correct selected (which should match the Project ID found on Qwiklabs - just select from the dropdown menu) and then after that you can open the Google Cloud Shell (looks like a shell icon):

![_config.yml]({{ site.baseurl }}/images/GCP-projectid-shell.png)


10. We now need to create a Deep Learning virtual machine, which can be done by copying and pasting the following code into the shell. Make sure that you modify the second line of the code to match you qwiklabs email address:


```bash
INSTANCE_NAME=dlvm
GCP_LOGIN_NAME=YOURQWIKLABSEMAILADDRESSHERE@QWIKLABS.NET
gcloud config set compute/zone us-west1-a
gcloud compute instances create ${INSTANCE_NAME} \
--machine-type=n1-standard-8 \
--scopes=bigquery,cloud-platform,userinfo-email \
--min-cpu-platform="Intel Skylake" \
--image-family=tf-latest-gpu \
--image-project=deeplearning-platform-release \
--boot-disk-size=100GB \
--boot-disk-type=pd-ssd \
--accelerator=type=nvidia-tesla-p100,count=1 \
--boot-disk-device-name=${INSTANCE_NAME} \
--maintenance-policy=TERMINATE \
--restart-on-failure \
--metadata="proxy-user-mail=${GCP_LOGIN_NAME},install-nvidia-driver=True"
```

11. If everything has gone well, you should be able to click on the 'hamburger' icon on the top left, select AI Platform, and then Notebooks. Here you will find the instance called DLVM and you can select **'OPEN JUPYTERLAB'**. What is amazing is that at any given time, you may stop the instance and swithch in and out CPU, RAM, GPU to achieve what you need.


![_config.yml]({{ site.baseurl }}/images/GCP-aiplatform-notebooks.png)


12. Once we have launched JupyterLab, we need to clone a repository from GitHub - select the git clone icon:

![_config.yml]({{ site.baseurl }}/images/jupterlab-git.PNG)

and clone the following repository:
  **https://github.com/GoogleCloudPlatform/training-data-analyst.git**
 
 
13. Once you have cloned the repository, select the folder training-data-analyst to go into it and select Git icon on the left-hand side and make sure you are not working on the master branch, but on origin/asl-oslo:

![_config.yml]({{ site.baseurl }}/images/git-head-switch.PNG)


14. Everything is now set up! We can now and start on the first notebook, located at: 
  **training-data-analyst/courses/machine_learning/deepdive/01_bigquery/labs/a_sample_explore_clean.ipynb**

  Inside the labs folders are the exercises, whereas one folder directory up are the solutions.
