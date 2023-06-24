Link To Practise Test : https://kodekloud.com/topic/practice-test-pods-2/

1. How many pods exist on the system?
   Solution : kubectl get pods
2. Create a new pod with the nginx image.
   Solution : kubectl run nginx --image=nginx
3. How many pods are created now?
   Solution : kubectl get pods
   Count the number of pods (if any)

To get the system to tell you you can also do this

kubectl get pods --no-headers | wc -l
--no-headers should be obvious - output only the details.
wc is the word count program. -l tells it to count lines instead, and it will count the lines emitted by kubectl

4. What is the image used to create the new pods?
   Solution : kubectl describe outputs lots of information. The following will describe all pods whose name starts with newpods, and then we filter with grep to get what we are looking for.

kubectl describe pod newpods | grep image

5. Which nodes are these pods placed on?
   Solution : kubectl get pods -o wide
Note the node column for each of the 3 newpods pods

6. How many containers are part of the pod webapp?
 Solution : kubectl describe pod webapp
Look under the Containers section. Note there is nginx and agentx

7. What images are used in the new webapp pod?
 Solution :Examine the output from Q6. For each of the identified containers, look at Image:

8. What is the state of the container agentx in the pod webapp?
 Soluton: kubectl describe pod webapp
Examine the State: field for the agentx container.

9. Why do you think the container agentx in pod webapp is in error?
 Solution: Examine the output from Q8 and look in the Events: section at the end. Look at the event that says failed to pull and unpack image ...

10. What does the READY column in the output of the kubectl get pods command indicate?
 Solution: kubectl get pods
Look at the webapp pod which we know has two containers and one is in error. You can deduce which of the answers is correct from this.

11.Delete the webapp Pod.
 Solutions: kubectl delete pod webapp
To delete the pod without any delay and confirmation, we can add --force flag. Note that in a real production system, forcing is a last resort as it can leave behind containers that haven't been cleaned up properly. Some may have important cleanup jobs to run when they are requested to terminate, which wouldn't get run.

You can however use --force in the exam as it will gain you time. No exam pods rely on any of the above.

12. Create a new pod with the name redis and with the image redis123.
 Solution: Use a pod-definition YAML file.
To create the pod definition YAML file:

--dry-run=client tells kubectl to test without actually doing anything. -o yaml says "Output what you would send to API server to the console", which we then redirect into the named file.

kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml
And now use the YAML you created to deploy the pod.

kubectl create -f redis.yaml

13. Now change the image on this pod to redis.
Once done, the pod should be in a running state.
There are three ways this can be done!

 Solutions:
Method 1
Edit your manifest file created in last question

vi redis.yaml
Fix the image name in the redis.yaml to redis, save and exit.

Apply the edited yaml

kubectl apply -f redis.yaml
Method 2
Edit the running pod directly (note not all fields can be edited this way)

kubectl edit pod redis
