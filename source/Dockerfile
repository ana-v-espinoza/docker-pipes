FROM rockylinux:latest

RUN useradd -ms /bin/bash rocky
RUN echo "rocky ALL=NOPASSWD: ALL" >> /etc/sudoers
ENV HOME /home/rocky
ENV USER rocky

RUN mkfifo -m u=rw $HOME/pipe

WORKDIR $HOME
USER rocky

ENTRYPOINT [ "/bin/bash" ]
