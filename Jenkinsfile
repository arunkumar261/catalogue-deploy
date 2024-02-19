pipeline{
    agent {
        node {
            label 'AGENT-1'
        }
    }

    options{
        timeout(time:1, unit:'HOURS')
        disableConcurrentBuilds()
    }
    parameters{
        // string(name : 'version', defaultValue : '1.0.0', description : 'wt is artifact version?')
        // string(name : 'environment', defaultValue : 'dev', description : 'wt is environment?')
        string(name : 'version', defaultValue : '', description : 'wt is artifact version?')
        string(name : 'environment', defaultValue : '', description : 'wt is environment?')
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