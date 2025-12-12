pipeline
{
     agent any
     tools
     {
        maven "maven-3.9.6"
     }
     
     stages
     {
        stage('git checkout')
        {
           steps
           {
              notifyBuild('STARTED')
              git branch: 'dev', url: 'https://github.com/Adinarayanareddy1993/maven-webapplication-project-kkfunda.git'
           }
        }
        stage('compile')
        {
           steps
           {
              sh "mvn compile"
           }
        }
        stage('Build')
        {
           steps
           {
              sh "mvn clean package"
           }
        }
       /* stage('SonarQubeReports')
        {
           steps
           {
              sh "mvn sonar:sonar"
           }
        }  */
        stage('Deploy to Nexus')
        {
           steps
           {
              sh "mvn clean deploy"
           }
        }
        stage('Deploy to tomcat')
        {
           steps
           {

              sh """

      curl -u admin:password \
--upload-file /var/lib/jenkins/workspace/declarative-pipeline/target/maven-web-application.war \
"http://13.204.65.115:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """           
           }
        }
   }  // stage ending

}  // pipeline ending
