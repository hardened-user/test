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
        sendmail()
        throw e
    }
}

@NonCPS
def sendmail() {
    if (currentBuild.rawBuild.getActions(jenkins.model.InterruptedBuildAction.class).isEmpty()) {
        currentBuild.result = "FAILED"
        additional_recipients = ''
        /*common.sendMailFailed(
            additional_recipients,
            "FAILED ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            """\
            <p><strong><span style="color: #ff0000;">FAILED</span> ${env.JOB_NAME} #${env.BUILD_NUMBER}</strong></p>
            <p><a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a><p>""".stripIndent()
        )*/
    } else {
        currentBuild.result = "ABORTED"
    }
}
