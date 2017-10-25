#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
	def SFDC_USERNAME = env.SFDC_USERNAME
	
	def toolbelt = tool 'toolbelt'
	
	stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    
    stage('Get default scratch org') {
        // need to pull out assigned username
        rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:config:get defaultusername --json"
        println(rmsg)
        def jsonSlurper = new JsonSlurperClassic()
        def robj = jsonSlurper.parseText(rmsg)
        if (robj.status != 0) { error 'org creation failed: ' + robj.message }
        SFDC_USERNAME=robj.result[0].value
        println(SFDC_USERNAME)
        robj = null
    }
    
    stage('Run Provar test cases') {
    	println(SFDC_USERNAME)
    	rmsg = bat returnStdout: true, script: "ant -f ANT/build.xml -DSFDC_USERNAME_SO=${SFDC_USERNAME}"
        println(rmsg)
    }
}
