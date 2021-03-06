# Copyright 2014 Google Inc.
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

FROM gce-jetty:9.3.2.v20150730

ADD . $JETTY_BASE

WORKDIR $JETTY_BASE
RUN sed -i 's/^\([a-zA-Z\.]*=\).*/#\1(see gae.ini)/' start.d/server.ini \
 && rm -f start.d/setuid.ini start.d/deploy.ini \
 && java -jar $JETTY_HOME/start.jar --add-to-startd=gae,gae-deploy \
 && java -jar $JETTY_HOME/start.jar --dry-run \
  | sed 's/^.*java /& ${JAVA_OPTS} ${DBG_AGENT} ${PROF_AGENT} -Xms${HEAP_SIZE} -Xmx${HEAP_SIZE} /' > jetty_cmd.sh
    
WORKDIR /app

# Jetty HTTP port number
EXPOSE 8080
# Java VM debug port number
EXPOSE 5005

# Env variable used by the jetty_run.sh script
ENV RUNTIME_DIR /var/lib/jetty

# Clean any CMD that might be inherited from previous image, because that
# will pollute our ENTRYPOINT, see
# http://docs.docker.io/en/latest/reference/builder/#entrypoint.
CMD []
VOLUME ["/var/log/app_engine"] 
ENTRYPOINT ["/var/lib/jetty/jetty_run.sh"]
