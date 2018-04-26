# k8s-monitoring-with-prometheus (Environment Azure)                                                                                                                                            
                                                                                                                                                                                                
Configuration files for setting up prometheus monitoring on Kubernetes cluster.                                                                                                                 
                                                                                                                                                                                                
we can reffer the below links and modified the .yaml file according to our requirements.                                                                                                        
                                                                                                                                                                                                
You can find the full tutorial from below links :                                                                                                                                               
 1. https://devopscube.com/setup-prometheus-monitoring-on-kubernetes                                                                                                                            
 2. http://blog.wercker.com/how-to-setup-alerts-on-prometheus                                                                                                                                   
 3. https://github.com/kubernetes/examples/tree/master/staging/volumes                                                                                                                          
                                                                                                                                                                                                
Statefull Prometheus deployment:                                                                                                                                                                
--------------------------------                                                                                                                                                                
If prometheus is using local storage for scraped data, the data is lost when the pod gets killed.                                                                                               
                                                                                                                                                                                                
To avoid this we can add and use remote storage option. Here we have used a file storage from Azure                                                                                             
and mounted it to the prometheus pod. The data scraped will then be loaded to Azurefilestorage.                                                                                                 
                                                                                                                                                                                                
In case of pod failure. A new pod will be created and the Azurefilestorage volume will be mounted to                                                                                            
the new pod.                                                                                                                                                                                    
                                                                                                                                                                                                
Requirements:                                                                                                                                                                                   
 1. cifs-utils.                                                                                                                                                                                 
 we can use the below command to install 'cifs-utils' in Ubuntu.                                                                                                                                
  * $sudo apt-get install cifs-utils                                                                                                                                                            
                                                                                                                                                                                                
we will need a storage account in Azure in same region and subnet on which the k8s-cluster exist.                                                                                               
Then we can create a secret by using below command.                                                                                                                                             
  * $kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=<...> --from-literal=azurestorageaccountkey=<...>                                                        
                                                                                                                                                                                                
This secret is later used to authenticate and mount the Azurefilestorage to the prometheus pod.                                                                                                 
                                                                                                                                                                                                
Steps to Deploy Prometheus:                                                                                                                                                                     
---------------------------                                                                                                                                                                     
1. Create a configmap for Prometheus "https://github.com/lokbahadurchhetri/k8s-monitoring-with-prometheus/blob/master/prometheus-config.yaml"                                                   
2. Then Deploy Prometheus "https://github.com/lokbahadurchhetri/k8s-monitoring-with-prometheus/blob/master/prometheus-alertmanager-deployment.yaml"                                             
3. Now Expose the Prometheus Deployment "https://github.com/lokbahadurchhetri/k8s-monitoring-with-prometheus/blob/master/prometheus-service.yaml"                                               
4. Get the exposedip "Kubectl get svc -n monitoring" of prometheus svc and browse it to get to the Prometheus Dashboard.                                                                        
                                                                                                                                                                                                
Alertmanager configuration:                                                                                                                                                                     
---------------------------                                                                                                                                                                     
The Alertmanager handles alerts sent by client applications such as the Prometheus server. It takes care of deduplicating, grouping, and routing them to the correct receiver integration such a
s email, PagerDuty, or OpsGenie. It also takes care of silencing and inhibition of alerts.                                                                                                      
                                                                                                                                                                                                
Steps to Deploy AlertManager:                                                                                                                                                                   
1. Use the following "https://github.com/lokbahadurchhetri/k8s-monitoring-with-prometheus/blob/master/alertmanager-config.yaml" for creating a configmap for alertmanager.                      
2. Then use this link "https://github.com/lokbahadurchhetri/k8s-monitoring-with-prometheus/blob/master/alertmanager-deployment.yaml" to deploy Alertmanager.                                    
3. Now we Expose the Alertmanager Deployment using kubectl "kubectl expose deployment alertmanager -n monitoring --type=LoadBalancer --name=alertmanager-svc"                                   
4. Then we get the exposed ip "kubectl get svc -n monitoring" and we can check the Dashboard with the http://Exposedip.9093/                                                                    
                                                                                                                                                                                                
                                                                                                                                                                                                
                                                                                                                                                                                                
                                                                                                                                                                                                
                                                                                                                                                                                                
Note : The above code is still in progress. (The alert manager is yet to be configured)                                                                                                         
