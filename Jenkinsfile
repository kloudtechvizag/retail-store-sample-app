pipeline {
  agent any

  environment {
    DOCKER_REGISTRY = 'myregistry.example.com'
    SRC_DIR = 'src'
  }

  stages {

    stage('Build & Test') {
      parallel {
        stage('UI') {
          steps {
            dir("${SRC_DIR}/ui") {
              sh '././mvnw clean package -DskipTests'
              sh './mvnw test'
              junit '**/target/surefire-reports/*.xml'
            }
          }
        }

        // stage('Catalog') {
        //   steps {
        //     dir("${SRC_DIR}/catalog") {
        //       sh 'go mod tidy'
        //       sh 'go test ./...'
        //     }
        //   }
        // }

        stage('Cart') {
          steps {
            dir("${SRC_DIR}/cart") {
              sh './mvnw clean package -DskipTests'
              sh './mvnw test'
              junit '**/target/surefire-reports/*.xml'
            }
          }
        }

        stage('Orders') {
          steps {
            dir("${SRC_DIR}/orders") {
              sh './mvnw clean package -DskipTests'
              sh './mvnw test'
              junit '**/target/surefire-reports/*.xml'
            }
          }
        }

        // stage('Checkout') {
        //   steps {
        //     dir("${SRC_DIR}/checkout") {
        //       sh 'npm ci'
        //       sh 'npm test'
        //     }
        //   }
        // }
      }
    }

    // stage('Dockerize & Push') {
    //   parallel {

    //     stage('Dockerize UI') {
    //       steps {
    //         dir("${SRC_DIR}/ui") {
    //           script {
    //             def image = "${DOCKER_REGISTRY}/ui:${BUILD_NUMBER}"
    //             sh "docker build -t ${image} ."
    //             sh "docker push ${image}"
    //           }
    //         }
    //       }
    //     }

    //     stage('Dockerize Catalog') {
    //       steps {
    //         dir("${SRC_DIR}/catalog") {
    //           script {
    //             def image = "${DOCKER_REGISTRY}/catalog:${BUILD_NUMBER}"
    //             sh "docker build -t ${image} ."
    //             sh "docker push ${image}"
    //           }
    //         }
    //       }
    //     }

    //     stage('Dockerize Cart') {
    //       steps {
    //         dir("${SRC_DIR}/cart") {
    //           script {
    //             def image = "${DOCKER_REGISTRY}/cart:${BUILD_NUMBER}"
    //             sh "docker build -t ${image} ."
    //             sh "docker push ${image}"
    //           }
    //         }
    //       }
    //     }

    //     stage('Dockerize Orders') {
    //       steps {
    //         dir("${SRC_DIR}/orders") {
    //           script {
    //             def image = "${DOCKER_REGISTRY}/orders:${BUILD_NUMBER}"
    //             sh "docker build -t ${image} ."
    //             sh "docker push ${image}"
    //           }
    //         }
    //       }
    //     }

    //     stage('Dockerize Checkout') {
    //       steps {
    //         dir("${SRC_DIR}/checkout") {
    //           script {
    //             def image = "${DOCKER_REGISTRY}/checkout:${BUILD_NUMBER}"
    //             sh "docker build -t ${image} ."
    //             sh "docker push ${image}"
    //           }
    //         }
    //       }
    //     }

    //   }
    // }
  }

  post {
    success {
      echo "✅ Build & Dockerize stages completed for all components."
    }
    failure {
      echo "❌ One or more stages failed. Check logs above."
    }
  }
}
