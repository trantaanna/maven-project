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
	                	"Current Build: ${currentBuild.number}\n" +
	                	"... ${currentBuild.number} - MVN ${result.getNumber()}\n"
	                )

	                env['tag'] = "${currentBuild.number} - MVN ${result.getNumber()}"
	                env['GIT_COMMIT'] = result.rawBuild.environment.GIT_COMMIT
                	
                	println( "${env.tag}")
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
        stage ('Tag build'){
        	steps{
        		echo 'Check out git'
        		git url: 'https://github.com/trantaanna/maven-project.git'
        		script{
        			println( "${env.tag} ${env.GIT_COMMIT}")
        		}
        	}
        }
    }
}