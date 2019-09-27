node {
    
	try {
            notify('Started')
            
            stage('checkout') {
                checkout scm
            //    git 'https://github.com/dilipRathoreRepo/jenkins2-course-spring-petclinic.git'
            }
            
                stage('build') {
                    def mvnHome = tool name: 'maven', type: 'maven'
                    sh label: '', script: "${mvnHome}/bin/mvn clean package"   
                }
                
            notify('Done')
            
    } catch (err) {
        notify("Error ${err}")
        currentBuild.result = 'FAILURE'
    }
        stage('archive') {
            publishHTML([allowMissing: true, 
                         alwaysLinkToLastBuild: false, 
                         keepAll: true, 
                         reportDir: 'target/site/jacoco/', 
                         reportFiles: 'index.html', 
                         reportName: 'Code Coverage', 
                         reportTitles: ''])
                         
            archiveArtifacts "target/*.?ar"
            step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/TEST-*.xml'])
        }
}

node {
    notify('deploy to staging ?')
}

input 'deploy to staging ?'

node{
    stage('deployment') {
        sh label: '', script: 'echo "deployment completed"'
    }
}


def notify(status){
    emailext (
      to: "dilip.rathore.infosys@gmail.com",
      subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}


