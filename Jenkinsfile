node {
    def buildNo = env.BUILD_NUMBER
    def imageName = 'standings'

    stage 'Checkout'
    checkout scm

    stage 'Build'
    def gradleHome = tool 'Gradle'
    sh "${gradleHome}/bin/gradle clean build"

    junit 'build/test-results/*.xml'

    stage 'Sonar'
    echo "Do Sonar analysis here"
    Random r = new Random()
    int v = r.nextInt(3)
    if (v == 0) {
      echo "You have great code!"
    }
    else if (v == 1) {
      echo "You have excellent code!"
    }
    else if (v == 2) {
      echo "You have awesome code!"
    }

    stage 'Build Docker image'
    sh "${gradleHome}/bin/gradle prepDocker -x build"
    def image = docker.build("${imageName}:${buildNo}", 'build/docker')

    stage 'Run system tests'
    image.withRun('-p 8888:8888 --add-host="vogelhost:169.254.255.254"') { c->
        sh "${gradleHome}/bin/gradle systemTest"
    }
}
