
pipeline {
    
    agent { label 'master' } 
    
    stages {
        stage('Make rsa') {
            steps {
                git 'https://github.com/Thrayee/jenkinsfile.git'
                mvnHome = tool 'M3'
                }
            }
        stage('build') {
            steps {
                if (isUnix()) {
         			sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      			} 
      			else {
        			 bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      			}
            }
        }
        stage('Analysis') {
            steps {
                step([$class: 'JUnitResultArchiver', testResults: '**/target/**/TEST-*.xml'])    
    			step([$class: 'hudson.plugins.checkstyle.CheckStylePublisher', pattern: '**/target/checkstyle-result.xml', unstableTotalAll:'0',unhealthy:'100', healthy:'100'])
    			step([$class: 'PmdPublisher', pattern: '**/target/pmd.xml'])
    			step([$class: 'FindBugsPublisher', pattern: '**/findbugsXml.xml'])
            }              
        }
	}              
}


