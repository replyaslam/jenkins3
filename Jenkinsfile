#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
   
    stage('Example') {
	 echo env.BRANCH_NAME   
        if (env.BRANCH_NAME == 'develop') {
            echo 'this is develop branch'
        } else {
            echo 'I is elsewhere'
        }
    }	
	println "env.BRANCH_NAME:"
	println(env.BRANCH_NAME) 
	

    //JENKINS CREDENTIALS LAPTOP1 AND LAPTOP2-------------------------
        JWT_KEY_CRED_ID='4ce2a556-336a-4c23-b76d-724dc06045ea'  //this uses server.key from Laptop1
        //JWT_KEY_CRED_ID='5c813bda-f7d3-4c35-9edf-26076d1673e4'  //this uses server.key from Laptop2

        //org1 
        SFDC_HOST='https://login.salesforce.com'
        HUB_ORG='replyamijenkins1@yahoo.com'        

        CONNECTED_APP_CONSUMER_KEY='3MVG9xB_D1giir9ouqdpx6TLReZGNLSvnQPrlIUhn9d3LpPkwPmslUu8PjsPVAO6myXIqyttMQdpBi4ehe7yH'  //this uses server.crt from Laptop1	
        CONNECTED_APP_CONSUMER_KEY='3MVG9xB_D1giir9ouqdpx6TLRecCUlZczZHBwR5z65mioXYYHfGMM8CRXmth2HYqJKPiTxl._PYI9p7NyXuGm'	//this uses server.crt from Laptop2
	
	//new openSSL version keys
	CONNECTED_APP_CONSUMER_KEY='3MVG9xB_D1giir9ouqdpx6TLReXohLykpqvZ1GKJw6_bMafIgGaVD4JuCyLNCBGEHCR9dcSb2JmV6ARYRounJ'
        JWT_KEY_CRED_ID='01934d10-5a9b-4e35-a0cb-2742153196f5'

        //org2
        //SFDC_HOST='https://login.salesforce.com'
        //HUB_ORG='replyamijenkins2@yahoo.com'

        //CONNECTED_APP_CONSUMER_KEY='3MVG9xB_D1giir9rQ28.ZSOZMNzgZfh656KIRFlQOEp9Beiq2xm8ue4dInQ0XlHUOfWnghEK1jcDtNTGyCG9y' //this uses server.crt from Laptop1
        //CONNECTED_APP_CONSUMER_KEY='3MVG9xB_D1giir9rQ28.ZSOZMNwTMu9ioZWVzaHgYS4m9IgZSU_8f5K2KD0REdjYN3tQBs2EE6G3rxToF7ztY'  //this uses server.crt from Laptop2
        

        //org3
        //SFDC_HOST='https://login.salesforce.com'
        //HUB_ORG='replyamijenkins3@yahoo.com'
 
        //CONNECTED_APP_CONSUMER_KEY='3MVG9sh10GGnD4Dt2J6frnovQpib7_3a4OqWLfDNeK_DeZUDBCc7t28hpBqObv38In8w2p3MKdD2xa9u8f7F0'  //this uses server.crt from Laptop1
        //CONNECTED_APP_CONSUMER_KEY='3MVG9sh10GGnD4Dt2J6frnovQphUDU7pg3OZsQBvAgFCZgwkSqDkh4WOQ6mwGue9rTQeQ24554C9.tdjqiPKp'  //this uses server.crt from Laptop2
        


    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Deploye Code') {		
	    println "${toolbelt}"
	    println "${env.BRANCH_NAME}"
	    //println(env.BRANCH_NAME) 
		
            println 'this is Windows'
            println "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            
            //sfdx force:auth:logout --targetusername replyamijenkins3@yahoo.com -p
            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:logout --targetusername ${HUB_ORG}  -p"

            rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
			}else{
				println "toolbelt-AI:"
				println "\"${toolbelt}\" force:source:deploy --sourcepath manifest/. -u ${HUB_ORG}"
				
				println "\"${toolbelt}\" force:source:deploy --manifest manifest/. -u ${HUB_ORG}"
                		rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:deploy --manifest manifest/package.xml -u ${HUB_ORG}"
                		//rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
				println('AI-1')	
			}
			  
            //printf rmsg
            println('Hello from a Job DSL script!')
            //println(rmsg)
        }
    }
}
