node{
    echo "Job name is : ${env.JOB_NAME}"
    echo "Build number is : ${env.BUILD_NUMBER}"
    echo "Node name is : ${env.NODE_NAME}"
    echo "Jenkins Home dir is : ${env.JENKINS_HOME}"

    def mavenhome = tool name:'maven3.9.9'
    stage('checkoutcode')
    {
        git branch: 'development', credentialsId: '4a17ff0a-d69c-43cf-bc09-6fb37d17cf1d', url: 'https://github.com/dks-devops-2025/maven-web-application.git'
    }
    stage('Build')
    {
        sh "${mavenhome}/bin/mvn clean package"
    }
    stage('ExecuteSonarqubeReport')
    {
        sh "${mavenhome}/bin/mvn clean sonar:sonar"
    }
    stage('uploadartifactintoNexus')
    {
        sh "${mavenhome}/bin/mvn clean deploy"
    }
    stage('DeployAppInTomcatServer')
    {
        sshagent(['d7dca3e3-fd95-4cd4-9eb2-75644c22a376']) {
        sh "scp -o StrictHostKeyChecking=yes target/maven-web-application.war ec2-user@172.31.1.162:/opt/apache-tomcat-9.0.98/webapps/"
     }
    }
  }
