# /flaskserver

## Files

```
./
│   app.py
│   Dockerfile
│   requirements.txt
```
### [/flaskserver/requirements.txt](/flaskserver/requirements.txt)
In this file is were there are listed all the libraries needed for the flask app.

### [/flaskserver/app.py](/flaskserver/app.py)
In this file is where i wrote down all the code of the flask app and the connection with redis. In this case redis was used just for store and retrieve data.

### [/flaskserver/Dockerfile](/flaskserver/Dockerfile)
This file were used to build a new image based on a python image from docker hub. Once the image were downloaded, copy the files app.py and requirements in the folder /app inside of the container. After that using the requirements file all the libraries neede were installed.

```
FROM python:3
MAINTAINER elmerreal98@gmail.com
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["app.py"]
```