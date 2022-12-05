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
            name: 'inventory_repo_url',
            defaultValue: 'https://github.com/hardened-user/test',
            description: "Git repo with Ansible inventory",
            trim: true
        ),
        string(
            name: 'inventory_repo_branch',
            defaultValue: 'main',
            description: "",
            trim: true
        ),
        string(
            name: 'inventory_dir_name',
            defaultValue: 'main',
            description: "",
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
        branches: [[name: '*/' + inventory_repo_branch]],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CleanCheckout'], [$class: 'RelativeTargetDirectory', relativeTargetDir: "inventory"]],
        submoduleCfg: [],
        userRemoteConfigs: [[url: params.inventory_repo_url ]] // credentialsId: <CRED_ID>
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        // Credentials type: Secret file
        withCredentials([file(credentialsId: '197141e6-5e25-4866-9353-437f105d3fc1', variable: 'vault.txt')]) {
            // Credentials type: Username with password
            //docker.withRegistry(<REGISTRY_URL>, <CRED_ID>) {
                docker.image(params.image).inside() {
                    sh "env; ls -lah inventory;"
                    sh "ansible-playbook -i inventory/${params.inventory_dir_name}/inventory.yaml --vault-password-file vault.txt --diff local.yaml $@"
                }
            //}
        }
        }
    }
}
