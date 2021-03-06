FROM openjdk:8-alpine

LABEL maintainer="David Sperling <dsperling@smithmicro.com>"

ENV JMETER_VERSION apache-jmeter-4.0
ENV JMETER_HOME /opt/$JMETER_VERSION
ENV PATH $JMETER_HOME/bin:$PATH

# overridable environment variables
ENV RESULTS_LOG results.jtl
ENV JMETER_FLAGS=

# Install the required tools for JMeter
RUN apk add --update --no-cache \
    curl \
    openssh-client \
    unzip

WORKDIR /opt

# install JMeter and a few Plugins
RUN curl -O https://archive.apache.org/dist/jmeter/binaries/$JMETER_VERSION.tgz \
  && tar -xvf $JMETER_VERSION.tgz \
  && rm $JMETER_VERSION.tgz \
  && rm -rf $JMETER_VERSION/docs $JMETER_VERSION/printable_docs \
  && curl -O https://jmeter-plugins.org/files/JMeterPlugins-Standard-1.4.0.zip \
    -O https://jmeter-plugins.org/files/JMeterPlugins-Extras-1.4.0.zip \
    -O https://jmeter-plugins.org/files/JMeterPlugins-ExtrasLibs-1.4.0.zip \
  && unzip -n '*.zip' \
  && rm *.zip

# copy our entrypoint
COPY entrypoint.sh /opt/jmeter/
RUN chmod +x /opt/jmeter/entrypoint.sh

WORKDIR /logs

EXPOSE 1099 50000 51000 4445/udp

# default command in the entrypoint is 'minion'
ENTRYPOINT ["/opt/jmeter/entrypoint.sh"]
CMD ["minion"]
