node {
    // Here, we create a container with JDK 8 and Gradle
    def myGradleContainer = docker.image('gradle:jdk8-alpine')
    // We need to pull the image needed to build the container
    myGradleContainer.pull()

    // We create a new Jenkins stage in our pipeline
    // ------------------ Stage: Prep ------------------ //
    stage('prep') {
    // We have to pull from the GitHub repo inside of our container
        checkout scm
    }
    // ---------------- END:Stage: Prep ---------------- //

    // ------------------ Stage: Test ------------------ //
    stage('test') {
        // We create a volume that will link the .gradle to a .gradle folder in our new container
        // @see docker -v
        myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
            sh 'cd complete && ./gradlew test'
        }
    }
    // ---------------- END:Stage: Test ---------------- //

    // ------------------ Stage: Run ------------------ //
    stage('run') {
        myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
            sh 'cd complete && ./gradlew run'
        }
    }
    // ---------------- END:Stage: run ---------------- //
}