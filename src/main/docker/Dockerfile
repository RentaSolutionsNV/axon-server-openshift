# Copyright (c) 2018 by Axoniq B.V. - All rights reserved
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
FROM registry.access.redhat.com/openjdk/openjdk-11-rhel7

LABEL maintainer="Fabian Dammekens <fabian.dammekens@cegeka.com>"

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.axoniq.server.version="4.2" \
      io.k8s.description="Zero-configuration message router and event store for Axon" \
      io.k8s.display-name="axon-server-openshift" \
      io.openshift.expose-services="8024:http,8124:http" \
      io.openshift.tags="axon,server"

USER root

ENV AXONSERVER_HOME /opt/axonserver
RUN mkdir -p ${AXONSERVER_HOME}/data

RUN yum -y install unzip wget \
    && cd ${AXONSERVER_HOME} \
    && wget -q https://download.axoniq.io/axonserver/AxonServer.zip
RUN cd ${AXONSERVER_HOME} && unzip AxonServer.zip axonserver-4.2.4/axonserver.jar axonserver-4.2.4/axonserver-cli.jar \
    && mv ${AXONSERVER_HOME}/axonserver-4.2.4/*.jar . \
    && rm -rf ${AXONSERVER_HOME}/axonserver-4.2.4/ ${AXONSERVER_HOME}/AxonServer.zip

COPY src/main/docker/axonserver.properties ${AXONSERVER_HOME}/axonserver.properties
COPY src/main/docker/startup.sh ${AXONSERVER_HOME}/startup.sh

COPY ["AXONIQ OPEN SOURCE LICENSE.txt", "${AXONSERVER_HOME}/AXONIQ OPEN SOURCE LICENSE.txt"]
COPY ["LICENSE", "${AXONSERVER_HOME}/APACHE LICENSE v2.0.txt"]

RUN chmod 755 ${AXONSERVER_HOME}/startup.sh \
    && chmod g+w ${AXONSERVER_HOME}/ ${AXONSERVER_HOME}/axonserver.properties \
    && chgrp -R 0 ${AXONSERVER_HOME}/ \
    && yum -y remove unzip wget \
    && yum clean all

USER 1001

EXPOSE 8024
EXPOSE 8124
VOLUME [ "${AXONSERVER_HOME}/data" ]
VOLUME [ "${AXONSERVER_HOME}/log" ]

WORKDIR ${AXONSERVER_HOME}
CMD [ "/bin/sh", "-c", "${AXONSERVER_HOME}/startup.sh" ]
