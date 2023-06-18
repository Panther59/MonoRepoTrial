pipeline {
  agent {
    label 'buildagent'
  }
  environment {
    dotnet = 'C:\\Program Files\\dotnet\\dotnet.exe'
  }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/Panther59/MonoRepoTrial.git', branch: 'main'
      }
    }
    stage('Build & Test') {
      matrix {
        axes {
          axis {
            name 'project'
            values 'ConsoleApp1', 'ConsoleApp2'
          }
        }
        when {
          anyOf {
                    changeset pattern: "$project/**"
                    changeset pattern: "SharedClassLib/**"
                }          
        }
        stages {
          stage('Restore PACKAGES') {
            steps {
              dir("$project") {
                bat "dotnet restore"
              }
            }

          }
          stage('Clean') {
            steps {
              dir("$project") {
                bat "dotnet clean"
              }
            }
          }
          stage('Build') {
            
            steps {
               lock(resource: 'synchronous-build', inversePrecedence: true)
              {
                dir("$project") {
                  bat "dotnet build --configuration Release"
                }
              }
            }
          }
        }
      }
    }
  }
}