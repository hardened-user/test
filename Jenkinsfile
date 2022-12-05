//
properties([
    parameters([
        string(
            name: 'image',
            defaultValue: 'hardeneduser/toolkit',
            description: "docker image",
            trim: true
        ),
        string(
            name: 'inventory',
            defaultValue: 'https://github.com/hardened-user/test',
            description: "inventory git repo",
            trim: true
        )
    ])
])

node ('docker') {
    try {
    checkout scm
        // -----------------------------------------------------------------------------------------------------------------
        stage ("RUN") {
            docker.image(params.image).inside("") {
                sh "env; ls -lah; exit 1"
            }
        }
    } catch (e) {
        sh "echo errorrr"
        if (currentBuild.rawBuild.getActions(jenkins.model.InterruptedBuildAction.class).isEmpty()) {
            currentBuild.result = "FAILED"
            /*
                actions on fail
            */
        } else {
            // org.jenkinsci.plugins.workflow.steps.FlowInterruptedException
            currentBuild.result = "ABORTED"
        }
        throw e
    }
}
