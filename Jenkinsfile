pipeline {
    agent {
        label 'nod1'
    }
    parameters {
        choice choices: ['dev', 'prod', 'booba'], name: 'select_env'
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
                script{
                    file = load "script.groovy"
                    file.hello()
                }
                sh 'mvn clean package -Dskiptests=true'
                
            }
            
        }
    
    stage('test')
    {
        parallel {
            stage('testA')
            {
                agent{label "nod1"}
              steps{
                echo "this is test A"
                sh "mvn test"
              }
                
            }
            stage('testB')
            {
                agent{label "nod1"}
                steps{
                    echo "this is test B"
                    sh "mvn test"
                }
                
            }
        }
        post {
                success {
                    dir("webapp/target/")
                    {
                        stash name: "maven-build", includes: "*.war"
                    }
                }
            }
    }
    stage('deploye dev')
    {
        when { expression {params.select_env == 'dev'}
            beforeAgent true
            }
        steps{
            dir ("/var/www/html"){
                unstash "maven-build"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
            
        }
    }
    stage('deploy_prod')
    {
        when { expression {params.select_env == 'prod'}
            beforeAgent true
            agent{label 'nod2'}
            }
        steps{
            dir ("/var/www/html"){
                unstash "maven-build"
            }
            sh """
            cd /var/www/html/
            jar -xvf webapp.war
            """
        
    }
    }

    }
}