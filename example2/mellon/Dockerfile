ARG APACHE_VERSION=2.4

FROM httpd:${APACHE_VERSION} as build

ARG MELLON_VERSION=0.14.2

RUN apt-get update && \
  apt-get install build-essential apache2-dev liblasso3 pkg-config liblasso3-dev libcurl4-openssl-dev curl libxmlsec1 libxml2-dev libxmlsec1-dev ca-certificates -y

# Compile & install mod_auth_mellon
RUN mkdir /tmp/mod_auth_mellon && \
  curl -L https://github.com/latchset/mod_auth_mellon/archive/v${MELLON_VERSION}.tar.gz | tar -xvz -C /tmp/mod_auth_mellon --strip 1 && \
  cd /tmp/mod_auth_mellon && \
  autoreconf && \
  ./configure --enable-diagnostics --with-apxs2=/usr/local/apache2/bin/apxs && \
  make && \
  make install

FROM httpd:${APACHE_VERSION}
RUN apt update && apt install openssl liblasso3 libxmlsec1 curl -y



# copy mellon_create_metadata.sh
COPY --from=build /tmp/mod_auth_mellon/mellon_create_metadata.sh /usr/sbin/

COPY --from=build /usr/local/apache2/modules/mod_auth_mellon.so /usr/local/apache2/modules/mod_auth_mellon.so

RUN echo " Include conf.d/saml.conf" >> /usr/local/apache2/conf/httpd.conf

# ENV fqdn=samltest.org
# ENV mellon_endpoint_url="https://${fqdn}/mellon"
# ENV mellon_entity_id="${mellon_endpoint_url}/metadata"
# RUN file_prefix="$(echo "$mellon_entity_id" | sed 's/[^A-Za-z.]/_/g' | sed 's/__*/_/g')"

# RUN chmod +x /usr/sbin/mellon_create_metadata.sh 

# RUN /usr/sbin/mellon_create_metadata.sh "https://samltest.org/mellon/metadata" "https://samltest.org/mellon"

# RUN echo ${PWD}
# RUN mkdir mellon

# RUN mv ${PWD}/https_samltest.org_mellon_metadata.cert /usr/local/apache2/mellon/mellon.cert
# RUN mv ${PWD}/https_samltest.org_mellon_metadata.key /usr/local/apache2/mellon/mellon.key
# RUN mv ${PWD}/https_samltest.org_mellon_metadata.xml /usr/local/apache2/mellon/mellon_metadata.xml


