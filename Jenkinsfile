// -----------------------------------------------------------------------------------------------------------------
def monctlDockerImage = 'hardeneduser/toolkit'
def ansibleCredID = '0b2e42a9-725e-4335-80fb-c578969cd51f'
def registryUrl = 'docker.io'
def registryCredID = '*****'
// -----------------------------------------------------------------------------------------------------------------
properties([
    parameters([
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
        )
    ])
])

node ('docker') {
    // checkout current
    //checkout scm
    // checkout inventory
    checkout([
        $class: 'GitSCM',
        branches: [[name: '*/' + params.inventory_repo_branch]],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CleanCheckout'], [$class: 'RelativeTargetDirectory', relativeTargetDir: "inventory"]],
        submoduleCfg: [],
        userRemoteConfigs: [[url: params.inventory_repo_url ]] // credentialsId: <CRED_ID>
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        // Credentials type: Secret file
        withCredentials([string(credentialsId: '0b2e42a9-725e-4335-80fb-c578969cd51f', variable: 'ANSIBLE_VAULT_SECRET')]) {
            // Credentials type: Username with password
            //docker.withRegistry(<REGISTRY_URL>, dockerCredID) {
                docker.image(monctlDockerImage).inside("--tmpfs /ram:rw,noexec,nosuid,size=64k") {
                    sh 'echo "${ANSIBLE_VAULT_SECRET}" > /ram/se'
                    sh "env; ls -lah inventory; echo ${env.BUILD_ID}, df -h"
                    //sh "ansible-playbook -i inventory/inventory.yaml --vault-password-file vault.txt --diff local.yaml $@"
                }
            //}
        }
    }
}
