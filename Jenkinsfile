pipeline{
agent any
environment{
	CONTAINER_NAME="ecommerce"
	DOCKER_CRED= "docker_creds"
	DOCKER_IMAGE="kpkm25/jenkins-ecommerce"
}
stages{
	stage("Git clone"){
		git url:"https://github.com/KPkm25/jenkins-ecommerce" ,branch:"main"
}
	stage("Docker build"){
		sh " docker build -t ${DOCKER_IMAGE} . "
}
	stage("Docker push"){
		docker.withRegistry('',${DOCKER_CRED}){
		sh "docker push ${DOCKER_IMAGE}"
}
}
	stage("Local Deployment"){
	sh "
	docker pull ${DOCKER_IMAGE}:latest
	docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker stop || true
        docker ps -a -q --filter name=${CONTAINER_NAME} | xargs -r docker rm || true
	docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:latest
	"
}
}
}
