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
        def beginIndex = rmsg.indexOf('{')
        def endIndex = rmsg.indexOf('}')
        def jsobSubstring = rmsg.substring(beginIndex)
        println(jsobSubstring)
        def jsonSlurper = new JsonSlurperClassic()
        def robj = jsonSlurper.parseText(jsobSubstring)
        if (robj.status != 0) { error 'Config get defaultusername failed: ' + robj.message }
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
