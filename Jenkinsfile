pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Get Code') {
            steps {
                echo '[Get Code] Obteniendo codigo de github...'
                git 'https://github.com/Nxito/helloworld.git'
                sh 'ls -a'
                echo "$WORKSPACE"
            }
        }
        stage('Build') {
            steps {
                echo '[Build] Actualmente no requiere una Build'
            }
        }
        stage('Testing Parallel') {
            parallel {
                stage('Unit') {
                    environment {
                        PYTHONPATH = "${WORKSPACE}"
                    }
                    steps {
                        echo '[Unit Testing] Inicio los test unitarios ubicados en ./test'
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                            sh 'pytest --junitxml=result-unit.xml test/unit'
                            junit 'result-unit.xml'
                        }
                    }
                }
                stage('Service') {
                    environment {
                        PYTHONPATH = "${WORKSPACE}"
                        FLASK_APP = 'app/api.py'
                    }
                    steps {
                        echo '[Iniciar Flask]'
                        sh '''
                            flask run --host=127.0.0.1 --port=5000 > flask.log 2>&1 &
                            FLASK_PID=$!
                            for i in $(seq 1 10); do
                                if curl --silent --fail http://127.0.0.1:5000 > /dev/null; then
                                    echo "Flask está disponible"
                                    break
                                fi
                                echo "Flask no está listo, reintentando..."
                                sleep 2
                            done

                            if curl --silent --fail http://wiremock:8080/__admin > /dev/null; then
                                echo "WireMock está disponible"
                            else
                                echo "No se pudo conectar a WireMock en http://wiremock:8080"
                                exit 1
                            fi
                            # Aqui pongo las pruebas del api rest
                            pytest --junitxml=result-rest.xml test/rest
                            kill $FLASK_PID
                        '''
                        junit 'result-rest.xml'
                    }
                }
            }
        }
        stage('Results') {
            steps {
                junit 'result-*.xml'
            }
        }
    }
}