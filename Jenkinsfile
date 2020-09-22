#!groovy

common = fileLoader.fromGit('jenkins-jobs/common.groovy', 'https://github.com/ClarabridgeInc/microstructure.git', 'develop', '748a5d8c-4e19-4d15-aa55-098b87f0f9ac', 'master')

def gitUrl = "https://github.com/iharh/graktnative.git" // common.config().fxUrl
def gitBranch = "develop"

node(common.config().mainNode) {
    try {
        stage('Checkout') {
            common.gitCheckout(gitUrl, gitBranch)
        }
        stage('Build') {
            gradle('clean build')
        }
    } catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
        echo 'Job was canceled by the user'
        return
    } catch (Exception err) {
        println err
        currentBuild.result = 'FAILURE'
    }
}

def gradle(gradleArgs) {
    common.execCrossPlatform("./gradlew --no-daemon $gradleArgs")
    common.execCrossPlatform("./gradlew --stop")
}
