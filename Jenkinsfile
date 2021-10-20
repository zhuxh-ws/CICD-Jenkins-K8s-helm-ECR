node {
    def Namespace = "default"
    def ImageName = "enel-helloword"
    def ECRLink = "581114568537.dkr.ecr.cn-north-1.amazonaws.com.cn/enel-helloword"

    stage ('checkout'){
        final scmVars = checkout(scm)
        ImageTag = "${checkout(scm).GIT_COMMIT}"
    }
        
    stage('Docker Build and Push'){
        dir("${env.WORKSPACE}"){
            docker.build("${ImageName}:latest")
        }
            
        docker.withRegistry("${ECRLink}", 'ecr:cn-north-1:enel-helloword'){
            docker.image("${ImageName}").push("${ImageTag}")
        }
     }

    stage('Deploy helloworld on K8s'){
        dir("${env.WORKSPACE}"){
            'echo ok'
            'docker tag enel-helloword:latest 581114568537.dkr.ecr.cn-north-1.amazonaws.com.cn/enel-helloword:latest'
            'docker push 581114568537.dkr.ecr.cn-north-1.amazonaws.com.cn/enel-helloword:latest'
        }
    }
}
