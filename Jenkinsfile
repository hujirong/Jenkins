pipeline {
    agent {
        node {
            label 'master'
        }
    }
    
    stages {

        stage("last-changes") {
            steps {
                script {
                    
                        checkout scm
                        def lastSuccessfulCommit = getLastSuccessfulCommit()
                        def currentCommit = commitHashForBuild( currentBuild.rawBuild )
                        if (lastSuccessfulCommit) {
                            commits = sh(
                                script: "git rev-list $currentCommit \"^$lastSuccessfulCommit\"",
                                returnStdout: true
                                ).split('\n')
                            println "Commits are: $commits"
                        }
                    
                }
            }
        }
    }
}
