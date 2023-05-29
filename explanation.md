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

We create a plabook.yaml file and populate it;

1. Every playbook must start with ---
2. we set our hosts machine to all . Thi will help ansible pick up the virtual machine that spinned up by vagrant in our virtualbox
3. we set become to true so we are able to run our commands as sudo in our virtual machine.
4. We set a pre_tasks that updates our apt cache and give it an interval of 3600 seconds. this will ensure that everytime we run our playbook, ansible will be installing the latest versions of whats defined in our plays, from our apt repository.
5. We set our var_tasks to point to our vars.yml file. In this file, we will declare variables to be used in our playbook.
6. We can now start defining our tasks.
We need to configure our vm by setting up an environment to deploy our application. For that we need;
   a. Install the git package. This will allow us to be able to git clone our project from git hub.
   b. install dependancies and update cache. This are the dependancies needed to aid in installation of the various softwares we will require for the project.
   c. installing docker and docker-compose.
      1. add gpg keys from docker website, then we add a docker repository to apt, then we install docker using apt by installing *docker-ce, docker-ce-cli,containerd.io*.
      2. we make sure that docker is running and will run on booting by setting the state to started and enabled to yes.
      3. Install docker compose python package by usisng ansible.builtin.pip and install the docker python module to enable ansible to run docker compose up.
    d. Install mongo db in our virtual machine.
      1. import public key from mongodb.org
      2. Add a reporitory in apt and update the apt cache
      3. install mongodb using apt. the apt package name is mongodb-org
      4. Ensure that mongodb is running and will run on booting by setting the state to started and enabled to yes.


 By this point our virtual machine is ready to receive our containirised application and run it.
 We clone the repository using the ansible.builtin.git module and our destination to /usr/local/src/yolo and set our state to present.
 finaly we run our docker compose by setting our play as shown below 

 - name: Run docker-compose
      docker_compose:
       project_src: /usr/local/src/yolo
       state: present

The last step is to check the website via the link http://192.168.56.0:3000/ add a product and confirm that the data persists.

Point to note, to run the playbook use the command vagrant up to create the VM and vagrant provision to initiate the plays.

# App Deploment to GKE

### Preriquisites
1. Docker
2. GCP account
3. credit card to activate the 90days free account.

### Steps configurations 

1. Create a project in your GCP account
2. Configure your compute region and zone
3. Create your cluster with atleast 2 nodes where your pods will be created and run.
4. Install gcloud CLI in your local terminal to gain access to the cluster you just created. https://cloud.google.com/sdk/docs/install
5. Install Kubectl https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

### Git Clone
Clone this repository to your machine.
`git clone https://github.com/AlbertineN/yolo.git`

### Execution
1. Navigate to the yolo project folder `cd /path/to/project`
2. Run the command `gcloud init` to log in to the gcloud CLI
3. On the terminal run `kubectl config get-contexts` to see the clusters you have. 
4. On the terminal run `kubectl config use-context <name of your cluster>` to make sure you are working on the right cluster

### set up StatefulSet for our mongodb
Run the comand `kubectl create -f manifest/mongodb/pvc.yaml -n development` to create a persistent volume claim within our cluster
Run the comand `kubectl create -f manifest/mongodb/scn.yaml -n development` to create a storage class within our cluster
Run the comand `kubectl apply -f manifest/mongodb/priorityclass.yaml -n development` to set up a priorityClass.
Run the comand `kubectl create -f manifest/mongodb/StatefulSet.yaml -n development` to create our pod
Run the comand `kubectl apply -f manifest/mongodb/service.yaml -n -development` to expose our pod externaly on port 27017

to cofirm if our pod is up and running run the comand `kubectl get pods -n development`

### Set up our backend app
Run the command `kubectl create -f manifest/backend/yolobackdep.yaml -n development` to create the pod
Run the comand `kubectl apply -f manifest/backend/service.yaml -n -development` to expose our pod externaly on port 5000







