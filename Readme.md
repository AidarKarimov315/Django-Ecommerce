## Description

Visit the new version of the store at [habanerasdelino.com](https://habanerasdelino.com)

<a name="habaneras"></a>
### What is this project?

__Habaneras de Lino__ is an online store to buy linen and cotton clothes that offer an experience of comfort, luxury ,and modernity. The clients can filter the clothing by category, collection, and other characteristics, as well as customize the products (set color, size, sleeve cut, ...). It uses Stripe for managing the payments.

<a name="stack"></a>
### Tech Stack and Components

The main components of __Habaneras de Lino__ are:
- __Django and Django_Rest_Framework (This repo):__
  - __API(store_app folder):__  For managing user requests such as making CRUD operations over the store __Cart__ and making payments and orders. The code for this API can be found inside the __store_app__ folder of this repo.
  - __The store administration (admin_app folder):__ Intended to be used by the store administrator. It is different from Django's Admin, and it allows advanced filtering and CRUD operations over products, collections, configs, orders, payments, ... .
- __Stripe SDK (Third Party):__ For managing payments. It is accessed by Django when the user makes a purchase. [Link to Stripe](https://stripe.com).
- __Next.js and React.js store Frontend :__ The store UI that is visible to the clients was created using Next.js which connects to the Django API when making API calls and to Cloudinary when fetching the products', collections', and categories' images.
- __Cloudinary for storing images and as CDN (Third Party):__ Used for storing all images uploaded using the Django custom store administration. The images can be seen at the store's admin and in the store frontend by clients. [Link to Cloudinary](https://cloudinary.com).
- __PostgreSQL as database (Third Party):__ Connected to the Django API. Contains all the information about the store administration.

__Note:__ The Installation and the Useful Links sections contain more-in-detail information about each component.

<div align="center">

  ![Map of Services](.github_images/Map.png)

</div>

<a name="install"></a>
## Installation

<a name="docker_install"></a>
### Install (Run) with Docker

#### About the Builds and Images in use:
There are currently 3 services in use: the api (Django App), the db (the postgreSQL database), and the nginx (Nginx configuration).
    - __api:__ The Django Dockerfile is in the root directory, and it has an entrypoint file that connects the backend to the database and runs migrations as well as collects the statics.
    - __db:__ This is built from the postgres:13-alpine image. The default environment variables are set in the docker-compose.yml file.
    - __nginx:__ The default configuration for nginx is inside the nginx folder in the nginx.conf file.

#### Runing Docker-Compose

1. Clone the repo:
    ```bash
    git clone https://github.com/AidarKarimov315/Django-Ecommerce.git
    ```
1. Configure the environment variables.
    1. Copy the content of the example env file that is inside the DRF_API folder into a .env file:
        ```bash
        cd DRF_API/settings
        cp simple_env_conf.env .env
        ```
    1. The new .env file should contain all the environment variables necessary to run all the django app in all the environments. However, the only needed variables for docker to run are the following:
        ```bash
        DOCKER_SECRET_KEY
        DOCKER_DB_NAME
        DOCKER_DB_USER
        DOCKER_DB_PASSWORD
        DOCKER_DB_HOST
        DOCKER_DB_PORT
        DOCKER_STRIPE_PUBLISHABLE_KEY
        DOCKER_STRIPE_SECRET_KEY
        ```
    1. For the database, the default configurations should be:
        ```bash
        DOCKER_DB_NAME=docker_habanerasdelino_db
        DOCKER_DB_USER=docker_habanerasdelino_user
        DOCKER_DB_PASSWORD=docker_habanerasdelinouser!
        DOCKER_DB_HOST=db
        DOCKER_DB_PORT=5432
        ```
    1. The DOCKER_SECRET_KEY is the django secret key. To generate a new one see: [Stackoverflow Link](https://stackoverflow.com/questions/41298963/is-there-a-function-for-generating-settings-secret-key-in-django)

    1. The DOCKER_STRIPE_PUBLISHABLE_KEY and the DOCKER_STRIPE_SECRET_KEY can be obtained from a developer account in [Stripe](https://stripe.com/).
        - To retrieve the keys from a Stripe developer account follow the next instructions:
            1. Log in into your Stripe developer account (stripe.com) or create a new one (stripe.com > Sign Up). This should redirect to the account's Dashboard.
            1. Go to Developer > API Keys, and copy both the Publishable Key and the Secret Key.


1. Run docker-compose:
    ```bash
    docker-compose up --build
    ```
1. Congratulations =) !!! The App should be running in [localhost:8001](http://localhost:8001)
1. (Optional step) To create a super user run:
    ```bash
    docker-compose run api ./manage.py createsuperuser
    ```



<a name="docker_install"></a>
### Installation without Docker

1. Clone the repo:
    ```bash
    git clone https://github.com/AidarKarimov315/Django-Ecommerce.git
    ```
1. Configure a virtual env and set up the database. See [Link for configuring Virtual Environment](https://docs.python-guide.org/dev/virtualenvs/) and [Link for Database setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04).
1. Configure the environment variables.
    1. Copy the content of the example env file that is inside the DRF_API folder into a .env file:
        ```bash
        cd DRF_API/settings
        cp simple_env_conf.env .env
        ```
    1. The new .env file should contain all the environment variables necessary to run all the django app in all the environments. However, the only needed variables for the development environment to run are the following:
        ```bash
        SECRET_KEY
        DB_NAME
        DB_USER
        DB_PASSWORD
        DB_HOST
        DB_PORT
        STRIPE_PUBLISHABLE_KEY
        STRIPE_SECRET_KEY
        ```
    1. For the database, the default configurations should be:
        ```bash
        DB_NAME=habanerasdelino_db
        DB_USER=habanerasdelino_user
        DB_PASSWORD=habanerasdelinouser!
        DB_HOST=localhost
        DB_PORT=5432
        ```
    1. The SECRET_KEY is the django secret key. To generate a new one see: [Stackoverflow Link](https://stackoverflow.com/questions/41298963/is-there-a-function-for-generating-settings-secret-key-in-django)

    1. The STRIPE_PUBLISHABLE_KEY and the STRIPE_SECRET_KEY can be obtained from a developer account in [Stripe](https://stripe.com/).
        - To retrieve the keys from a Stripe developer account follow the next instructions:
            1. Log in into your Stripe developer account (stripe.com) or create a new one (stripe.com > Sign Up). This should redirect to the account's Dashboard.
            1. Go to Developer > API Keys, and copy both the Publishable Key and the Secret Key.

1. Run the migrations and then the app:
    ```bash
    python manage.py migrate
    python manage.py runserver
    ```
1. Congratulations =) !!! The App should be running in [localhost:8000](http://localhost:8000)
1. (Optional step) To create a super user run:
    ```bash
    python manage.py createsuperuser
<a name="deploy"></a>
## Deploy on VPS
1. Clone the repo:
    ```bash
    git clone https://github.com/AidarKarimov315/Django-Ecommerce.git
    ```
1. Install the dependencies:
    ```bash
    sudo apt-get update
    sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx
    ```
1. Set up the postgresql database [Setup Database](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04)
1. Create an .env file and configure the environment variables
1. Create a virtual env and activate it:
    ```bash
    virtualenv myprojectenv
    source myprojectenv/bin/activate
    ```
1. Pip install the requirements:
    ```bash
    pip install -r requirements.txt
    ```
1. Pip install gunicorn:
    ```bash
    pip install gunicorn
    ```
1. Run the migrations and then test the the app:
    ```bash
    python manage.py migrate
    python manage.py runserver
    ```
1. Complete the setup of the website with this [Link](https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04)
1. Configure the CORS to allow the Frontend to make api calls. See [Link](https://www.stackhawk.com/blog/django-cors-guide/)