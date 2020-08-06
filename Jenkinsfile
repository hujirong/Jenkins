pipeline {
    agent {
        node {
            label 'master'
        }
    }
    
    stages {
        stage("checkout") {
            steps {
                script {
                    git url: 'https://github.com/hujirong/Jenkins.git'
                }
            }
        }

        stage("last-changes") {
            steps {
                script {
                    def publisher = LastChanges.getLastChangesPublisher "LAST_SUCCESSFUL_BUILD", "SIDE", "LINE", true, true, "", "", "", "", ""
                    publisher.publishLastChanges()
                    def changes = publisher.getLastChanges()
                    println(changes.getEscapedDiff())
                    for (commit in changes.getCommits()) {
                        println(commit)
                        def commitInfo = commit.getCommitInfo()
                        println(commitInfo)
                        println(commitInfo.getCommitMessage())
                        println(commit.getChanges())
                    }
                }
            }
        }
    }
}
