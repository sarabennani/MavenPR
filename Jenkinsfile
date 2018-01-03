node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

     stage ('Build') {
            steps {
		        bat 'mvn install'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
	stage('cobrtr'){
		steps{
			bat 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
		}
		post{
			success {
				step([$class: 'CoberturaPublisher', autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/*.xml', failUnhealthy: false, failUnstable: false, maxNumberOfBuilds: 0, onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false])
			}
		}
	}
	stage('jvdoc'){
		steps{
			bat 'mvn javadoc:javadoc'
		}
		post{
			success {
				step([$class: 'JavadocArchiver', javadocDir: 'target/site/apidocs', keepAll: false])
			}
		}
	}
	stage('package'){
		steps{
			bat 'mvn package'
		}
		
	}
}
