//
properties([
    parameters([
        string(
            name: 'img',
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

node {

    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        docker.image(parameters.image).inside() {
            sh "env"
        }
    }
}
