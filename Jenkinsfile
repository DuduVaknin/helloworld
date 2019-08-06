//////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////
//                                                                  //
// author: David Vaknin                                             //
// last updated: 08/08/19                                           //
// email: david@vaknin.io                                           //  
//                                                                  //
//////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////

node('master') {

    // clean work-space
    cleanWs deleteDirs: true

    //get tinestamp
    def now = new Date()
    env.TIME=now.format("yyyyMMdd-HH:mm:ss.SSS", TimeZone.getTimeZone('UTC'))
    
    def app

    stage('Clone repository') {
        /* repository will clone to our workspace */
        checkout([$class: 'GitSCM', branches: [[name: 'master']],
        doGenerateSubmoduleConfigurations: false,
        extensions: [],
        submoduleCfg: [],
        userRemoteConfigs: [[credentialsId: 'duduvaknin', url: 'https://github.com/DuduVaknin/helloworld.git']]])
    }

    stage('Build image') {
        /* This builds the actual image; helloworld:latest
         * docker build on the command line */

        app = docker.build("helloworld")
    }

    // stage('Test image') {
    //     /* Ideally, we would run a test framework against our image.
    //      * For this example, we're using a Volkswagen-type approach ;-) */

    //     app.inside {
    //         sh 'echo "Tests passed"'
    //     }
    // }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com/duduvaknin/vcita_helloworld', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}

//this method run shell command
def runCommand( command ) {
    ansiColor('xterm') {
        if(isUnix()) {
            sh command
        } else {
            bat command
        }
    }
}

//this method send email with build result
def emailExt (result, zipToEmail) {
     emailext (body: '''${SCRIPT, template="buildlog.template"}''',
        mimeType: 'text/html',
        subject: "[Jenkins] - Build "+ result,
        attachmentsPattern: zipToEmail,
        to: params.MailRecipients,
        replyTo: params.MailRecipients,
        recipientProviders: [[$class: 'CulpritsRecipientProvider']])

 } 

//this method define the build result and send the result to emailExt function
def emailExtRun () {
     if (currentBuild.currentResult == 'SUCCESS') {
            
                emailExt ('SUCCESS', 'reports.7z')
            }
            else if (currentBuild.currentResult == 'UNSTABLE') { 
             
                emailExt ('UNSTABLE', 'reports.7z')
            }
            else if (currentBuild.currentResult == 'FAILURE') { 
                    
                emailExt ('FAILURE', 'reports.7z')
            }
 }
 