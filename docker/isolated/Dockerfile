FROM dockerhost/docker-repo/awx/awx_task:docker_tag

ENV UID=1000
ENV HOMEDIR=ansible

USER 0

RUN dnf install -y gcc python36-devel
RUN python3 -m ensurepip && \
    pip3 install "virtualenv < 20" ansible-runner

RUN dnf remove -y gcc python36-devel && \
    dnf clean all && \
    rm -rf /var/cache/dnf

RUN rm -f /etc/ssh/*_key
RUN ssh-keygen -q -A -N ""
RUN sed -i "s/UsePAM.*/UsePAM yes/g" /etc/ssh/sshd_config

RUN useradd -u ${UID} -d /home/${HOMEDIR} awx && \
    mkdir /home/${HOMEDIR}/.ssh && \
    chmod 700 /home/${HOMEDIR}/.ssh && \
    chown awx:awx /home/${HOMEDIR}/.ssh && \
    mkdir /var/lib/awx/.ansible && \
    chown awx:awx /var/lib/awx/.ansible

RUN rm -rf /run/nologin

EXPOSE 22

ENTRYPOINT ["tini", "--"]
CMD ["/usr/sbin/sshd", "-D", "-e"]
