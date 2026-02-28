node
{
    def mvn=tool name : "mvn"
    try
    {
    
    stage('git checkout')
    {
        notifyBuild('STARTED')
        git branch: 'feature2', url: 'https://github.com/Devops-aws848/maven-webapplication-project-kkfunda.git'
     }
     stage('compiler')
    {
        sh "${mvn}/bin/mvn clean compile"
    }
    stage('build')
    {
        sh "${mvn}/bin/mvn clean package"
    }
    stage('quality')
    {
        notifyBuild('STARTED')
        sh "${mvn}/bin/mvn sonar:sonar"
    }
    
        stage('deploy')
    {
      sh    """
        
        curl -u admin:password \
        --upload-file /var/lib/jenkins/workspace/Bsnl/target/maven-web-application.war \
        "http://13.234.67.81:8080/manager/text/deploy?path=/maven-web-application&update=trye"
        """
     }
    }   
    catch (e) {
   
       currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)       //function calling
  }
} 
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#D6F527'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#27F580'
  } else {
    color = 'RED'
    colorCode = '#F52727'
  }

slackSend (color: colorCode, message: summary, channel: '#theprint,#soical,#offset')
  
}
