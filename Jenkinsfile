pipeline{
    agent {
        node {
            label 'AGENT-1'
        }
    }

    options{
        timeout(time:1, unit:'HOURS')
        //disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters{
        // string(name : 'version', defaultValue : '1.0.0', description : 'wt is artifact version?')
        // string(name : 'environment', defaultValue : 'dev', description : 'wt is environment?')
        string(name : 'version', defaultValue : '', description : 'wt is artifact version?')
        string(name : 'environment', defaultValue : '', description : 'wt is environment?')\
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Pick something')
    }
  
    stages{
        stage('Print version'){
            steps{
                sh """
                    echo "version : ${params.version}"
                    echo "environment" : ${params.environment}
                """
            }
        }
         stage('Init'){
            steps{
                sh """
                    cd terraform
                    terraform init --backend-config=${params.environment}/backend.tf -reconfigure
                """
            }
        }
        stage('Plan'){
            steps{
                sh """
                    cd terraform
                    terraform plan -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}"
                """
            }
        }
         stage('Apply') {
            when{
                expression {
                    params.action== 'apply'
                }
            }
            input{
                message "should we continue?"
                ok "yes, U can.."
            }
            steps {
                sh """
                    cd terraform
                    terraform apply -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                """
            }
        }
        stage('Destroy') {
            when{
                expression {
                    params.action== 'destroy'
                }
            }
            input{
                message "should we continue?"
                ok "yes, U can.."
            }
            steps {
                sh """
                    cd terraform
                    terraform destroy -var-file=${params.environment}/${params.environment}.tfvars -var="app_version=${params.version}" -auto-approve
                """
            }
        }
    }

    
    //post build
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }
}