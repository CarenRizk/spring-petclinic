def BROKEN = null
def STABLE = null
def COUNT = 0

pipeline {
    agent any
    
    stages {
        stage('Build') {
            when {
                expression { (STABLE == null || COUNT >=8 ) }
            }
            steps {
                sh 'mvn clean' 
            }
        }
        stage('Test') {
            when {
                expression { COUNT >= 8 }
            }
            steps {
                sh 'mvn test' 
            }
        }
        stage('Package') {
            when {
                expression { COUNT >= 8 }
            }
            steps {
                sh 'mvn package' 
            }
        }
    }
    post {
        always {
            COUNT = COUNT + 1
        }
    	failure {
            BROKEN = env.GIT_COMMIT
            sh "git bisect start ${BROKEN} ${STABLE}"
            sh "git bisect run mvn clean test"
            sh "git bisect reset"
    	}
        success {
            STABLE = env.GIT_COMMIT
        }
   }
}