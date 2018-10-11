pipeline {
    agent any

    stages{
        stage('Build MVN'){
            steps {
            	echo "BUILD information before build start ...."
            	echo "Branch: ${BRANCH}"
            	echo "Git Commit: ${GIT_COMMMIT}"
            	script {
					def result = build(
	                    job: 'mvn_build'
	                )

	                println(
	                	"MVN build: ${result.getNumber()}\n"
	                	"${result.rawBuild.environment.GIT_COMMIT}\n"
	                	"Current Build: ${currentBuild.number}\n"
	                )
                }

            }
            post {
                success {
                    echo 'Done build...'
     
                }
                failure {
                	echo 'FAILED to build mvn'
                }
            }
        }
        stage ('Dummy build'){
            steps {
            	echo 'Hello World inside Dummy build ...'
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}