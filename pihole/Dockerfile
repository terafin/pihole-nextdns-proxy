FROM debian:latest

EXPOSE 53
EXPOSE 53/udp
EXPOSE 80
EXPOSE 443

RUN apt-get update && apt-get install -fy git curl locales && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

RUN echo "resolvconf resolvconf/linkify-resolvconf boolean false" | debconf-set-selections
ENV LANG "en_US.utf8"
ENV BRANCH "release/v5.0"

RUN rm -rf /var/lib/apt/lists/*q


RUN mkdir -p /etc/pihole
RUN mkdir -p /var/runner
RUN mkdir -p /var/source
COPY run.sh /var/runner/run.sh
COPY setupVars.conf /etc/pihole/setupVars.conf

RUN chmod u+r /etc/pihole/setupVars.conf
RUN chmod guo+x /var/runner/run.sh

WORKDIR /var/source
RUN git clone -b "$BRANCH" --depth 1 https://github.com/pi-hole/pi-hole.git pihole


WORKDIR /var/source/pihole/automated\ install

RUN chmod +x basic-install.sh
RUN ./basic-install.sh --unattended

# HEALTHCHECK --interval=60s --timeout=10s --start-period=5s --retries=1 \
# 	CMD dig +time=20 @127.0.0.1 -p 53 probe-test.dns.nextdns.io && dig +time=20 @127.0.0.1 -p 8053 probe-test.dns.nextdns.io

CMD ["/var/nextdns/run.sh"]
