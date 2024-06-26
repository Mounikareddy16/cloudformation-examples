pipeline {
    agent any

    environment {
        GITHUB_CREDENTIALS_ID = 'git-creds'
        SNYK_API_TOKEN = credentials('test-snyk-api')
        BRANCH_NAME = 'feature' 
    }
    tools {
        nodejs 'NodeJS'  // Use the name of the NodeJS installation defined in Jenkins
    }
    stages {		
        stage('Checkout') {
            steps {
                // Check out the specific branch
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: env.BRANCH_NAME]],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: 'https://github.com/Mounikareddy16/cloudformation-examples']]
                ])
            }
        }       
 
        stage('Run IAC Scan') {
            steps {
                sh 'snyk auth f557c5e3-ea14-40fe-ae60-71e7367f91fa'
                sh 'snyk iac test > iac_report.json --report'

             }
             post {
                always {
                     sh 'export SNYK_DEBUG=* && snyk monitor --org=mouni.prani16 --project-name=Mounikareddy16/cloudformation-examples'
                     cleanWs notFailBuild: true, patterns: [[pattern: 'iac_report.json', type: 'EXCLUDE']]
               }
           }

       }
        

    }
  
}

