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
                 echo '...we are running ansible-playbook'
                 sh 'ansible-playbook site.yml'
                 s3Download(file:"${WORKSPACE}/hiapp.war", bucket:'super-original-name-for-task-bucket-1-upload', path:'hiapp.war', force:true)
                 sh 'ls -al'
                 sh "scp ${WORKSPACE}/hiapp.war ec2-user@34.220.195.55:/usr/share/tomcat/webapps"
                 sh 'ansible tom -m file -a "dest=/usr/share/tomcat/webapps/hiapp mode=644 owner=tomcat group=tomcat"'
                 }
        }
    
    }
    
}
