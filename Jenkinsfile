def myVariable = false
pipeline {
    agent any

    stages {
        stage('Clone B') {
            steps {
                echo 'Clone B'
                git branch: 'main', credentialsId: 'cf3d6d86-2ff7-465a-8767-58e572a16539', url: 'https://github.com/Anju-Alexander/Repository_B.git'
                   
            }
            
        }
       
        stage('Build & push')
        {
             steps {
                 echo 'build'
                 commit = sh(returnStdout: true, script: 'git log -1 --oneline').trim()
                            
                 commitMsg = commit.substring( commit.indexOf(' ') ).trim()
                 
                if(commitMsg.contains('Anju'))
                {

                     sh 'git remote add repo_b_push https://github.com/Anju-Alexander/Repository_B.git'
                     sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\${parsedVersion.majorVersion}.\\${parsedVersion.minorVersion}.\\${parsedVersion.nextIncrementalVersion} versions:commit'
                     sh 'mvn install'
                     sh 'git status'
                     sh 'git add pom.xml'
                     sh 'git commit -m "updated version"'
                     echo 'push'
                     sh 'git push -u repo_b_push main'
                     sh 'git remote rm repo_b_push'
                }
                else
                {
                    sh 'mvn install'
                    echo 'build stable!'
                }
             }
        }
        stage('Test')
        {
            steps{
                
                echo 'Run'
            
            }
        
        }
       
        stage('Trigger Pipeline_A')
        {
            steps {
                
                    build 'Repo_A pipeline'
                    echo 'Built Repo_A successfully!'
               
            }
        }
    }
}
