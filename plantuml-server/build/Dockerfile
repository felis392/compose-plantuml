FROM plantuml/plantuml-server:jetty
ARG context_path
RUN mv /var/lib/jetty/webapps/ROOT.war /var/lib/jetty/webapps/$context_path.war
