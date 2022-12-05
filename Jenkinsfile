
node {


        parameters([
            booleanParam(
                name: 'publishEnabled',
                defaultValue: true,
                description: ""
            ),
            booleanParam(
                name: 'deployEnabled',
                defaultValue: getEnvDEPLOY(),
                description: ""
            ),
            choice(
                name: 'deployTo',
                choices: "smvp\nnonexistent",
                description: ""
            )
        ])
    ])

    stage('S1') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}
