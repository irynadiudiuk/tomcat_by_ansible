pipeline {
    agent none 
  /*  parameters {
        string(name: 'S3', defaultValue: 'super-original-name-for-task-bucket-1-upload')
           } */
    tools {
        maven 'maven'
        jdk 'jdk'
    }
    stages {
        stage('Cloning Git repository') {
            agent { label 'master' } 
            steps {
                deleteDir() 
                echo '...cloning GIT repository'
                git 'https://github.com/irynadiudiuk/tomcat_by_ansible.git'
                sh 'pwd' 
                sh 'ls -al' 
                  }
        }
        
        stage('S3 download') {
            agent { label 'master' } 
               steps {
                 echo '...we are downloading file from S3'
                 s3Download(file:"${WORKSPACE}/roles/tomcat/files/hiapp.war", bucket:'super-original-name-for-task-bucket-1-upload', path:'hiapp.war', force:true)
                 sh 'ls -al ${WORKSPACE}/roles/tomcat/files/'
                 /*sh "scp ${WORKSPACE}/hiapp.war ${WORKSPACE}/roles/tomcat/files"*/
                 /* sh 'ansible tom -m file -a "dest=/usr/share/tomcat/webapps/hiapp.war mode=644 owner=tomcat group=tomcat"' */
                 }
        }
         stage('Deploying WAR file to Tomcat') {
            agent { label 'master' } 
               steps {
                 echo '...we are running ansible-playbook'
                 sh 'ansible-playbook site.yml'
                 /* sh 'ansible tom -m file -a "dest=/usr/share/tomcat/webapps/hiapp.war mode=644 owner=tomcat group=tomcat"' */
                 }
        }
    
    }
    
}
