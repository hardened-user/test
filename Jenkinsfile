//
pipeline {
    // -----------------------------------------------------------------------------------------------------------------
    parameters {
        string(
            name: 'inventory',
            defaultValue: 'https://github.com/hardened-user/test',
            description: "inventory git repo",
            trim: true
        )
    // -----------------------------------------------------------------------------------------------------------------
    stage('S1') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}
