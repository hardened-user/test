//
properties([
    parameters([
        string(
            name: 'image',
            defaultValue: 'hardeneduser/toolkit',
            description: "Docker image with Ansible",
            trim: true
        ),
        string(
            name: 'inventory',
            defaultValue: 'https://github.com/hardened-user/test',
            description: "Git repo with Ansible inventory",
            trim: true
        )
    ])
])

node ('docker') {
    // checkout current
    // need it? checkout scm
    // checkout inventory
    checkout([
        $class: 'GitSCM',
        branches: [[name: '*/master']],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CleanCheckout'], [$class: 'RelativeTargetDirectory', relativeTargetDir: "inventory"]],
        submoduleCfg: [],
        userRemoteConfigs: [[url: params.inventory ]]
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        docker.image(params.image).inside() {
            sh "env; ls -lah; exit 1"
        }
    }
}
