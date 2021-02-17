# 리눅스
## 리눅스 환경 확인

### CPU 관련 사항 확인
~~~ bash
#cpu info 정리
cat /proc/cpuinfo

#cpu 갯수
grep -c processor /proc/cpuinfo

#cpu 코어 정보
grep "cpu cores" /proc/cpuinfo | tail -1

#메모리 정보
cat /proc/meminfo | grep MemTotal

~~~

# HyperData 설치

## Trouble Shooting

### 설치 중 kernel 설정 오류
~~~ bash                                                                                                                                     11m
[root@hc-hd01 v8.3.4]$ kubectl get apply -f 04-1.tb.yaml 
error: when paths, URLs, or stdin is provided as input, you may not specify resource arguments as well

[root@hc-hd01 v8.3.4]$ ls -al
합계 12480
drwxr-xr-x. 4 root root     4096  1월 21 09:37 .
drwxr-xr-x. 3 root root       75  1월 21 09:36 ..
-rw-r--r--. 1 root root       89 12월 28 16:23 01.namespace.yaml
-rw-r--r--. 1 root root      273 12월 28 18:02 02.pvc.yaml
-rw-r--r--. 1 root root     1760 12월 30 10:29 03.service.yaml
-rw-r--r--. 1 root root     1816 12월 28 17:53 04-1.tb.yaml
-rw-r--r--. 1 root root     4184 12월 28 17:07 04.tb_hl_eda.yaml
-rw-r--r--. 1 root root     3017  1월  6 14:51 05.hd_yaml
-rw-r--r--. 1 root root      484  1월 13 17:22 06.add_kubeflow_role.yaml
-rw-r--r--. 1 root root      180  1월 12 14:43 07.disable-mtls.json
drwxr-xr-x. 4 1000 1000      275 12월 30 14:37 automl_deploy_8.3.4_private
drwxr-xr-x. 4 root root      275  1월 19 20:10 automl_deploy_8.3.4hotpatch_private
-rw-r--r--. 1 root root 12735009  1월 21 09:33 release8.3.4hotpatch_automl_installation_private.tar.gz

[root@hc-hd01 v8.3.4]$ kubectl apply -f 04-1.tb.yaml 
deployment.apps/tibero-deployment created

[root@hc-hd01 v8.3.4]$ kubectl get po -n hyperdata
NAME                                 READY   STATUS            RESTARTS   AGE
tibero-deployment-6b46cb587b-2g79n   0/1     SysctlForbidden   0          12s
tibero-deployment-6b46cb587b-4ckmh   0/1     SysctlForbidden   0          11s
tibero-deployment-6b46cb587b-5jmnp   0/1     SysctlForbidden   0          1s
tibero-deployment-6b46cb587b-b87dt   0/1     SysctlForbidden   0          4s
tibero-deployment-6b46cb587b-c7zbp   0/1     SysctlForbidden   0          8s
tibero-deployment-6b46cb587b-cr9s9   0/1     SysctlForbidden   0          12s
tibero-deployment-6b46cb587b-d827k   0/1     SysctlForbidden   0          5s
tibero-deployment-6b46cb587b-dgrqp   0/1     SysctlForbidden   0          2s
tibero-deployment-6b46cb587b-f9hjb   0/1     SysctlForbidden   0          8s
tibero-deployment-6b46cb587b-jd4sm   0/1     SysctlForbidden   0          10s
tibero-deployment-6b46cb587b-jn8l5   0/1     SysctlForbidden   0          1s
tibero-deployment-6b46cb587b-kpjsb   0/1     SysctlForbidden   0          3s
tibero-deployment-6b46cb587b-lp7cc   0/1     SysctlForbidden   0          2s
tibero-deployment-6b46cb587b-lwfdl   0/1     SysctlForbidden   0          4s
tibero-deployment-6b46cb587b-mlwdx   0/1     SysctlForbidden   0          11s
tibero-deployment-6b46cb587b-pkhdl   0/1     SysctlForbidden   0          12s
tibero-deployment-6b46cb587b-qqnrt   0/1     SysctlForbidden   0          12s
tibero-deployment-6b46cb587b-rrgn2   0/1     SysctlForbidden   0          10s
tibero-deployment-6b46cb587b-sbql6   0/1     SysctlForbidden   0          7s
tibero-deployment-6b46cb587b-sngds   0/1     SysctlForbidden   0          9s
tibero-deployment-6b46cb587b-wjskt   0/1     SysctlForbidden   0          6s
tibero-deployment-6b46cb587b-x5t4b   0/1     Pending           0          0s
tibero-deployment-6b46cb587b-xdbr7   0/1     SysctlForbidden   0          7s
tibero-deployment-6b46cb587b-zczhk   0/1     SysctlForbidden   0          5s

