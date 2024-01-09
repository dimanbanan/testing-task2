# Тестовое задание а позицию DevOps junior»

### Для выполнения задания использовал microk8s, т.к. kubeadm не поддерживает network policies, а kubespray долго ставится. Также использовал yandex cloud для развертывания виртуалки с помощью terraform. Для подов использовал образы nginx и network-multitools.

```
ubuntu@server-kube:~$ micro config set-context --current --namespace=web
Context "microk8s" modified.
ubuntu@server-kube:~$ micro get po -o wide --all-namespaces
NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE     IP            NODE          NOMINATED NODE   READINESS GATES
kube-system   calico-node-l7xlg                         1/1     Running   0          43m     10.0.3.25     server-kube   <none>           <none>
kube-system   coredns-864597b5fd-qw69m                  1/1     Running   0          43m     10.1.55.193   server-kube   <none>           <none>
kube-system   calico-kube-controllers-77bd7c5b-6g2b4    1/1     Running   0          43m     10.1.55.194   server-kube   <none>           <none>
ingress       nginx-ingress-microk8s-controller-n2bvw   1/1     Running   0          42m     10.1.55.195   server-kube   <none>           <none>
app           nginx-686bfc6c75-92st4                    2/2     Running   0          7m21s   10.1.55.196   server-kube   <none>           <none>
db            nginx-686bfc6c75-q9gk4                    2/2     Running   0          7m21s   10.1.55.198   server-kube   <none>           <none>
web           nginx-686bfc6c75-4g995                    2/2     Running   0          7m21s   10.1.55.197   server-kube   <none>           <none>
ubuntu@server-kube:~$ micro get ns
NAME              STATUS   AGE
kube-system       Active   43m
kube-public       Active   43m
kube-node-lease   Active   43m
default           Active   43m
ingress           Active   42m
app               Active   42m
db                Active   42m
web               Active   42m
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-4g995 curl 10.1.55.196
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   370k      0 --:--:-- --:--:-- --:--:--  600k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-4g995 curl 10.1.55.198
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
ubuntu@server-kube:~$ micro config set-context --current --namespace=app
Context "microk8s" modified.
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-92st4 -- curl 10.1.55.198
Defaulted container "nginx" out of: nginx, multitool
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   336k      0 --:--:-- --:--:-- --:--:--  600k
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-92st4 -- curl 10.1.55.197
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
ubuntu@server-kube:~$ micro config set-context --current --namespace=web
Context "microk8s" modified.
ubuntu@server-kube:~$ micro get po -o wide --all-namespaces
NAMESPACE     NAME                                      READY   STATUS    RESTARTS   AGE     IP            NODE          NOMINATED NODE   READINESS GATES
kube-system   calico-node-l7xlg                         1/1     Running   0          43m     10.0.3.25     server-kube   <none>           <none>
kube-system   coredns-864597b5fd-qw69m                  1/1     Running   0          43m     10.1.55.193   server-kube   <none>           <none>
kube-system   calico-kube-controllers-77bd7c5b-6g2b4    1/1     Running   0          43m     10.1.55.194   server-kube   <none>           <none>
ingress       nginx-ingress-microk8s-controller-n2bvw   1/1     Running   0          42m     10.1.55.195   server-kube   <none>           <none>
app           nginx-686bfc6c75-92st4                    2/2     Running   0          7m21s   10.1.55.196   server-kube   <none>           <none>
db            nginx-686bfc6c75-q9gk4                    2/2     Running   0          7m21s   10.1.55.198   server-kube   <none>           <none>
web           nginx-686bfc6c75-4g995                    2/2     Running   0          7m21s   10.1.55.197   server-kube   <none>           <none>
ubuntu@server-kube:~$ micro get ns
NAME              STATUS   AGE
kube-system       Active   43m
kube-public       Active   43m
kube-node-lease   Active   43m
default           Active   43m
ingress           Active   42m
app               Active   42m
db                Active   42m
web               Active   42m
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-4g995 curl 10.1.55.196
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   370k      0 --:--:-- --:--:-- --:--:--  600k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-4g995 curl 10.1.55.198
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
ubuntu@server-kube:~$ micro config set-context --current --namespace=app
Context "microk8s" modified.
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-92st4 -- curl 10.1.55.198
Defaulted container "nginx" out of: nginx, multitool
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   336k      0 --:--:-- --:--:-- --:--:--  600k
ubuntu@server-kube:~$ micro exec nginx-686bfc6c75-92st4 -- curl 10.1.55.197
Defaulted container "nginx" out of: nginx, multitool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0

```

### Как видно по условию задания:
Web -> App	OK    
Web -> DB	Blocked    
App -> DB	OK    
App -> Web	Blocked    
