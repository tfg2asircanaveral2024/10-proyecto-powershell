pipeline {
    agent none
    options {
        // no obtener el contenido del repositorio nada más empezar la Pipeline
        skipDefaultCheckout true
    }

    stages {
        stage('Importar Repositorio') {
            agent any            

            steps {
                dir('/var/jenkins_home/workspace-compartido') {
                    git branch: 'correcto', url: 'https://github.com/tfg2asircanaveral2024/10-proyecto-powershell.git'
                }
            }
        }

        stage('Dependencias (PSDepend)') {
            agent { label 'agente-pwsh-jnlp' }

            steps {
                dir('/var/jenkins_home/workspace-compartido') {
                    pwsh 'Invoke-PSDepend -Path ./Dependencias -Force -ErrorAction Stop'
                }                
            }
        }

        stage('Tests (Pester)') {
            agent { label 'agente-pwsh-jnlp' }

            steps {
                dir('/var/jenkins_home/workspace-compartido') {
                    pwsh 'Invoke-Pester -ErrorAction Stop'
                }                
            }
        }

        stage('Tests (PSScriptAnalyzer)') {
            agent { label 'agente-pwsh-jnlp' }

            steps {
                dir('/var/jenkins_home/workspace-compartido') {
                    pwsh './Gestionar-Datos.ScriptAnalyzer.ps1'
                }                
            }
        }
        
        stage('Despliegue a la rama Produccion de GitHub') {
            agent any
            steps {
                dir('/var/jenkins_home/workspace-compartido') {
                    sh 'chmod u+x script-despliegue-git.sh'                
                    sh './script-despliegue-git.sh'
                }
            }
        }
    }
}