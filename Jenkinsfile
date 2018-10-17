node {
  // Mark the code checkout 'stage'....
  stage 'Stage Checkout'

checkout([$class: 'GitSCM', branches: [[name: '*/master']], browser: [$class: 'BitbucketWeb',
repoUrl: 'https://KevadiyaKrunalK:115320693022@bitbucket.org/KevadiyaKrunalK/jenkinsdemo.git'],
doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '1833afea-8bd9-4877-96ff-d3bbba200216', url: 'https://KevadiyaKrunalK:115320693022@bitbucket.org/KevadiyaKrunalK/jenkinsdemo.git']]])

    //Checkout code from repository and update any submodules
    //checkout scm
    sh 'git submodule update --init'

    stage 'Stage Build'

    //branch name from Jenkins environment variables
    echo "My branch is: ${env.BRANCH_NAME}"

    def flavor = flavor(env.BRANCH_NAME)
    echo "Building flavor ${flavor}"

    //build your gradle flavor, passes the current build number as a parameter to gradle
    sh "./gradlew clean assemble${flavor}Debug -PBUILD_NUMBER=${env.BUILD_NUMBER}"

    stage 'Stage Archive'
    //tell Jenkins to archive the apks
    archiveArtifacts artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true

    //stage 'Stage Upload To Fabric'
    //sh "./gradlew crashlyticsUploadDistribution${flavor}Debug  -PBUILD_NUMBER=${env.BUILD_NUMBER}"
}

// Pulls the android flavor out of the branch name the branch is prepended with /QA_
@NonCPS
def flavor(branchName) {
    def matcher = (env.BRANCH_NAME =~ [a-z_])
    assert matcher.matches()
    matcher[0][1]
}
