def git_commit
def build
def mvn_build

pipeline {
    agent any

    stages{
        stage('Build MVN'){
            steps {
            	echo "BUILD information before build start ...."
            	echo '...'
            	echo '...'
            	script {
					def result = build(
	                    job: 'mvn_build'
	                )

	                println(
	                	"MVN build: ${result.getNumber()}\n" +
	                	"Branch: ${result.rawBuild.environment.BRANCH}\n" +
	                	"GIT_COMMIT: ${result.rawBuild.environment.GIT_COMMIT}\n" +
	                	"Current Build: ${currentBuild.number}\n"
	                )
	                git_commit = result.rawBuild.environment.GIT_COMMIT
	                mvn_build = result.getNumber()
	                build =  currentBuild.number
	                env['BUILD_FULL_VERSION'] = "Proj Build ${build} - MVN ${mvn_build}"
	                env['GIT_COMMIT'] = git_commit
                }
                echo "${env['BUILD_FULL_VERSION']}"
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
        stage ('Tag build'){
        	steps{
        		echo 'Check out git'
        		git url: 'https://github.com/trantaanna/maven-project.git'

				script {

					sshagent(['Jenkins']) {
					   withEnv(["BUILD_FULL_VERSION=${BUILD_FULL_VERSION}","GIT_COMMIT=${GIT_COMMIT}"]) {
							sh """git config --global user.email \"dstudt@cylance.com\"
		git config --global user.name \"jenkins_builder\"
		git tag -a ${BUILD_FULL_VERSION} ${GIT_COMMIT} -m \"Auto Tag ${BUILD_FULL_VERSION}\"
							"""
							sh """git push origin ${BUILD_FULL_VERSION}"""
						}
					}
				}
        	}
        }
    }
}