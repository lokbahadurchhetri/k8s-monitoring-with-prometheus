# k8s-monitoring-with-prometheus

Configuration files for setting up prometheus monitoring on Kubernetes cluster.

we can reffer the below links and modified the .yaml file according to our requirements.

You can find the full tutorial from below links :

1.https://devopscube.com/setup-prometheus-monitoring-on-kubernetes
2.http://blog.wercker.com/how-to-setup-alerts-on-prometheus

Statefull Prometheus deployment:
--------------------------------
If prometheus is using local storage for scraped data, the data is lost when the pod gets killed.

To avoid this we can add and use remote storage option. Here we have used a file storage from Azure
and mounted it to the prometheus pod. The data scraped will then be loaded to Azurefilestorage.

In case of pod failure. A new pod will be created and the Azurefilestorage volume will be mounted to
the new pod.

Requirements:
1. cifs-utils
we can use the below command to install 'cifs-utils' in Ubuntu.
 * $sudo apt-get install cifs-utils

we will need a storage account in Azure in same region and subnet on which the k8s-cluster exist.
Then we can create a secret by using below command.
 * $kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=<...> --from-literal=azurestorageaccountkey=<...>

This secret is later used to authenticate and mount the Azurefilestorage to the prometheus pod.


Note : The above code is still in progress. (The alert manager is yet to be configured)
