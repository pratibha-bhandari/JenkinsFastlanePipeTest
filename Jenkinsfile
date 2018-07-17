node {

    //Wipe the workspace so that we are building completely clean
    //deleteDir()

    stage('Checkout') {

        // Checkout files.
        checkout([
            $class: 'GitSCM',
            branches: [[name: '*/master']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'CleanBeforeCheckout']], submoduleCfg: [],
            userRemoteConfigs: [[
                credentialsId: 'fe76b6f7-a7b2-4742-8ccc-62fc02e192c1',
                url: 'https://github.com/pratibha-bhandari/JenkinsFastlanePipeTest.git'
            ]]
        ])
    }

    /*stage('build') {
        // Build
        //sh 'security unlock-keychain -p niit@123 ${HOME}/Library/Keychains/login.keychain'

        //sh 'security set-key-partition-list -S apple-tool:,apple:,codesign: -s -k niit@123 login.keychain'

sh '/usr/bin/xcodebuild -scheme JenkinsFastlanePipeTest -configuration Release clean build archive -archivePath /Users/Shared/Jenkins/Home/workspace/JenkinsFastlanePipeTestGithub/build/Release-iphoneos/JenkinsFastlanePipeTest.xcarchive DEVELOPMENT_TEAM=UAWU67869T'

sh '/usr/bin/xcodebuild -exportArchive -archivePath /Users/Shared/Jenkins/Home/workspace/JenkinsFastlanePipeTestGithub/build/Release-iphoneos/JenkinsFastlanePipeTest.xcarchive -exportPath /Users/Shared/Jenkins/Home/workspace/JenkinsFastlanePipeTestGithub/build -exportOptionsPlist /Users/Shared/Jenkins/Home/workspace/JenkinsFastlanePipeTestGithub/build/developmentUAWU67869TExport.plist'
    }*/

    stage('fastlane') {
        sh 'whereis fastlane'

        dir ('/Users/Shared/Jenkins/Home/workspace/JenkinsFastlanePipeTestGithub') {
            fastlane("beta")
        }
       //sh 'fastlane("beta")'
    }

    stage('test') {
        //testing
        //sh 'xcodebuild -scheme "JenkinsFastlanePipeTest" -configuration "Debug" build test -destination "platform=iOS Simulator,name=iPhone 8,OS=11.2" -enableCodeCoverage YES | /usr/local/bin/xcpretty -r junit'
    }

    stage('post-build') {
        // Publish test restults.
        //step([$class: 'JUnitResultArchiver', allowEmptyResults: true, testResults: 'build/reports/junit.xml'])
    }

    stage('archive') {
        //Archiving artifacts
        archiveArtifacts '**'
    }
}

def fastlane(lane) {
def env = [
"PATH+LOCAL=/usr/local/bin/",
"HTTP_PROXY=http://dmzproxy.tech.rz.db.de:8080",
"HTTPS_PROXY=http://dmzproxy.tech.rz.db.de:8080",
"LC_ALL=en_US.UTF-8",
"LANG=en_US.UTF-8",
"FASTLANE_EXPLICIT_OPEN_SIMULATOR=2"
]

withEnv(env) {
timeout(time: 30, unit: 'MINUTES') {
wrap([$class: 'AnsiColorBuildWrapper', 'colorMapName': 'XTerm', 'defaultFg': 1, 'defaultBg': 2]) {
sh "fastlane ${lane}"
}
}
}
}

