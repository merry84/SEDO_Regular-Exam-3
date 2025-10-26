pipeline {
  agent any

  options {
    timestamps()
   
  }

  environment {
    DOTNET_CLI_TELEMETRY_OPTOUT = '1'
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE = '1'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Restore') {
      when { branch 'main' }
      steps {
        bat 'dotnet --info'
        bat 'dotnet restore Horizons.sln'
      }
    }

    stage('Build') {
      when { branch 'main' }
      steps {
        bat 'dotnet build Horizons.sln --configuration Release --no-restore'
      }
    }

    stage('Test Unit') {
      when { branch 'main' }
      steps {
        bat 'dotnet test Horizons.Tests.Unit/Horizons.Tests.Unit.csproj --configuration Release --no-build --logger "trx;LogFileName=unit-tests.trx" --results-directory "TestResults/Unit"'
      }
      post {
        always {
          junit allowEmptyResults: true, testResults: '**/TestResults/Unit/*.trx'
        }
      }
    }

    stage('Test Integration') {
      when { branch 'main' }
      steps {
        bat 'dotnet test Horizons.Tests.Integration/Horizons.Tests.Integration.csproj --configuration Release --no-build --logger "trx;LogFileName=integration-tests.trx" --results-directory "TestResults/Integration"'
      }
      post {
        always {
          junit allowEmptyResults: true, testResults: '**/TestResults/Integration/*.trx'
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: '**/TestResults/**/*.trx, **/bin/**/Release/**', allowEmptyArchive: true
    }
  }
}
