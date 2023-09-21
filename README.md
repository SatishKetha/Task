# Task
1. Dockerized the node js web application and pushed the image to the Docker Hub.

       docker build . -t satish2797/node-web-app
       docker run -p 3000:3000 -itd satish2797/node-web-app
       docker login (give username and password)
       docker push satish2797/node-web-app

3. Created an EKS cluster on AWS Cloud Platform using Terraform and also created the work station and installed kubectl on it to interact with the cluster.

4. Installed AWSCLIv2 on the work station instance and downloaded the kubeconfig file using the below command.

        aws eks update-kubeconfig --region <region_name> --name <cluster_name>

5. Created  the kubernetes deployment manifest i.e., deployment.yaml to deploy our node js application to the EKS Cluster and applied it using the below command.

       kubectl apply -f deployment.yml

6. Added the Prometheus helm chart repository and implemented the cluster monitoring using Prometheus and Grafana. Note: Changed the default service type of Prometheus and Grafana services from ClusterIP to 
   LoadBalancer to access them from the browser.

   a. Install  helm chart repository

       curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
       chmod 700 get_helm.sh
       ./get_helm.sh

   b.Create a dedicated namespace for prometheus

        kubectl create namespace monitoring

   c.Add Prometheus helm chart repository

       helm repo add prometheus-community https://prometheus-community.github.io/helm-charts 

   d.Update the helm chart repository
      
        helm repo update
        helm repo list

   e.Install the prometheus

         helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring

   f.Above helm create all services as ClusterIP. To access Prometheus out side of the cluster, we should change the service type load balancer

         kubectl edit svc prometheus-kube-prometheus-prometheus -n monitoring

   g.Login to Prometheus dashboard to monitor application https://ELB_DNS:9090

   h.Check for node_load15 executor to check cluster monitoring
    
   i.We check similar graphs in the Grafana dashboard itself. for that, we should change the service type of Grafana to LoadBalancer  

         kubectl edit svc prometheus-grafana -n monitoring

   j.To login to Grafana account, use the below username and password  

        username: admin
   
        password: prom-operator

   k.Here we should check for "Node Exporter/USE method/Node" and "Node Exporter/USE method/Cluster" USE - Utilization, Saturation, Errors
    
   l.Even we can check the behaviour of each pod, node, and cluster

   
   
