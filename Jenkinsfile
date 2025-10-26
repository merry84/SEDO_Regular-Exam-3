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
      
      steps {
        bat 'dotnet --info'
        bat 'dotnet restore Horizons.sln'
      }
    }

    stage('Build') {
      
      steps {
        bat 'dotnet build Horizons.sln --configuration Release --no-restore'
      }
    }

    stage('Test Unit') {
     
      steps {
        bat 'dotnet test Horizons.Tests.Unit/Horizons.Tests.Unit.csproj --configuration Release --no-build --logger "trx;LogFileName=unit-tests.trx" --results-directory "TestResults/Unit"'
      }
      
    }

    stage('Test Integration') {
     
      steps {
        bat 'dotnet test Horizons.Tests.Integration/Horizons.Tests.Integration.csproj --configuration Release --no-build --logger "trx;LogFileName=integration-tests.trx" --results-directory "TestResults/Integration"'
      }
     
    }
  }


}
