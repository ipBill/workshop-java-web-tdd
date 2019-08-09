node {
//    stage('pull-code') { // for display purposes
//         git 'https://github.com/up1/workshop-java-web-tdd'
//    }
   stage('build') {
        sh label: '', script: 'mvn clean test'
        junit 'target/surefire-reports/*.xml'
   }
   stage('code-coverage') {
      sh label: '', script: 'mvn cobertura:cobertura'
      cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
   }
   stage('war') {
      sh label: '', script: 'mvn package -DskipTest'
   }
   stage('deploy') {
      //build 'deploy_new'
      deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://localhost:8081')], contextPath: null, war: 'target/demo.war'
   }
   stage('robot') {
       parallel Chrome: {
           stage('Chrome') {
               //sh label: '', script: 'robot atdd/*.robot'
           }
       }, Firefox: {
           stage('Firefox') {
               
           }
        }, IE: {
            stage('IE')  {
                
            }
        }
   }
   stage('jmeter') {
       sh label: 'JMeter...', script: 'jmeter -n -t jmeter/demo.jmx -l jmeter/result.csv'
       perfReport filterRegex: '', sourceDataFiles: 'jmeter/result.csv'
   }
}