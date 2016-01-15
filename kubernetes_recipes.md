# Kubernetes Recipes

###Launch a single container

Problem: I want to launch a container, not write a Pod or Replication Controller file.

Solution: Use `kubectl run`, for example like so:
```
$ kubectl run asimplewebserver --image=nginx
```

###Bootstrap a Replication Controller file

Problem: How can I bootstrap a Replication Controller (RC) manifest file?

Solution: Use `kubectl run` with the `--dry-run` argument:
```
$ kubectl run asimplewebserver --image=nginx --output=yaml --dry-run > asimplewebserver-rc.yaml
```

Note that above command will output the RC into `asimplewebserver-rc.yaml` which you then can edit to your liking.

###Using Secrets

Problem: I want to provide a username and password to a container, how can I do that in a secure way?

Solution: Use Kubernetes Secrets, for example, as described in this blog post that has a step-by-step description of the steps needed.

###Debugging using labels

Problem: I noticed one of the pods has gone bad. Can I debug it online?

Solution: Yes, by using labels. Assume you mark up your running pods with `status=serving` and when you want to debug one of the pods without impacting the service, just re-label the bad pod to `status=troubleshooting` and the RC will kick in to launch a new one, instead. Also, you have the bad pod for yourself to dissect. See also the walkthrough example here for a step-by-step instruction:

**That's our RC:**
```
$ cat ws-rc.yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: webserver-rc
spec:
  replicas: 5
  selector:
    app: webserver
    status: serving
  template:
    metadata:
      labels:
        app: webserver
        env: prod
        status: serving
    spec:
      containers:
      - image: nginx:1.9.7
        name: nginx
```
**Now let's fire up some pods:**
```
$ kubectl create -f ws-rc.yaml
replicationcontrollers/webserver-rc
```

**All pods serving traffic, yes? Let's have a look:**
```
$ kubectl get pods --selector="status=serving"
NAME                 READY     STATUS    RESTARTS   AGE
webserver-rc-baeui   1/1       Running   0          18s
webserver-rc-dgijd   1/1       Running   0          18s
webserver-rc-ii79i   1/1       Running   0          18s
webserver-rc-lxag2   1/1       Running   0          18s <-- THIS ONE GONE BAD
webserver-rc-x5yvm   1/1       Running   0          18s
```

**OIC one pod, `webserver-rc-lxag2`, has gone bad. Let's isolate it:**
```
$ kubectl label pods webserver-rc-lxag2 --overwrite status=troubleshooting
NAME                 READY     STATUS    RESTARTS   AGE
webserver-rc-lxag2   1/1       Running   0          45s
```

**And how many pods do we now have serving traffic (remember, I asked the RC for 5 replicas):**
```
$ kubectl get pods --selector="status=serving"
NAME                 READY     STATUS    RESTARTS   AGE
webserver-rc-baeui   1/1       Running   0          49s
webserver-rc-dgijd   1/1       Running   0          49s
webserver-rc-ii79i   1/1       Running   0          49s
webserver-rc-pwst1   0/1       Running   0          4s <-- BACKUP ALREADY UP AND RUNNING
webserver-rc-x5yvm   1/1       Running   0          49s
```

Sweet! Within 4s the RC has detected that I took the bad pod `webserver-rc-lxag2` offline and has launched a backup somewhere. Now, how's my guinea pig doing?
```
$ kubectl get pods --selector="status!=serving"
NAME                 READY     STATUS    RESTARTS   AGE
webserver-rc-lxag2   1/1       Running   0          1m <-- HERE'S MY GUINEA PIG
```

Here is the bad pod `webserver-rc-lxag2` that I can now live-debug, without impacting the overall operation.
```
view rawREADME.md hosted with ❤ by GitHub
```

###Multiple commands in container

Problem: Can I specify multiple commands for a container in a pod?

Solution: Yes, for example like so:
```
command: ["/bin/sh","-c"]
args: ["first_command ; then_second_command && third_command_only_if_second_succeeded"]
```

With command: `["/bin/sh", "-c"]` above you specify a shell to execute in the container that gets the arguments listed in `args: [ ... ]` passed in. In the above case, the first command as well as the second would always be executed and the third command only if the second command successfully executed (that is, a return value of 0). See also these two StackOverflow posts: [1], [2].

###Access pod label from within one of its container

Problem: I want to access the labels of a pod from within one of its containers.

Solution: Use the Downward API to achieve this. This API enables containers to query infos about themselves (as well as about the overall system state). For an example see this docs page.