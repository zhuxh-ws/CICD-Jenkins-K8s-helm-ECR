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
            echo "ok test list"
            #sh "ansible-playbook ./CI-CD/deploy/Jenkins_deploy_playbook.yml  --extra-vars ECRLink=${ECRLink}  --extra-vars ImageName=${ImageName} --extra-vars imageTag=${ImageTag} --extra-vars Namespace=${Namespace}"
        }
    }
}
