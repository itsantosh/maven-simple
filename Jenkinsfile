stage('Release and publish artifact') {
            when {
                // check if branch is master
                branch 'master'
            }
            steps {
                // create the release version then create a tage with it , then push to nexus releases the released jar
                script {
                    def mvnHome = tool 'Maven 3.3.9' //
                    if (currentBuild.result == null || currentBuild.result == 'SUCCESS') {
                        def v = getReleaseVersion()
                        releasedVersion = v;
                        if (v) {
                            echo "Building version ${v} - so released version is ${releasedVersion}"
                        }
                        // jenkins user credentials ID which is transparent to the user and password change
                        sshagent(['0000000-3b5a-454e-a8e6-c6b6114d36000']) {
                            sh "git tag -f v${v}"
                            sh "git push -f --tags"
                        }
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true  versions:set  -DgenerateBackupPoms=false -DnewVersion=${v}"
                        sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true clean deploy"

                    } else {
                        error "Release is not possible. as build is not successful"
                    }
                }
            }
        }
