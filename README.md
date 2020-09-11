# Google Cloud Practice Project


##Labs
* ### Lab 1
    #### Title - Google Cloud Fundamentals: Getting Started with Cloud Marketplace 
    #### Objectives
     * To launch a cloud solution using Cloud Marketplace
    
    #### Task 1 - Use Market Place to deploy a LAMP stack
     ##### Steps (Code)
     *  `gcloud deployment-manager deployments create my-dep --config lampstack.jinja` 
       - The configuration for the 
            lampstack is defined in the lampstack,jinja file which is used by the deployment manager.
         
    #### Task 2 - Verify your deployment
     ##### Steps (Code)
     * Change the current working directory to opt/bitnami `cd /opt/bitnami`
     * Copy the phpinfo.php file to a publicly accessible directory under the web server document root `sudo sh -c 'echo "<?php phpinfo(); ?>" > apache2/htdocs/phpinfo.php'`
     * Visit the url to view the page `http://[SITE_ADDRESS]/phpinfo.php`
         
    #### Screenshots
    ![Lab 1](screenshots/lab1.png)
* ### Lab 2
    #### Title - Google Cloud Fundamentals: Getting Started with Compute Engine
    #### Objectives
     * Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console
     * Create a Compute Engine virtual machine using the gcloud command-line interface
     * Connect between the two instances
   #### Task 1 - Create a virtual machine using the GCP Console
    ##### Steps (Code)
    * Create a VM instance  `gcloud compute instances create my-vm-1 --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=36480928317-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 --reservation-affinity=any`
  #### Task 2 - Create a virtual machine using the gcloud command line
   ##### Steps (Code)
    * `gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213"--subnet "default"`
  #### Task 3 - Connect between VM instances
    ##### Steps (Code)
     * Connect to my-vm-2 via SSH and ping my-vm-1  `ping my-vm-1`
     * Connect to my-vm-1 instance - `ssh my-vm-1`
     * Install Nginx server `sudo apt-get install nginx-light -y`
     * Confirm the web server is running `curl http://localhost/`
  
  #### Screenshot
  ![Lab 2](screenshots/lab2.png)
  
* ### Lab 3
    #### Title - Google Cloud Fundamentals: Getting Started with Cloud Storage and Cloud SQL
    #### Objectives 
     * Create a Cloud Storage bucket and place an image into it.
     * Create a Cloud SQL instance and configure it.
     * Connect to the Cloud SQL instance from a web server.
     * Use the image in the Cloud Storage bucket on a web page.
     
   #### Task 1 -  Deploy a web server VM instance
    ##### Steps (Code)
    * Create the vm instance and run the startup scripts `gcloud compute instances create bloghost --zone=us-central1-a --machine-type=n1-standard-1 --subnet=default --network-tier=PREMIUM --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --maintenance-policy=MIGRATE --service-account=622331546967-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=bloghost --reservation-affinity=any`
    * Set a firewall rule on the vm instance `gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server`
    
  #### Task 2 - Create a Cloud Storage bucket using the gsutil command line
   ##### Steps (Code)
    * Create a bucket `gsutil mb -l us-central1-b gs://$DEVSHELL_PROJECT_ID`
    * Copy an image to the bucket `gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`
    * Modify the access control list of the object in the bucket `gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`
    
  #### Task 3 - Create the Cloud SQL instance
     ##### Steps (Code)
     * Create an sql instance `gcloud sql instances create blog-db --tier=db-n1-standard-2 --region=us-central1`
     * Add a User account `gcloud sql users set-password root --host=% --instance [INSTANCE_NAME] --password [PASSWORD]`
     * Add networks to the sql instance connections `gcloud sql instances patch [INSTANCE_ID] --authorized-networks=[NETWORK_RANGE_1]`

 #### Screenshot
 ![Lab 3](screenshots/lab3.png)
