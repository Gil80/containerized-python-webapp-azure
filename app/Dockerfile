# Pick a low configuration python base image
FROM python:alpine
ENV PYTHONUNBUFFERED=1
ENV FLASK_APP=app.py
ENV FLASK_ENV=development
RUN apk add --update --no-cache python3 && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools

# create and activate virtual environment
# using final folder name to avoid path issues with packages
RUN python3 -m venv /home/ubtech/venve
ENV VIRTUAL_ENV=/home/ubtech/venve
ENV PATH="/home/ubtech/venve/bin:$PATH"
RUN source /home/ubtech/venve/bin/activate

# create user without password
RUN adduser ubtech -D

WORKDIR /home/ubtech/app

# Create a folder app in container and work inside it
WORKDIR /app

COPY . .

# Install bash
RUN apk add --no-cache bash

# Install all the requirements
RUN python3 -m pip install --upgrade pip
#COPY ./requirements.txt .
RUN pip3 install -r requirements.txt


# Expose the port 5000 of the docker container
EXPOSE 5000

# Execute flask application inside the container
#CMD python3 app.py

CMD flask --app app run --host=0.0.0.0
