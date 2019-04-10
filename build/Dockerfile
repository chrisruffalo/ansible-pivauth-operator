# build from operator base @ quay.io
FROM quay.io/operator-framework/ansible-operator
MAINTAINER Chris Ruffalo <cruffalo@redhat.com>

# install OC client
RUN curl -L https://mirror.openshift.com/pub/openshift-v3/clients/4.0.22/linux/oc.tar.gz | tar xz && \
    mv oc /usr/local/bin/oc

# copy watch yaml
COPY watches.yml ${HOME}/watches.yaml 

# copy playbooks
COPY ./ansible ${HOME}

