timestamps {
node() {

	stage("prepare image") {
		scm checkout
		echo "inhalt einer datei" > datei.txt
	}
    def url
    stage("create instance") {
        withAWSCredentials([credentials: 'aws']) {
            withEC2Instance(terminate:false) {
                echo "Deploying to server ${PUBLIC_DNS_NAME}"
                url = "tcp://${PUBLIC_DNS_NAME}:4243"
            }
        }
    }
    stage("start jenkins") {
        docker.withServer(url) {
            def myImage = docker.image('jenkins/jenkins')
            myImage.pull()
            myImage.run('-p 8080:8080')
        }
    }
}}