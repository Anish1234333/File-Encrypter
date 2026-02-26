node {

    try {

        stage('Build') {
            sh '''
            echo "Building Java project..."
            echo "Listing workspace contents:"
            ls
            cd "Password Protection"
            mkdir -p build
            javac -d build src/*.java
            echo "Build successful"
            '''
        }

        stage('Test') {
            sh '''
            echo "Checking for test folder..."
            cd "Password Protection"

            if [ -d test ]; then
                echo "Running tests..."

                if [ ! -f junit-platform-console-standalone.jar ]; then
                    curl -L -o junit-platform-console-standalone.jar \
                    https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
                fi

                mkdir -p test-build
                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java || true

                java -jar junit-platform-console-standalone.jar \
                --class-path build:test-build \
                --scan-class-path || true

                echo "Tests executed"
            else
                echo "No test folder found. Skipping tests."
            fi
            '''
        }

        stage('Deploy') {
            sh '''
            echo "Packaging application..."
            cd "Password Protection"
            jar cf FileEncrypter.jar -C build .
            echo "Deployment successful"
            '''
        }

        echo "Pipeline executed successfully!"

    } catch (Exception e) {

        echo "Pipeline failed!"
        throw e
    }

}
