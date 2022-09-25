FROM centos:7

RUN yum -y install openssh-server

RUN useradd remote_user && \
  echo "1234" | passwd remote_user --stdin && \
  mkdir /home/remote_user/.ssh && \
  chmod 700 /home/remote_user/.ssh

COPY ./remote-key-2.pub /home/remote_user/.ssh/authorized_keys

RUN chown remote_user:remote_user -R /home/remote_user/.ssh/ && \
  chmod 600 /home/remote_user/.ssh/authorized_keys

RUN /usr/sbin/sshd-keygen


# Install Docker
RUN  yum install -y yum-utils && \
     yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
RUN yum -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
COPY systemctl.py /usr/bin/systemctl
RUN chmod a+x /usr/bin/systemctl

# Compose
RUN curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose

RUN usermod -aG docker remote_user

CMD /usr/sbin/sshd -D