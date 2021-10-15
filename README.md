# 14848-Mini-Project

## Major Steps
A. Got all programs and codes from GitHub:
   https://github.com/rinormaloku/k8s-mastery
   
B. Inside k8s-mastery folder, build container image for sa-frontend, sa-logic and sa-webapp according to README files accordingly.
   for instance, to build sa-webapp image:
   1. install maven manually (mac users)
   2. run mvn install
   3. run java -jar sentiment-analysis-web-0.0.1-SNAPSHOT.jar --sa.logic.api.url=http://localhost:5000
   4. build the container by running: docker build -f Dockerfile -t elizxiong/sentiment-analysis-web-app .
   5. run and test if the container can be run successfully:
      get the container ip from sa-logic container, and run the container: docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://172.17.0.3:5000' elizxiong/sentiment-analysis-web-app 
   6. push the container to Docker Hub using: docker push elizxiong/sentiment-analysis-web-app 
   <img width="745" alt="Screen Shot 2021-10-14 at 5 28 45 PM" src="https://user-images.githubusercontent.com/60122319/137398195-d112a719-db45-40ec-a2b9-2758642d47d1.png">

C. create project on GCP, enable container registry, and deploy Docker Images pushed to Docker Hub to Google's container registry, for instance

   $ docker pull elizxiong/sentiment-analysis-web-app
   
   $ docker tag elizxiong/sentiment-analysis-web-app gcr.io/mini-project-328902/elizxiong/sentiment-analysis-web-app:latest
   
   $ docker push gcr.io/mini-project-328902/elizxiong/sentiment-analysis-web-app:latest
   
   <img width="705" alt="Screen Shot 2021-10-14 at 5 35 21 PM" src="https://user-images.githubusercontent.com/60122319/137398865-d1df5de7-d12e-45ca-86d1-0a831c0853d3.png">

D. create a Kubernetes cluster:
   $ gcloud container clusters create --machine-type n1-standard-2 --num-nodes 2 --zone us-central1-a --cluster-version latest miniprojectcluster

E. Deploy container images to GKE

<img width="1059" alt="Screen Shot 2021-10-14 at 5 37 57 PM" src="https://user-images.githubusercontent.com/60122319/137399152-92ba07d3-168a-4c21-963e-d0a173344e9f.png">

F. Configure and edit yaml, add port and env information.
   for sa-web-app-deployment, add following to spec: container:  
  env:
    - name: SA_LOGIC_API_URL
      value: "http://{sa_web_app_ip}/5000"
  ports:
    - containerPort: 8080

G. Expose deployments
  
<img width="1266" alt="Screen Shot 2021-10-14 at 5 46 38 PM" src="https://user-images.githubusercontent.com/60122319/137400098-e1b169dd-d094-4e7c-8fd2-7fd5566e3582.png">

F. Check and run the app on external endpoints


## Docker Hub Images URLs
1.	https://hub.docker.com/repository/docker/elizxiong/sentiment-analysis-frontend

    docker pull elizxiong/sentiment-analysis-frontend

2.	https://hub.docker.com/repository/docker/elizxiong/sentiment-analysis-logic

    docker pull elizxiong/sentiment-analysis-logic

3.	https://hub.docker.com/repository/docker/elizxiong/sentiment-analysis-web-app

    docker pull elizxiong/sentiment-analysis-web-app
    
## Project Video

Please see a short walkthrough presentation video of this project in the folder: https://github.com/ELizXiong/14848-Mini-Project/blob/main/Sentiment-Analysis/project%20walkthrough.mp4 

If the file cannot be viewed properly, you can also watch it on YouTube: https://youtu.be/sZXyRLO-yUY 
      
