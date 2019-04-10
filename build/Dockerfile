# build from operator base @ quay.io
FROM quay.io/operator-framework/ansible-operator
MAINTAINER Chris Ruffalo <cruffalo@redhat.com>

# copy watch yaml
COPY watches.yml ${HOME}/watches.yml 

# copy playbooks
COPY ./ansible /opt

