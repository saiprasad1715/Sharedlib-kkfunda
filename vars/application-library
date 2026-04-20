def call() {

    stage('Compile') {
        sh 'mvn clean compile'
    }

    stage('Build') {
        sh 'mvn package'
    }

    stage('SonarQube Report') {
        sh 'mvn sonar:sonar'
    }

    stage('Nexus') {
        sh 'mvn deploy'
    }

    stage('Deploy to Tomcat') {
        withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {

            sh '''
            WAR_FILE=$(ls target/*.war)
            curl -v -u $USER:$PASS -T $WAR_FILE "$TOMCAT_URL/deploy?path=/myapp&update=true"
            '''
        }
    }
}
