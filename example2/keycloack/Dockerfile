FROM jboss/keycloak

ARG KEYCLOAK_ADMIN_USER=admin
ARG KEYCLOAK_ADMIN_PASWORD=admin
ARG KEYCLOAK_IMPORT_REALM
ENV KEYCLOAK_IMPORT_REALM ${KEYCLOAK_IMPORT_REALM:-realm-export.json}

RUN /opt/jboss/keycloak/bin/add-user.sh -u ${KEYCLOAK_ADMIN_USER} -p ${KEYCLOAK_ADMIN_PASWORD}

RUN echo "${KEYCLOAK_IMPORT_REALM}"
ADD ${KEYCLOAK_IMPORT_REALM} /opt/jboss/keycloak/s

ENTRYPOINT ["/bin/bash","-c", "/opt/jboss/keycloak/bin/standalone.sh" ]

CMD ["-b", "0.0.0.0", "echo","-Dkeycloak.import=/opt/jboss/keycloak/$KEYCLOAK_IMPORT_REALM"]