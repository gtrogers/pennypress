FROM ubuntu:16.04
MAINTAINER grogers@thoughtworks.com

RUN apt-get update && apt-get install -y openssh-server supervisor git
RUN adduser --system git
RUN chsh -s /usr/bin/git-shell git
RUN mkdir -p /var/run/sshd /var/run/supervisord /var/log/supervisord /home/git/.ssh

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY sshd_config /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

# Setup SSH
COPY authorized_keys /home/git/.ssh/authorized_keys
RUN chown -R git /home/git/.ssh
RUN chmod go-w /home/git
RUN chmod 700 /home/git/.ssh
RUN chmod 600 /home/git/.ssh/authorized_keys

# Create git repo
RUN mkdir -p /home/git/data.git
RUN cd /home/git/data.git && git init --bare
RUN chown -R git /home/git/data.git

EXPOSE 2022
CMD ["/usr/bin/supervisord","-c","/etc/supervisor/conf.d/supervisord.conf"]
