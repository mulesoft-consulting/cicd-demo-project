pipeline {
  agent any
  tools { 
        maven 'maven'
        jdk 'jdk8' 
  }
  stages {
    stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
       }
    }
    stage('Unit Test') {
      steps {
        sh 'mvn clean test'
      }
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
        muleEnv = "${env.cloudhub_env.toLowerCase()}"
      }
      steps {
        echo "----Publishing to Artifactory------ "
        sh 'mvn clean package deploy -U -Dmaven.test.skip=true'
        echo "----Running Build ${env.BUILD_ID} on muleEnv - ${muleEnv}----- "
        sh 'mvn clean package deploy -DmuleDeploy -P cloudhub -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -Dmule.env=${muleEnv}'
      }
    }
  }
}
