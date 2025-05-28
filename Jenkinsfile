pipeline {
    agent any

    environment {
        VENV_DIR = '.venv'
        APP_PORT = '5000'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'git@github.com:NdPrk/flask.git', credentialsId: 'github-ssh'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    source $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Flask with Gunicorn') {
            steps {
                sh '''
                    pkill gunicorn || true
                    source $VENV_DIR/bin/activate
                    nohup gunicorn --bind 0.0.0.0:$APP_PORT app:app &
                '''
            }
        }
    }
}