[root@hc-hd01 v8.3.4]$ kubectl describe po -n hyperdata tibero-deployment-6b46cb587b-2g79n
Name:           tibero-deployment-6b46cb587b-2g79n
Namespace:      hyperdata
Priority:       0
Node:           hd-hc03/
Start Time:     Thu, 21 Jan 2021 09:42:07 +0900
Labels:         app=hyperdata
                lb=tibero-lb
                pod-template-hash=6b46cb587b
Annotations:    cni.projectcalico.org/ipv4pools: ["default-ipv4-ippool"]
Status:         Failed
Reason:         SysctlForbidden
Message:        Pod forbidden sysctl: "kernel.sem" not whitelisted
IP:             
IPs:            <none>
Controlled By:  ReplicaSet/tibero-deployment-6b46cb587b
Containers:
  hd-tibero:
    Image:       192.168.158.62:5000/hyperdata8.3_tb:20201217_v1
    Ports:       8629/TCP, 8630/TCP, 9999/TCP, 9095/TCP, 9093/TCP, 9094/TCP, 9777/TCP
    Host Ports:  0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP
    Limits:
      cpu:             2
      memory:          8Gi
      nvidia.com/gpu:  0
    Requests:
      cpu:             2
      memory:          8Gi
      nvidia.com/gpu:  0
    Environment:
      TB_PORT:             8629
      MAX_SESSTION_COUNT:  100
      TOTAL_SHM_SIZE:      3
      MEMORY_TARGET:       6
    Mounts:
      /db from pv-storage (rw)
      /etc/localtime from tz-seoul (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-tzmkf (ro)
Volumes:
  pv-storage:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  hyperdata-pvc
    ReadOnly:   false
  tz-seoul:
    Type:          HostPath (bare host directory volume)
    Path:          /usr/share/zoneinfo/Asia/Seoul
    HostPathType:  
  default-token-tzmkf:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-tzmkf
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason                  Age        From                     Message
  ----     ------                  ----       ----                     -------
  Normal   Scheduled               <unknown>  default-scheduler        Successfully assigned hyperdata/tibero-deployment-6b46cb587b-2g79n to hd-hc03
  Normal   SuccessfulAttachVolume  31s        attachdetach-controller  AttachVolume.Attach succeeded for volume "pvc-63aed05a-8b06-4066-a5b3-79736a504d77"
  Warning  SysctlForbidden         30s        kubelet, hd-hc03         **forbidden sysctl: "kernel.sem" not whitelisted**
[root@hc-hd01 v8.3.4]# kubectl apply -f 04.tb_hl_eda.yaml 
deployment.apps/tibero-deployment configured
deployment.apps/hyperloader-deployment created
deployment.apps/eda-deployment created
[root@hc-hd01 v8.3.4]# kubectl get po -n hyperdata
NAME                                      READY   STATUS            RESTARTS   AGE
eda-deployment-c7bd9f64b-rhhgm            1/1     Running           0          5s
hyperloader-deployment-7888b69f8c-ncdjl   1/1     Running           0          5s
tibero-deployment-5cdbb8f47-89l7h         0/1     Pending           0          5s
tibero-deployment-6b46cb587b-25r5w        0/1     SysctlForbidden   0          22s
tibero-deployment-6b46cb587b-2bv7k        0/1     SysctlForbidden   0          42s
tib
tibero-deployment-6b46cb587b-hbj4k        0/1     SysctlForbidden   0          6s
tibero-deployment-6b46cb587b-hpxbv        0/1     SysctlForbidden   0          34s
tibero-deployment-6b46cb587b-jbnlx        0/1     Pending           0          0s
tibero-deployment-6b46cb587b-jd4sm        0/1     SysctlForbidden   0          55s
tibero-deployment-6b46cb587b-jh75q        0/1     SysctlForbidden   0          36s
tibero-deployment-6b46cb587b-jn8l5        0/1     SysctlForbidden   0          46s
tibero-deployment-6b46cb587b-jpg5m        0/1     SysctlForbidden   0          21s
tibero-deployment-6b46cb587b-ztzw2        0/1     SysctlForbidden   0          19s
#... Tibero 설치가 안 되고 sysctlForbideden이 계속 생기는 현상
# describe해보면 "forbidden sysctl: "kernel.sem" not whitelisted" 라고 뜨는 현상

[root@hc-hd01 v8.3.4]$ kubectl describe po -n hyperdata tibero-deployment-5cdbb8f47-89l7h
Name:           tibero-deployment-5cdbb8f47-89l7h
Namespace:      hyperdata
Priority:       0
Node:           hd-hc03/
Start Time:     Thu, 21 Jan 2021 09:43:17 +0900
Labels:         app=hyperdata
                lb=tibero-lb
                pod-template-hash=5cdbb8f47
Annotations:    cni.projectcalico.org/ipv4pools: ["default-ipv4-ippool"]
Status:         Failed
Reason:         SysctlForbidden
Message:        Pod forbidden sysctl: "kernel.sem" not whitelisted
IP:             
IPs:            <none>
Controlled By:  ReplicaSet/tibero-deployment-5cdbb8f47
Containers:
  hd-tibero:
    Image:       192.168.158.62:5000/hyperdata8.3_tb:20201217_v1
    Ports:       8629/TCP, 8630/TCP, 9999/TCP, 9095/TCP, 9093/TCP, 9094/TCP, 9777/TCP
    Host Ports:  0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP, 0/TCP
    Limits:
      cpu:             2
      memory:          16Gi
      nvidia.com/gpu:  0
    Requests:
      cpu:             2
      memory:          16Gi
      nvidia.com/gpu:  0
    Environment:
      TB_PORT:             8629
      MAX_SESSTION_COUNT:  100
      TOTAL_SHM_SIZE:      4
      MEMORY_TARGET:       8
    Mounts:
      /db from pv-storage (rw)
      /etc/localtime from tz-seoul (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-tzmkf (ro)
Volumes:
  pv-storage:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  hyperdata-pvc
    ReadOnly:   false
  tz-seoul:
    Type:          HostPath (bare host directory volume)
    Path:          /usr/share/zoneinfo/Asia/Seoul
    HostPathType:  
  default-token-tzmkf:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-tzmkf
    Optional:    false
QoS Class:       Guaranteed
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason                  Age        From                     Message
  ----     ------                  ----       ----                     -------
  Warning  FailedScheduling        <unknown>  default-scheduler        0/5 nodes are available: 1 Insufficient memory, 4 Insufficient cpu.
  Warning  FailedScheduling        <unknown>  default-scheduler        0/5 nodes are available: 1 Insufficient memory, 4 Insufficient cpu.
  Normal   Scheduled               <unknown>  default-scheduler        Successfully assigned hyperdata/tibero-deployment-5cdbb8f47-89l7h to hd-hc03
  Warning  SysctlForbidden         7s         kubelet, hd-hc03         forbidden sysctl: "kernel.sem" not whitelisted
  Normal   SuccessfulAttachVolume  7s         attachdetach-controller  AttachVolume.Attach succeeded for volume "pvc-63aed05a-8b06-4066-a5b3-79736a504d77"

[root@hc-hd01 v8.3.4]# kubectl delete -f 04.tb_hl_eda.yaml 
deployment.apps "tibero-deployment" deleted
deployment.apps "hyperloader-deployment" deleted
deployment.apps "eda-deployment" deleted

[root@hc-hd01 v8.3.4]$ kubectl delete -f 04-1.tb.yaml 
Error from server (NotFound): error when deleting "04-1.tb.yaml": deployments.apps "tibero-deployment" not found

[root@hc-hd01 v8.3.4]$ kubectl get po -n hyperdata
NAME                             READY   STATUS        RESTARTS   AGE
eda-deployment-c7bd9f64b-rhhgm   0/1     Terminating   0          71s

#tibero yaml 내 커널 설정 확인
[root@hc-hd01 v8.3.4]$ vi 04-1.tb.yaml 
#

#kernel sem 설정 확인
[root@hc-hd01 v8.3.4]$ cat /proc/sys/kernel/sem
250	32000	32	128

[root@hc-hd01 v8.3.4]$ vi 04-1.tb.yaml 

[root@hc-hd01 v8.3.4]$ vi /proc/sys/kernel/sem

[root@hc-hd01 v8.3.4]$ systemctl status kubelet
● kubelet.service - kubelet: The Kubernetes Node Agent
   Loaded: loaded (/usr/lib/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: active (running) since 월 2021-01-18 14:32:10 KST; 2 days ago
     Docs: https://kubernetes.io/docs/
 Main PID: 6432 (kubelet)
    Tasks: 31
   Memory: 80.4M
   CGroup: /system.slice/kubelet.service
           └─6432 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/l...

 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.682468    6432 clientconn.go:104] parsed scheme: ""
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.682499    6432 clientconn.go:104] scheme "" not registered, fallback to default scheme
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.682565    6432 passthrough.go:48] ccResolverWrapper: sending update to cc: {[{/var/lib/ku...}] <nil>}
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.682579    6432 clientconn.go:577] ClientConn switching balancer to "pick_first"
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.683984    6432 clientconn.go:104] parsed scheme: ""
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.684007    6432 clientconn.go:104] scheme "" not registered, fallback to default scheme
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.684030    6432 passthrough.go:48] ccResolverWrapper: sending update to cc: {[{/var/lib/ku...}] <nil>}
 1월 21 09:44:42 hc-hd01 kubelet[6432]: I0121 09:44:42.684041    6432 clientconn.go:577] ClientConn switching balancer to "pick_first"
 1월 21 09:45:17 hc-hd01 kubelet[6432]: W0121 09:45:17.452697    6432 volume_linux.go:45] Setting volume ownership for /var/lib/kubelet/pods/ef711eff-4d4...
 1월 21 09:45:17 hc-hd01 kubelet[6432]: W0121 09:45:17.452749    6432 volume_linux.go:45] Setting volume ownership for /var/lib/kubelet/pods/ef711eff-4d4...
