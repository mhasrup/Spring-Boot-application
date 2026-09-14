pipeline {
    agent { label 'ubuntu' }
    stages {
       stage('CheckOutGit') {
                   steps {
                       checkout([$class: 'GitSCM',
                           branches: [[name: 'main']],
                           doGenerateSubmoduleConfigurations: false,
                           extensions: [],
                           submoduleCfg: [],
                           userRemoteConfigs: [[credentialsId: 'laxman_github_token',
                               url: 'https://github.com/mhasrup/Spring-Boot-application.git']]])
                   }
               }
    }
}