pipeline {
    agent any

    tools { 
        maven 'Leen_Maven' 
        jdk 'Leen_JDK' 
    }

    stages {
        stage('Check out') {
            steps {
                git(url: 'https://github.com/leen-kamalmaz/maven-samples-A6', branch: 'master')
            }
        }

        stage('Finding Bug-Induced Release') {
            steps {
                script {
                    // initialize git bisect
                    sh 'git bisect start'
                    
                    // mark the known good and bad commits
                    sh 'git bisect good 98ac319c0cff47b4d39a1a7b61b4e195cfa231e5'
                    sh 'git bisect bad 198644632661c67b6c32f59e9047c11a70685e15'

                    // automate git bisect with a script
                    // script returns 0 if the current commit is good, and 1 if it's bad
                    sh """
                        git bisect run sh -c '
                        mvn clean verify
                        if [ $? -eq 0 ]; then
                            exit 0  # good commit
                        else
                            exit 1  # bad commit
                        fi
                        '
                    """

                    // end git bisect session
                    sh 'git bisect reset'
                }
            }
        }

        stage('Run') {
            steps {
                sh 'mvn verify'
            }
        }
    }
}
