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
    //checkout scm
    // checkout inventory
    checkout([
        $class: 'GitSCM',
        branches: [[name: '*/main']],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CleanCheckout'], [$class: 'RelativeTargetDirectory', relativeTargetDir: "inventory"]],
        submoduleCfg: [],
        userRemoteConfigs: [[url: params.inventory ]] // credentialsId: <CRED_ID>
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        //docker.withRegistry(<REGISTRY_URL>, <CRED_ID>) {
            docker.image(params.image).inside() {
                sh "env; ls -lah; find ./"
            }
        //}
    }
}
