LimeSurvey
==========

LimeSurvey - the most popular
Free Open Source Software survey tool on the web.

https://www.limesurvey.org/en/

This docker image easies limesurvey installation. It includes a MySQL database as well a web server.

Forked from [crramirez](https://github.com/crramirez/limesurvey).

## Usage

To run limesurvey in 80 port do as follows:
1. `docker pull cbarnett/limesurvey:latest`
2. Either start a docker container or use docker-compose:
  - container:
      - `docker run -d --name limesurvey -p 80:80 cbarnett/limesurvey:latest`
  - use docker-compose (I prefer and recommend this way):
      - get [docker-compose.yml](https://github.com/chrisbarnettster/limesurvey/blob/master/docker-compose.yml)
      - `docker-compose up`

4. Go to a browser and type http://localhost
5. Click Next until you reach the *Database configuration* screen
6. Then enter the following in the field:
  - **Database type** *MySQL*
  - **Database location** *localhost*
  - **Database user** root*
  - **Database password**
  - **Database name** *limesurvey* #Or whatever you like
  - **Table prefix** *lime_* #Or whatever you like

You are ready to go.

## Database in volumes

** Note : if you used docker-compose this is all sorted already **

If you want to preserve data in the event of a container deletion, or version upgrade, you can assign the MySQL data into a named volume:

    docker volume create --name mysql
    docker run -d --name limesurvey -v mysql:/var/lib/mysql -p 80:80 cbarnett/limesurvey:latest


If you delete the container simply run again the above command. The installation page will appear again. Don't worry just put the same parameters as before and limesurvey will recognize the database.


## Upload folder

** Note : if you used docker-compose this is all sorted already **

If you want to preserve the uploaded files in the event of a container deletion, or version upgrade, you can assign the upload folder into a named volume:

    docker volume create --name upload
    docker run -d --name limesurvey -v upload:/app/upload -v mysql:/var/lib/mysql -p 80:80 cbarnett/limesurvey:latest


If you delete the container simply run again the above command. The installation page will appear again. Don't worry just put the same parameters as before and limesurvey will recognize the database and the uploaded files including images.

## Using Docker Compose

You can use docker compose to automate the above command if you create a file called *docker-compose.yml* and put in there the following:

```
      version: '2'
      services:
        limesurvey:
          ports:
            - "80:80"
          volumes:
            - mysql:/var/lib/mysql
            - upload:/app/upload
          image:
            cbarnett/limesurvey:latest
      volumes:
        mysql:
        upload:
```

To run: `docker-compose up -d`

The -d flag is Detached mode i.e. Run containers in the background ,detaches the output.

To stop: `docker-compose stop`
