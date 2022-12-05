// ---------------------------------------------------------------------------------------------------------------------
def monctlDockerImage = 'hardeneduser/toolkit'
def ansibleCredID = '0b2e42a9-725e-4335-80fb-c578969cd51f'
def inventoryRepoUrl = 'https://github.com/hardened-user/test'
def inventoryRepoBranch = 'main'
def inventoryCredID = '*****'
def registryUrl = 'docker.io'
def registryCredID = '*****'
// ---------------------------------------------------------------------------------------------------------------------
properties([
    parameters([
        string(
            name: 'ansible_limit_hosts',
            defaultValue: '',
            description: "Limitations of patterns",
            trim: true
        )
    ])
])
// ---------------------------------------------------------------------------------------------------------------------
node ('docker') {
    checkout([
        $class: 'GitSCM',
        branches: [[name: '*/' + inventoryRepoBranch]],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[$class: 'CleanCheckout'], [$class: 'RelativeTargetDirectory', relativeTargetDir: "inventory"]],
        submoduleCfg: [],
        userRemoteConfigs: [[url: inventoryRepoUrl]] // credentialsId: inventoryCredID
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("RUN") {
        // Credentials type: Secret text
        withCredentials([string(credentialsId: '0b2e42a9-725e-4335-80fb-c578969cd51f', variable: 'ANSIBLE_VAULT_SECRET')]) {
            // Credentials type: Username with password
            //docker.withRegistry(<REGISTRY_URL>, dockerCredID) {
                docker.image(monctlDockerImage).inside("--tmpfs /tmpfs:rw,noexec,nosuid,size=64k") {
                    sh 'echo "${ANSIBLE_VAULT_SECRET}" > /tmpfs/secret'
                    sh "echo ansible-playbook -i inventory/inventory.yaml --vault-password-file /tmpfs/secret --limit '${params.ansible_limit_hosts}' --diff local.yaml $@"
                    sh "env; ls -lah inventory; echo ${env.BUILD_ID}; df -h; cat /tmpfs/secret"
                }
            //}
        }
    }
}
