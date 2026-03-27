def deployApp(envName, port) {
    echo "Deploying to ${envName.toUpperCase()} environment on port ${port}..."
    git branch: 'main', url: 'https://github.com/mtararujs/python-greetings'
    bat "pm2 delete greetings-app-${envName} & EXIT /B 0"
    bat "pm2 start app.py --name greetings-app-${envName} --interpreter python -- --port ${port}"
    bat 'timeout /t 5 /nobreak >nul'
    bat "powershell -Command \"try { \$r = Invoke-WebRequest -Uri http://127.0.0.1:${port}/greetings -UseBasicParsing; Write-Host \$r.StatusCode; Write-Host \$r.Content } catch { Write-Host \$_; exit 1 }\""
}

def runTests(String envName) {
    bat "npm install"
    bat "npm run greetings greetings_${envName}"
}

pipeline {
    agent any

    stages {

        stage('install-pip-deps') {
            steps {
                echo 'Installing all required pip dependencies...'
                git url: 'https://github.com/mtararujs/python-greetings', branch: 'main'
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\python.exe -m pip install -r requirements.txt'
            }
        }

        stage('deploy-to-dev') {
            steps {
                echo 'Deploying to DEV environment on port 7001...'
                git url: 'https://github.com/mtararujs/python-greetings', branch: 'main'
                script {
                    deployApp('dev', '7001')
                }
            }
        }

        stage('tests-on-dev') {
            steps {
                echo 'Running tests on DEV environment...'
                git url: 'https://github.com/mtararujs/course-js-api-framework', branch: 'main'
                script {
                    runTests('dev')
                }
            }
        }

        stage('deploy-to-stg') {
            steps {
                echo 'Deploying to STG environment on port 7002...'
                git url: 'https://github.com/mtararujs/python-greetings', branch: 'main'
                script {
                    deployApp('stg', '7002')
                }
            }
        }

        stage('tests-on-stg') {
            steps {
                echo 'Running tests on STG environment...'
                git url: 'https://github.com/mtararujs/course-js-api-framework', branch: 'main'
                script {
                    runTests('stg')
                }
            }
        }

        stage('deploy-to-preprod') {
            steps {
                echo 'Deploying to PREPROD environment on port 7003...'
                git url: 'https://github.com/mtararujs/python-greetings', branch: 'main'
                script {
                    deployApp('preprod', '7003')
                }
            }
        }

        stage('tests-on-preprod') {
            steps {
                echo 'Running tests on PREPROD environment...'
                git url: 'https://github.com/mtararujs/course-js-api-framework', branch: 'main'
                script {
                    runTests('preprod')
                }
            }
        }

        stage('deploy-to-prod') {
            steps {
                echo 'Deploying to PROD environment on port 7004...'
                git url: 'https://github.com/mtararujs/python-greetings', branch: 'main'
                script {
                    deployApp('prod', '7004')
                }
            }
        }

        stage('tests-on-prod') {
            steps {
                echo 'Running tests on PROD environment...'
                git url: 'https://github.com/mtararujs/course-js-api-framework', branch: 'main'
                script {
                    runTests('prod')
                }
            }
        }

    }
}