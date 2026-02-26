pipeline {
    agent {
        label 'nod1'
    }
    parameters {
        string defaultValue: 'mhd', name: 'LASTNAME'
    }

    environment{
        NAME = "akif"
    }
    tools {
        maven 'mymaven'
    }


    stages {
        stage('build') {
            steps {
                sh 'mvn clean package'
                echo "hello $NAME #{params.LASTNAME}"
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}