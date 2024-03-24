pipeline {
    agent any

    tools { 
        maven 'DHT_MVN' 
        jdk 'DHT_SENSE' 
    }

    stages {
        stage('Check out') {
            steps {
                // Assuming the use of a parameter or hardcoded value for the repository
                git(url: 'https://github.com/dhetong/maven-samples-A6.git', branch: 'master')
            }
        }

        stage('Identify Bug-Inducing Commit') {
            steps {
                script {
                    // Initialize git bisect
                    sh 'git bisect start'
                    
                    // Mark the known good and bad commits
                    // Replace <good_commit_hash> and <bad_commit_hash> with actual values
                    sh 'git bisect good <good_commit_hash>'
                    sh 'git bisect bad <bad_commit_hash>'

                    // Automate git bisect with a custom script
                    // This script should return 0 if the current commit is good, and 1 if bad
                    // For simplicity, using mvn verify and checking the exit status
                    sh """
                        git bisect run sh -c '
                        mvn clean verify
                        if [ $? -eq 0 ]; then
                            exit 0  # Good commit
                        else
                            exit 1  # Bad commit
                        fi
                        '
                    """

                    // End git bisect session
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
