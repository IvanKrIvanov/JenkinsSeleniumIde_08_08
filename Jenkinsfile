pipeline {
    agent any
    
    stages {
        stage('Checkout code') {
            steps {
                // Checkout code from GitHub and specify the branch
                git branch: 'main', url: 'https://github.com/IvanKrIvanov/JenkinsSeleniumIde_08_08'
            }
        }

        stage('Set up .NET Core') {
            steps {
                bat '''
                echo Downloading .NET Sdk
                curl -l -o dotnet-sdk-6.0.132-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/ad59f1d1-5f19-4474-86be-2f09ab195618/5c7a64445dae84e386bb88e1f6ac09e4/dotnet-sdk-6.0.132-win-x86.exe
                echo installing dotnet-sdk-6.0.132-win-x86.exe
                dotnet-sdk-6.0.132-win-x86.exe /quite /norestart
                '''
            }
        }

        stage('restoring nuget packages') {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
                
            }
        }

        stage('buil') {
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
                
            }
        }

        stage('run test') {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx'
            }
        }        
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            //junit '**/TestResults/*.trx'
            step{
                $class:'MSTestPublisher',
                TestResultsFile: '**/TestResults/*.trx'
            }
        }
    }
}