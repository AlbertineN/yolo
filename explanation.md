Step one
Went through the instructions in the README.md file to test and see if the application works, which it does well on port 3000 for the ecomerce yolo website and port 5000 for the mongo database. 

Step two 
Create a Dockerfile for the client image that will create a docker container in which it will run in. 

Step Three
Create a Dockerfile for the backend image that will create a docker comtainer in which the backend will run in. 

Step Four
Create a docker-compose.yml file to orchestrate our e-commerce platform. In this file, we will also pull a mongodb image as our backend requires a mongodb database. 

Step Five
Run the docker compose up command to build images run them. Upon running the command, i realize that my client container exits with no errors before the deployment server starts.

Step Six
Added tty: true  >> this comand ensures that my container does not exit but rather keeps running. However I come across another error which is brought about  by the start script. updtaed the start script to  *"start": "react-scripts --openssl-legacy-provider start",* This solved my issue as my development server could now start. 

Step Seven
Accessed the website on *http://localhost:3000* tested my connections by adding a product. the product was added succesfully. Stopped my containers and run *sudo docker compose down*  to stop and remove my containers. run sudo docker compose up to create fresh containers from my images and upon accessing the site, the product I added was still there meaning I set my volumes well and my data persisted. 

Step Eight
pushed my docker images to docker hub

# How to deploy ans application to a vagrant virtual Machine usuing ansible

Preriquisites

1. Have Vagrant installed in your local machine or control node
2. Have ansible installed in your local machine or control node
3. Have virtual box installed in your local machine and disable secure booting in your boot menu.


First thing to do is configure your vagrant file in the root directory of the application you want to deploy. In my case the yolo directory. In your vagrant file, specify the virtual machine image to pull and initiate *geerlingguy/ubuntu2004* give it a host name *yolo.test*, a private network ip *192.168.56.0*, disable the ssh insert key by setting it to false  assign some memory to it *around 1500mb to 2000mb will do* last but not least configure an ansible provisioner so we can run our playbook with vagrant by providing the playbook to be run. In our case playbook.yaml.


