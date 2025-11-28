//jqglqngl
pipeline {
    agent any

    tools {
        maven "apache-maven-3.9.11"
    }

    stages {

        stage ('maven package') {
            steps {
                sh "mvn clean package"
            }
        }

        stage ('extract war + modify') {
            steps {
                sh """
                    cd /mnt/
                    rm -rf test || true
                    mkdir test
                    cd test
                    cp -r /root/.jenkins/workspace/RDS/target/LoginWebApp.war .
                    unzip LoginWebApp.war
                    rm -rf LoginWebApp.war

                    sed -i 's|localhost|database-1.cl2ge2kg8jsb.ap-south-1.rds.amazonaws.com|g' userRegistration.jsp
                    sed -i 's|"root", "root"|"admin", "ratan123"|g' userRegistration.jsp

                    
                    zip -r LoginWebApp.war *
                    cp LoginWebApp.war /var/lib/docker/volumes/V1/_data/
                """
            }
        }

        stage ('docker compose up') {
            steps {
                sh """
                    cd /mnt/project/
                    docker-compose down || true
                    docker-compose up -d
                """
            }
        }

    }

    post {
        always {
            sh "rm -rf ${WORKSPACE}/*"
        }
    }
}
