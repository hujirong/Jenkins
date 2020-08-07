node {
    

        stage("last-changes") {
                          
                        checkout scm
                        def allChangeResults = getAllChangeResults()
                        println "AllChangeResults are:\n $allChangeResults"
			def allChangeFiles = getAllChangeFiles()
	
        }
   
}

//https://stackoverflow.com/questions/50437626/how-to-get-list-of-all-modified-files-in-pipeline-jenkins
@NonCPS
def getAllChangeFiles() {
	def changeLogSets = currentBuild.changeSets
	def total = changeLogSets.size()
	println "Total Number of AllChangeFiles: ${total}+1"
	for (int i = 0; i < changeLogSets.size(); i++) {
    		def entries = changeLogSets[i].items
    		for (int j = 0; j < entries.length; j++) {
        		def entry = entries[j]
        		echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
        		def files = new ArrayList(entry.affectedFiles)
        		for (int k = 0; k < files.size(); k++) {
            			def file = files[k]
            			echo "  ${file.editType.name} ${file.path}"
        		}
    		}
	}
}

// https://gist.github.com/ftclausen/8c46195ee56e48e4d01cbfab19c41fc0
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
