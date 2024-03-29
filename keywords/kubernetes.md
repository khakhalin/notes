# Kubernetes

Parents: [[system_design]]
See also: [[docker]], [[flask]], [[microservice]], [[aws]], [[ci]]

#systems #tools


Kubertnetes is an open-source system (originally developed by Google) for coordinating [[docker]] containers on the cloud (something that is called **orchestration**). It operates **pods** that each contains several **containers**, that are placed together, within the same localhost. Pods can communicate with each other via pod IP. Kubernetes distributed containers across pods and also **nodes** (physical machines), allocates resources to them, automatically restart them if they fail, make sure that there's more than one instance of those containers that need to run in duplicate, and maintain load balancing across instances (see [[system_design]]).

# Deploying

🔥🔥🔥


# Environment variables and Secrets

Environment variables are shared with kubernetes as a [[yaml]] file, or files (typically peope have a separate file for each environment, as you will probably have different databases involved in your `dev` and `prod` environments, among other things). Traditionally, these yaml files are stored in a `k8s` folder.

There are two ways to store a value in these yaml files: openly (unincoded) or secretly (encoded). To create a simple (unsealed) yaml, do this:

1. Authenitate with [[aws]] to get access to the `kubectl` context. For example, if you use okta to authenticate, do something like `aws-okta select <name of your space>`.
2. Created a generic yaml file with unsealed secrets using this script:

```bash
kubectl create secret generic <name_of_secrets_collection> \
 --from-literal=SECRET_NAME='Really-secREt-vAlue' \
 --dry-run -o yaml > secret-dev.yaml
 ```
At this point, if the secret value is written in normal text, it is just represented in the file as-is. If the string has some special characters in it, it is encoded (by a string that is slightly longer than the original string), but not encripted.

To created a **sealed secret** stay authenticated with the space, for which the secrets are designed, and run the following command:
```bash
kubeseal --controller-namespace sealed-secrets \
--controller-name sealed-secrets \
--scope namespace-wide \
<  secret-dev.yaml --format yaml > sealed-secret-dev.yaml
```
Note that here you really need to connect with your `dev` environment to create your dev secrets, with your `prod` environment to seal your prod secrets etc. This info _is_ encoded in the string, together with the scopes. The encrypted string is quite longer than the original string (about 3 full screen widths).

Once you have a sealed value, you can either use this very sealed secrets file, or, if you have a template for a secrets file from some other source, you can just copy the sealed value from one file to another. Then place it into a `k8s` folder and push to origin. Typically, people set up [[ci]] in such a way that secrets are auto-deployed at pod deployment (but not pod restart). According to stackoverflow, there may be no way to "redeploy" the secrets with a command; you need to redeploy the pod.

To see secrets already on a cluster, log in, then run `kubectl get sealedsecrets`. It shows their age of a collection in days. It does NOT however show how long ago the collection was updated.

To learn the content of a sealed secret, log into the pod (with `kubectl exec -it pod_name -- bash`; see below), and then run `echo $SECRET_NAME` there (use the actual name of the variable; the one used in yaml files).

# Monitoring and troubleshooting

First log in to [[aws]] (or whatever provider than you use).

`kubectl get pods` - See what pods are running. Also shows pod age, and the number of instances for each pod. Note that at least in some cases (🔥 when?) kubernetes actually orchestrates the number of instances via a different mechanism, so it seems like all pods have only one instance, but actually they all look a bit like `meaningful_name-sd8fs-s8df8s0df98`, so some "generic name", followed by a hash-like autogenerated code that turns it into a "precise name". I'm not yet sure how it works exactly 🔥 , but it seems that some commands take a generic name, while some need a precise name (or maybe all names are "equipped" with a trailing wildcard at interpretation stage? 🔥 not sure)

`kubectl logs specific_pod_name` - See logs (those produced by a logger, see [[logging]]). The pod name can be taken from the pods list above. Note that the name in the pods list has a constant part (the beginning), and a unique part (the end) that is different for different instances, and changes when the pod restarts (?). For the log command you need the full version of the name (constant and variable), as you want to see logs of a particular instance of a pod. For some other commands below you only need the constant part, as you are scaling the number of pods, not a specific instance. (🔥 not sure the terminology is correct here)

If a pod fails, you can try to summarize the reasons with `kubectl describe pods my_pod_name`

**SSH into a pod:** `kubectl exec -it pod_name -- bash`

One useful reason to ssh into the pod is to see the values of hidden secret encoded environmental variables if they are mentioned in the code as `os.environ['MY_TABLE']` constants. Which is typical, as you want to keep them as env variables so that you could run the same code in different environments (dev / int / prod), and for security reasons they may be obfuscated (aka "sealed-secrets"). So to learn them you should enter the pod, and then do this:
```python
python3
import os
for (k,v) in sorted(os.environ.items()): print(k,v);
```

**Restart pod**: `kubectl delete pods pod_name_precise`. It deletes it indeed, but then kubernetes immediately restarts it. The name needs to be a precise one (?🔥 ) as you're killing an instance.

**Turn off a pod**: `kubectl scale deploy pod_name_generic --replicas=0`. This is a cool not-quite-destructive operation, as the pod stil remains deployed, it just no longer has any running instances. Which means that doing `kubectl scale depoly pod_name --replicas=2` can bring instances back (2 instances in this case). Scaling down from 2 to 1 or vice versa may also be helpful sometimes (during troubleshooting, de-[[gridlock]]ing etc.). Note also that keys can go in any order (useful when manually rescaling several pods at once). 🔥 _is there an even more automated command for scaling down all pods, or all pods by one, or something like that?_

# Other tools

Apparently some people vouch by the k9s tool as the best possible interface to kubernetes: https://k9scli.io/topics/commands/

# Refs

https://www.oreilly.com/library/view/kubernetes-cookbook-2nd/9781788837606/c71faba3-73be-48db-89b9-7754625e98eb.xhtml