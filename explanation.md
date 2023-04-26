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


