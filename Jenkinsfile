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
            'echo ok do'
        }
    }
}
