# Votingaap
Assignment 
1. go to root (cd /root/ ) . (both Master and Worker can perform this in their instances)
2. git clone https://github.com/ashishrpandey/example-voting-app
3. go to /root/example-voting-app/k8s-specifications
4. [root@ip-172-31-7-102 k8s-specifications]# kubectl apply -f .
Error from server (Invalid): error when creating "result-service.yaml": Service "result" is invalid: spec.ports[0].nodePort: Invalid value: 31001: provided port is already allocated
Error from server (Invalid): error when creating "vote-service.yaml": Service "vote" is invalid: spec.ports[0].nodePort: Invalid value: 31000: provided port is already allocated
Got error node ports are already assigned, it's might be because of I tried to execute two times. First time tried and it was worked but to do it from scratch I deleted all pods, rs and services, but second time it's giving me above error.
Tried to find where was node ports used but unable to find, to overcome this error  I was changed node ports from resut-service and vote-service yamal find and took 31002 & 31003  to apply it without error.

5. for voting and result pods, Observe that NodePort is assigned.
service/result   NodePort    10.99.80.52      <none>        5001:31003/TCP   84m
service/vote     NodePort    10.99.187.70     <none>        5000:31002/TCP   84m

6. take your publicIP (your instance IP) : NodePort ► open 2 browsers , one for VOTING and one for Results.
it's working with public ip and my node ports 
  
7 try voting and see the results paralelly in results page.
Result is working fine along with voting options

8. Now try to delete the Pods one by one (first voting Pod, then worker pod, then db pod) and see what happens to the voting and result app after deleting.
No change in result tab voting pod was deleted but it creates one voting pod again as replica set is running 
[root@ip-172-31-31-219 k8s-specifications]# kubectl delete po vote-94849dc97-k7clq
pod "vote-94849dc97-k7clq" deleted
[root@ip-172-31-31-219 k8s-specifications]#
[root@ip-172-31-31-219 k8s-specifications]# kubectl get po | grep -i vote
vote-94849dc97-wll9j                               1/1     Running     0          2m19s
[root@ip-172-31-31-219 k8s-specifications]#

No change in result tab worker pod was deleted but it creates one worker pod again as replica set is running 
[root@ip-172-31-31-219 k8s-specifications]# kubectl delete po worker-dd46d7584-g844x
pod "worker-dd46d7584-g844x" deleted
[root@ip-172-31-31-219 k8s-specifications]# kubectl get po | grep -i worker
worker-dd46d7584-pmwcn                             1/1     Running     0          51s
[root@ip-172-31-31-219 k8s-specifications]#

10. what happens after db pod deletion. 
db pod recreated as replica set is running but result was not working as expected after db pod deleted
  [root@ip-172-31-31-219 k8s-specifications]# kubectl delete po db-b54cd94f4-wnpmb
pod "db-b54cd94f4-wnpmb" deleted
[root@ip-172-31-31-219 k8s-specifications]#
Db-pod is created again after deleting it 
[root@ip-172-31-31-219 k8s-specifications]# kubectl get po | grep -i db
db-b54cd94f4-c4xlh                                 1/1     Running     0          62s
[root@ip-172-31-31-219 k8s-specifications]# kubectl g

 But result page is changed and vote is removed from result page, now it's not taking any vote from voting browser even we put vote 

11. complete the assignment by making the result pod work. (if you delete db pod, results would not be captured. So repeat what we did in the class in order to make the result pod work.).
Voting aap was not working after deleting db pod, after deleting result pod it recreates it because replica set is already running for it. once result pod recreated it sync with db pod and it working properly with correct result

