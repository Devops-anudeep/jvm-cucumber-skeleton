node
{
    stage ('Pull code from repository') {
    
        git 'https://github.com/cucumber/cucumber-java-skeleton.git'
    
    }

    stage('JIRA') {
        
         def issue = jiraGetFields idOrKey: 'DSS-51', site: 'LOCAL', failOnError: false
         echo issue.data.Description
         
    }


    stage ('Build code') {
    
        sh 'cd ${WORKSPACE}/'
        
         withMaven( maven: 'Maven305') {
        
                sh 'mvn package'

            }
    }
    
    stage('Test Code') {
        

        sh 'cd ${WORKSPACE}/'
        
         withMaven( maven: 'Maven305') {
        
                sh 'mvn test'

            }
        
        junit allowEmptyResults: true, testResults: '/var/lib/jenkins/workspace/pipeline_demo/target/surefire-reports/*.xml'
    
    }
    
    stage('Deploy Code') {

        sh 'cp -R /var/lib/jenkins/workspace/pipeline_demo/target /var/lib/jenkins/workspace/Deploy_Jar'


    }
    

    stage('Upload artifact to Nexus') {
       
        nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'Pipeline-test', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/pipeline_demo/target/cucumber-java-skeleton-0.0.1.jar']], mavenCoordinate: [artifactId: 'cucumber-java-skeleton.jar', groupId: 'cucumber-java-skeleton', packaging: 'jar', version: '0.0.1']]]
 
        nexusArtifactUploader artifacts: [[artifactId: 'cucumber-java-skeleton', classifier: '', file: '/var/lib/jenkins/workspace/pipeline_demo/target/cucumber-java-skeleton-0.0.1.jar', type: 'jar']], credentialsId: 'Nexus', groupId: 'cucumber-java-skeleton', nexusUrl: 'ec2-34-224-214-70.compute-1.amazonaws.com:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'Pipeline-test', version: '0.0.1'

    }    
    
    
    stage('Update JIRA'){
     
     jiraComment body: 'Testing complete', issueKey: 'DSS-51'
   
    }
}