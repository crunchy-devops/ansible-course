FROM python:3.6.15

WORKDIR /tmp/ansible-course

RUN apt update && \
    apt -y install ansible && \
    wget https://networkgenomics.com/try/mitogen-0.2.9.tar.gz  && \
    tar -zxvf mitogen-0.2.9.tar.gz 

CMD [ "tail", "-f", "/dev/null"  ]


