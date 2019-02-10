
pipeline {
    
    agent { label 'master' } 

    stages {
        stage('Make rsa') {
            steps {
                git 'https://github.com/Thrayee/jenkinsfile.git'
                
                }
            }
        stage('build') {
            steps {
            	script {
	            	mvnHome = tool 'M3'
	                if (isUnix()) {
	         			sh "'${mvnHome}/bin/mvn' -f static-code-analysis-example/pom.xml clean package checkstyle:checkstyle findbugs:findbugs cobertura:cobertura pmd:pmd"
	      			} 
	      			else {
	        			 bat(/cd static-code-analysis-example && "${mvnHome}\bin\mvn" clean package checkstyle:checkstyle findbugs:findbugs cobertura:cobertura pmd:pmd/)
	        		}
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


