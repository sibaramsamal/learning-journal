# AEM 6.5 Project Run & Build Commands

- Start in normal mode:  
  `java -jar aem6.5-author-p4502.jar`

- Start in debug mode:  
  `java -Xdebug -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8888 -jar aem6.5-author-p4502.jar`  
  *(Debug flags: `-Xdebug` enables debug, `transport=dt_socket` uses socket, `server=y` waits for debugger, `suspend=n` doesn’t pause startup, `address=8888` is the port.)*

- Clean project:  
  `mvn clean`

- Build project:  
  `mvn clean install` - to clean all the modules and delete target folders.

- Build & deploy to AEM:  
  `mvn clean install -PautoInstallPackage`

- Build & deploy to AEM (specific author port):  
  `mvn clean install -PautoInstallPackage -Daem.port=4502`

- Build & deploy to publisher:  
  `mvn clean install -PautoInstallPackagePublish`
