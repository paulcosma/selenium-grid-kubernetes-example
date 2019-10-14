podTemplate(cloud: 'gcp-k8s', label: 'maven-selenium', name: 'maven-selenium', containers:[
    containerTemplate(name:'maven-chrome', image:'maven:3.6.2-jdk-8', ttyEnabled:true, command:'cat'),
    containerTemplate(name:'maven-chrome2', image:'maven:3.6.2-jdk-8', ttyEnabled:true, command:'cat'),
    containerTemplate(name:'selenium-hub', image:'selenium/hub:3.141.59', ttyEnabled:true),
    //container run in the same network space. make sure there are no ports conflicts.
    containerTemplate(
        name:'selenium-chrome',
        image:'selenium/node-chrome:3.141.59',
        envVars: [
            envVar(key: 'HUB_HOST', value: 'localhost'),
            envVar(key: 'HUB_PORT', value: '4444')
        ]
    ),
    containerTemplate(
        name:'selenium-chrome2',
        image:'selenium/node-chrome:3.141.59',
        envVars: [
            envVar(key: 'HUB_HOST', value: 'localhost'),
            envVar(key: 'HUB_PORT', value: '4444')
        ]
    )
    ]){
    node ('maven-selenium') {
        stage('Checkout') {
            git url:"https://github.com/paulcosma/selenium-k8s-example.git"
            parallel (
                chrome: {
                    container('maven-chrome'){
                        stage("Test chrome") {
                            sh 'sleep 30'
                            sh 'mvn -B clean test -Dselenium.browser=chrome -Dsurefire.rerunFailingTestCount=5 -Dsleep=0'
                        }
                    }
                },
                chrome2: {
                    container('maven-chrome2'){
                        stage("Test chrome2") {
                            sh 'sleep 30'
                            sh 'mvn -B clean test -Dselenium.browser=chrome -Dsurefire.rerunFailingTestCount=5 -Dsleep=0'
                        }
                    }
                }
            )
        }

        stage('Logs'){
            containerLog('selenium-chrome')
        }
   }
}