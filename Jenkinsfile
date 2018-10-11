pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages{
        stage('Build MVN'){
            steps {
            	echo "BUILD information before build start ...."
            	echo "${BRANCH}"
            	echo "${GIT_COMMMIT}"
            	script {
					def result = build(
	                    job: 'mvn_build'
	                )

	                println(
	                	"MVN build: ${result.getNumber()}\n"
	                )
                }
                echo "${result.rawBuild.environment.GIT_COMMIT}"
            }
            post {
                success {
                    echo 'Done build...'
                    echo "${currentBuild.number}"
     
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