FROM 3.9-eclipse-temurin-21-alpine as BUILD
WorkDIR /app
COPY ../pom.xml  .
COPY ../src .

RUN mvn clean package

FROM jre21-temurin-jammy

COPY --from=BUILD /app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

CMD ["catalina.sh", "run"]