Hint: Some lines were ellipsized, use -l to show in full.
~~~

#### 해결방안
1. 설정 확인
   - 현재 설정값 확인방법 1
~~~ bash
[root@zetawiki ~] ipcs -l | grep -A5 Semaphore
------ Semaphore Limits --------
max number of arrays = 128
max semaphores per array = 250
max semaphores system wide = 32000
max ops per semop call = 32
semaphore max value = 32767
~~~

   - 현재 설정값 확인방법 2
~~~ bash
[root@zetawiki ~]$ cat /proc/sys/kernel/sem
250	32000	32	128
~~~
SEMMSL: 배열당 최대 세마포어 수 (max semaphores per array)
SEMMNS: 시스템 전체 최대 세마포어 수 (max semaphores system wide)
SEMOPM : 세마포어 호출당 최대 operation 수 (max ops per semop call)
SEMMNI: 최대 배열 수(max number of arrays)[1]

   - 부팅시 설정값 확인
~~~ bash
[root@zetawiki ~]# grep kernel.sem /etc/sysctl.conf
kernel.sem = 250 32000 32 128
~~~

2. 일시 변경
~~~ bash
sysctl -w kernel.sem="1000 32000 100 512"
~~~
재부팅되면 /etc/sysctl.conf에 기록된 값으로 초기화된다.

3. 영구 변경
1) sysctl.conf 파일을 수정한다.
2) kernel.sem을 찾아서 값을 수정한다.
3) 재부팅한다.
~~~bash
vi /etc/sysctl.conf
#kernel.sem = 250 32000 32 128
kernel.sem = 1000 32000 100 512
~~~

4. 같이 보기
- ipcs
- /etc/sysctl.conf

5. 참고
- http://www.puschitz.com/TuningLinuxForOracle.shtml
- http://www.unix.com/showthread.php?t=77785
- http://publib.boulder.ibm.com/infocenter/db2luw/v9/index.jsp?topic=%2Fcom.ibm.db2.udb.uprun.doc%2Fdoc%2Ft0008238.htm : 리눅스/proc/sys/kernel오라클 DBSysctl