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

        throw e
    }
}
