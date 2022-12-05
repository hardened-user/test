// ---------------------------------------------------------------------------------------------------------------------
def monctlDockerImage = 'hardeneduser/toolkit'
def registryUrl = 'local.regestry'
def registryCredID = '*****'
def inventoryRepoBranch = 'main'
def inventoryCredID = '*****'
def ansibleCredID = '0b2e42a9-725e-4335-80fb-c578969cd51f'
// ---------------------------------------------------------------------------------------------------------------------
properties([
    parameters([
        string(
            name: 'inventoryRepoUrl',
            defaultValue: 'https://github.com/hardened-user/test',
            description: "Inventory repository",
            trim: true
        ),
        string(
            name: 'ansibleLimit',
            defaultValue: '',
            description: "Limitations of patterns",
            trim: true
        ),
        booleanParam(
                name: 'diffEnabled',
                defaultValue: false,
                description: "Using diff mode"
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
        userRemoteConfigs: [[url: params.inventoryRepoUrl ]] // credentialsId: inventoryCredID
    ])
    // -----------------------------------------------------------------------------------------------------------------
    stage ("Run") {
        // Credentials type: Secret text
        withCredentials([string(credentialsId: ansibleCredID, variable: 'VAULT_SECRET')]) {
            // Credentials type: Username with password
            //docker.withRegistry(registryUrl, registryCredID) {
                docker.image(monctlDockerImage).inside("--tmpfs /tmpfs:rw,size=64k") {
                    sh '''echo \$HOME \\\$VAULT_SECRET'''
                    sh 'echo "\${VAULT_SECRET}" > /tmpfs/secret'
                    //
                    EXEC_CMD = 'ansible-playbook -i inventory/inventory.yaml --vault-password-file /tmpfs/secret playbook.yaml'
                    if (params.ansibleLimit) {
                        EXEC_CMD += " --limit '${params.ansibleLimit}'"
                    }
                    if (params.diffEnabled) {
                        EXEC_CMD += " --diff"
                    }
                    sh "${EXEC_CMD}"
                }
            //}
        }
    }
}
