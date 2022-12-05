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

    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        docker.image(params.image).inside("") {
            sh "env; ls -lah"
        }
    }
}
