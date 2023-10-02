FROM python:3.6.15

RUN mkdir /tmp/ansible-course 
WORKDIR /tmp/ansible-course

RUN apt update && \
    apt -y install vim && \
    pip3 install --user ansible==2.9

CMD [ "tail", "-f", "/dev/null"  ]


