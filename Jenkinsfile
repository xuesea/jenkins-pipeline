pipeline {
  agent {
    label 'all'
  }
  stages {
    stage('Checkout') {
      steps {
        echo "Checkout..."
      }
    }
    stage('CI') {
      parallel {
        stage('Build') {
          steps {
            echo "CI Build..."  
            sleep(10)
          }
        }
        stage('Test') {
          steps {
            echo "CI Test..."  
            sleep(10)
          }
        }
      }
    }
  }
  post {
    // 成功
    success {
      echo "流水线执行成功!"
      script {
        if (GIT_BRANCH == "master") {
          deploy.notificationSuccess(DEPLOYMENT, DINGDING_BOT, "上线啦！", RELEASE_BUILD)
        } else {
          deploy.notificationSuccess(DEPLOYMENT, DINGDING_BOT, "流水线完成了", RELEASE_BUILD)
        }
      }
    }
    // 失败
    failure {
    echo "流水线执行失败!"
    // check the npm log
    // fails lets check if it
    script {
      if (CURRENT_VERSION != null) {
        if (GIT_BRANCH == "master") {
          deploy.setupStaging()
        } else {
          deploy.setupInt()
        }
        deploy.rollbackDeployment(
          NAMESPACE,
          DEPLOYMENT,
          CONTAINER,
          CURRENT_VERSION,
        )
        deploy.notificationFailed(DEPLOYMENT, DINGDING_BOT, "流水线失败了", RELEASE_BUILD)
        }
      }
    }
    // 取消的
    aborted {
      echo "aborted!"
    }
  } 
}
