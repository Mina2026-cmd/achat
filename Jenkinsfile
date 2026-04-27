pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh '''
                        mvn deploy -DskipTests \
                        --settings <(echo "<settings><servers><server><id>nexus</id><username>${NEXUS_USER}</username><password>${NEXUS_PASS}</password></server></servers></settings>")
                    '''
                }
            }
        }
    }
}
