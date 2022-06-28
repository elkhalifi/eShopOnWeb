pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        bat 'dotnet build eShopOnWeb.sln'
      }
    }

    stage('Tests') {
      parallel {
        stage('Unit') {
          steps {
            bat 'dotnet test tests/UnitTests'
          }
        }

        stage('Integration') {
          steps {
            bat 'dotnet test tests/IntegrationTests'
          }
        }

        stage('Functional') {
          steps {
            warnError(message: 'Functional problem') {
              bat 'dotnet test tests/FunctionalTests'
            }

          }
        }

      }
    }

    stage('Deployment') {
      steps {
        bat 'dotnet publish eShopOnWeb.sln -o /var/aspnet'
        dir(path: '/var/aspnet') {
          archiveArtifacts '*'
        }

      }
    }

  }
}