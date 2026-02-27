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
                echo "hello $NAME ${params.LASTNAME}"
            }
            
        }
    
    stage('test')
    {
        parallel {
            stage('testA')
            {
              steps{
                echo "this is test A"
              }
                
            }
            stage('testB')
            {
                steps{
                    echo "this is test B"
                }
                
            }
        }
        post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
    }
    }
}