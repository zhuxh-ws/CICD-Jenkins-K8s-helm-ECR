node {
    def Namespace = "default"
    def ImageName = "enel-helloword"
    def ImageTag = "latest"
    def ECRLink = "https://581114568537.dkr.ecr.cn-north-1.amazonaws.com.cn/enel-helloword"

    stage ('Get Source'){
        echo "1.Clone Repo Stage"
        final scmVars = checkout(scm)
    }
        
    stage('Build Image'){
        echo "2.Build Docker Image Stage"
        dir("${env.WORKSPACE}"){
            docker.build("${ImageName}:latest")
        }
     }  
    
    stage('Push Image'){
        echo "3.Push Docker Image Stage"
        docker.withRegistry("${ECRLink}", 'ecr:cn-north-1:001'){
            docker.image("${ImageName}").push("${ImageTag}")
        }
     }

    stage('Deploy helloworld on K8s'){
        dir("${env.WORKSPACE}"){
            sh "ansible-playbook ./CI-CD/deploy/Jenkins_deploy_playbook.yml  --extra-vars ECRLink=${ECRLink}  --extra-vars ImageName=${ImageName} --extra-vars imageTag=${ImageTag} --extra-vars Namespace=${Namespace}"
        }
    }
    
    stage('apply nginx for test'){
        sh "kubectl get node"
        sh "kubectl create namespace dev"
        sh "kubectl apply -f nginx.yaml --namespace dev"
        sh "kubectl get pod -n dev"
        sh "kubectl get po"
        sh "kubectl get svc"
    }    
}
