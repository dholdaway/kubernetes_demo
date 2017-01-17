## install docker
# sudo apt-get install docker.io

## Enable cgroups and swap accounting
## edit /etc/defaults/grub.conf

#GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"

## sudo update-grub
## sudo reboot

## sudo docker run --volume=/var/etcd:/var/etcd --net=host -d gcr.io/google_containers/etcd:2.0.12 /usr/local/bin/etcd --addr=127.0.0.1:4001 --bind-addr=0.0.0.0:4001 --data-dir=/var/etcd/data

## sudo docker run \
--volume=/:/rootfs:ro \
--volume=/sys:/sys:ro \
--volume=/dev:/dev \
--volume=/var/lib/docker/:/var/lib/docker:ro \
--volume=/var/lib/kubelet/:/var/lib/kubelet:rw \
--volume=/var/run:/var/run:rw \
--net=host \
--pid=host \
--privileged=true \
-d \
gcr.io/google_containers/hyperkube:v1.0.1 \
/hyperkube kubelet --containerized --hostname-override="127.0.0.1" --address="0.0.0.0" --api-servers=http://localhost:8080 --config=/etc/kubernetes/manifests


                sudo docker run --volume=/:/rootfs:ro --volume=/sys:/sys:ro --volume=/dev:/dev --volume=/var/lib/docker/:/var/lib/docker:ro --volume=/var/lib/kubelet/:/var/lib/kubelet:rw --volume=/var/run:/var/run:rw --net=host --pid=host --privileged=true -d gcr.io/google_containers/hyperkube:v1.0.1 /hyperkube kubelet --containerized --hostname-override="127.0.0.1" --address="0.0.0.0" --api-servers=http://localhost:8080 --config=/etc/kubernetes/manifests

sudo docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v1.0.1 /hyperkube proxy --master=http:127.0.0.1:8080 --vv=2

### download kubectl

                wget https://storage.googleapis.com/kubernetes-release/release/v1.0.1/bin/linux/amd64/kubectl
                chmod +x kubectl
                mkdir bin
                mv kubectl bin
                . .profile

## login to docker

sudo docker login

## get nodeJS app from git, build and push

git clone https://github.com/wardviaene/docker-demo
cd docker-demo
sudo docker build -i dholdaway/k8s-demo
sudo docker push dholdaway/k8s-demo

##test the app

export SERVICE_IP=$(kubectl get service helloworld-service -o=template -t={{.spec.clusterIP}})  
export SERVICE_IP=$(kubectl get service helloworld-service -o=template '-t={{(index .spec.ports 0).port}}')  

curl http://${SERVICE_IP}:${SERVICE_PORT}

## Sources and Further Readings
Section 12, Lecture 74
http://www.slideshare.net/realgenekim/2014-state-o...
http://www.scriptrock.com/automation-enterprise-de...
http://www.mobify.com/blog/devops-101-best-practic...
http://www.slideshare.net/sanjeev-sharma/campdevop...
http://www.slideshare.net/pritiman/intro-to-devops...
http://www.slideshare.net/geekle/devops-5348895?ne...
http://en.wikipedia.org/wiki/DevOps
http://12factor.net
CD: http://continuousdelivery.com/
GIT: https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow (CC License)
git flow: http://danielkummer.github.io/git-flow-cheatsheet/
kubernetes setup: https://github.com/kubernetes/kubernetes/blob/release-1.1/docs/getting-started-guides/docker.md
The Phoenix Project (recommended book): http://www.amazon.com/The-Phoenix-Project-Helping-Business/dp/0988262592
