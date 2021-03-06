![](https://gaforgithub.azurewebsites.net/api?repo=CKAD-exercises/multi_container&empty)
# Multi-container Pods（マルチコンテナ Pod） (10%)

### Create a Pod with two containers, both with image busybox and command "echo hello; sleep 3600". Connect to the second container and run 'ls'（2つのコンテナを含むPodを作ってください。2つのコンテナはどちらもbusyboxイメージで"echo hello; sleep 3600"をコマンド実行するように設定してください。2つ目のコンテナに接続して'ls'コマンドを実行してくだい。）

<details><summary>show</summary>
<p>

Easiest way to do it is create a pod with a single container and save its definition in a YAML file:(一番簡単な方法は、1つのコンテナを含むPodを作り、その定義をyamlファイルに保存することです。)

```bash
kubectl run busybox --image=busybox --restart=Never -o yaml --dry-run -- /bin/sh -c 'echo hello;sleep 3600' > pod.yaml
vi pod.yaml
```

Copy/paste the container related values, so your final YAML should contain the following two containers (make sure those containers have a different name):（作成したYAMLファイルのコンテナに関する部分をコピー＆ペーストして、以下のような２つのコンテナを持つYAMLを作ってください。（コンテナ名がかぶらないようにしてください））

```YAML
containers:
  - args:
    - /bin/sh
    - -c
    - echo hello;sleep 3600
    image: busybox
    imagePullPolicy: IfNotPresent
    name: busybox
    resources: {}
  - args:
    - /bin/sh
    - -c
    - echo hello;sleep 3600
    image: busybox
    name: busybox2
```

```bash
kubectl create -f pod.yaml
# Connect to the busybox2 container within the pod
kubectl exec -it busybox -c busybox2 -- /bin/sh
ls
exit

# or you can do the above with just an one-liner
kubectl exec -it busybox -c busybox2 -- ls

# you can do some cleanup
kubectl delete po busybox
```

</p>
</details>
