# Perform2020
for Perform 2020


## Prerquisites
Create a Paas and Api Token in your Dynatrace cluster and edit the creds.json accordingly, dont forget to alter the url to your tenant as well
Make sure your kubectl config is properly set up to connect to your K8s cluster

## Instructions

1. Run 1-deployDynatraceOperator.sh to deploy the OneAgentOperator in your K8s cluster. Check if everything is up and running:
    ```
    kubectl get pods -n dynatrace 
    ```
    
2. Run 2-setupSockShop.sh to install the SockShop that we will use for our example
    kubectl get services -n production and wait for the public ip to be set and note it down
3. Run 3-installAnsible.sh to install Ansible Tower it will also install some job templates that we need for our handson training.
    If this script shows that some resources are missing execute kubectl delete ns tower and run 3-installAnsible.sh again
    The script will show you the name of the job template we want dynatrace to trigger so write that url down
4. Create tags in Dynatrace via Settings -> Tags -> Automatically applied tags
    Create a tag named "app" with the Rule "Services get value '{ProcessGroup:KubernetesContainerName}' on process groups where Kubernetes container name exists"
    Create a tag named "environment" with the Rule "Services get value '{ProcessGroup:KubernetesNamespace}' on process groups where Kubernetes namespace exists"
5. Create a Problem notification in Dynatrace via Settings->Integrations-> Problem notifications -> Ansible
    Name: "Ansible Tower"
    template Url: insert the url of Step 4 here
6. Login to your Ansible Tower Interface and Invoke the Campaign job template, be aware to set the 0 to a Number higher than 20 and launch the campaign
    This will cause 20 % of the requests to fail
7. Create the Requests using the add-to-cart.sh script like 
    ./add-to-cart.sh %ip_of_your_sock_shop_from_step_2%
8. Watch the self-healing take place ;)
