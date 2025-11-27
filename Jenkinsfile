//jqglqngl
pipeline {
    agent any

    tools {
        maven "apache-maven-3.9.11"
    }

    stages {
		stage ('clean-repo') {
			steps {
				sh " rm -rf /mnt/servers/apache-tomcat-10.1.49/webapps/LoginWebApp* || true "
			}
		}

        stage ('maven') {
            steps {
                    sh "mvn clean package"
                }
            }

        stage ('extract war file') {
            steps {
                sh """
                cd /mnt/
                rm -rf test || true
                mkdir test
                cd test/

                cp -r LoginWebApp.war .
                unzip LoginWebApp.war
                rm -rf LoginWebApp.war
                """
            }
        }

        stage ('edit userregistration file') {
            steps {
                dir ('/mnt/test/') {
                    sh """
                    sed -i 's|localhost|database-1.cl2ge2kg8jsb.ap-south-1.rds.amazonaws.com|g' userRegistration.jsp
		    sed -i 's|"root", "root"|"admin", "ratan1234"|g' userRegistration.jsp
                
                    zip -r LoginWebApp.war *
                    cp LoginWebApp.war /mnt/servers/apache-tomcat-10.1.49/webapps/
                    """
                }
            }
        }

    }
	post {
		always {
			sh " rm -rf ${WORKSPACE}/*"
		}
	}

}
