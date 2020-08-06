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
                        def allChangeResults = getAllChangeResults()
                        println "AllChangeResults are: $allChangeResults"
                        def changeStringForBuild = getChangeStringForBuild()
                        println "changeStringForBuild are: $changeStringForBuild"                    
		  }
            }
        }
    }
}

@NonCPS
def getAllChangeResults() {
	def result = getChangeStringForBuild(currentBuild)

	def buildToCheck = currentBuild.getPreviousBuild()
	while (buildToCheck != null && buildToCheck.result != 'SUCCESS') {
		result += "\nBuild #${buildToCheck.number} [${buildToCheck.result}]\n"
		result += getChangeStringForBuild(buildToCheck)

		buildToCheck = buildToCheck.previousBuild
	}

	return result
}

@NonCPS
def getChangeStringForBuild(build) {
    MAX_MSG_LEN = 60

    echo "Gathering SCM changes"
    def changeLogSets = build.changeSets
	def changeString = ""

    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            def truncated_msg = entry.msg.take(MAX_MSG_LEN)
			if (entry.msg.length() > MAX_MSG_LEN) {
				truncated_msg += "..."
			}

            changeString += "-[@${entry.author.getFullName().toLowerCase()}][${entry.commitId.take(6)}]: ${truncated_msg}\n"
        }
    }

    if (!changeString) {
        changeString = " - No new changes"
    }

    return "${changeString}"
}
