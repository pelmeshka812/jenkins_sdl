pipeline {
  agent any

  stages {
    stage('Send to SAST') {
      steps {
        script {
          def auth = "Authorization: Bearer <API_KEY>"
          def branch = "<BRANCH>"
          def projectID = "<PROJECT_ID>"

          if (env.BRANCH_NAME == branch) {
            sh 'apk --no-cache add zip curl'
            sh 'zip -r $GIT_COMMIT.zip . -x ".git/*"'
            sh 'curl -X POST -H "$auth" -F "name=$GIT_COMMIT_MSG" -F "commit_hash=$GIT_COMMIT" -F "repository=$GIT_URL" -F "reference=$GIT_BRANCH" -F "archive=@./$GIT_COMMIT.zip" -F "outer_id=$projectID" -F "version=$GIT_SHORT_COMMIT" https://v2.sdl.bi.zone/api/external/upload_code/'
          } else {
            echo "Skipping 'Send to SAST' stage for branch ${env.BRANCH_NAME}"
          }
        }
      }
    }
   stage('Test') {}
  }
}
