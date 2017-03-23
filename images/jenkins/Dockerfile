FROM jenkins:2.32.3
RUN /usr/local/bin/install-plugins.sh \
    blueocean:1.0.0-rc2 \
    credentials-binding:1.10 \
    git:3.1.0 \
    github:1.26.1 \
    github-organization-folder:1.6 \
    kubernetes:0.11 \
    workflow-aggregator:2.5 \
    && mkdir -p /usr/share/jenkins/ref/secrets/ \
    && echo "false" > /usr/share/jenkins/ref/secrets/slave-to-master-security-kill-switch
