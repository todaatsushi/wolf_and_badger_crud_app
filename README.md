
# Wolf & Badger CRUD app

This app allows users to create, retrieve, update and delete their information/profiles onto the site with the purpose
of showing off their professional skills. Using Oauth, users can also have a profile using their GitHub accounts.

Built with Django and PostgreSQL or Sqlite3 as the database. The app also uses Docker for deployment, Selenium & Django's
test functionality for testing.


## Getting Started

These instructions will get you a copy of the project up and running on your local machine.

It's recomended that you use Docker to get it running but instructions to run it using virtualenv are also included.


### Installing
## Docker
To build:
```
docker-compose build
```

To run:
```
docker-compose up
```

## Virtualenv

Using a virtual environment, install dependencies with:
```
pip install -r requirements.txt
```

To run:
```
python manage.py runserver
```


### Setting up backend
To initialise the database, run:
```
docker-compose run web python manage.py migrate
```

To create a superuser:
```
docker-compose run web python manage.py createsuperuser
```

### Setting up Oauth with Django (GitHub)
[Complete tutorial](https://wsvincent.com/django-allauth-tutorial/)

#### Quick Summary
- Make a new application on [GitHub](https://github.com/settings/applications/new)
- Migrate your database
- Make a new `Site` on `Django admin`
- Make a new `Social Application` on `Django admin` using your GitHub application `Client ID` and `Secret Key`.

#### Oauth Testing
Create fixtures for testing Oauth by creating a fixture using the command below.

```
docker-compose run web python manage.py dumpdata --indent 2 --natural-primary --natural-foreign -e contenttypes -e auth.Permission > users/fixtures/allauth_fixture.json
```

#### IMPORTANT FOR TESTING
In your `allauth_fixture.json`, make sure both the SocialApp and Site models share the same `id` - this should also be the same one listed in your `settings.py` file as your `SITE_ID`.

```
# allauth_fixture.json
...
    {
        "model": "sites.site",
        "pk": 1,
        "fields": {
            "domain": "localhost",
            "name": "example.com"
        }
        },
        {
        "model": "socialaccount.socialapp",
        "pk": 1,
        "fields": {
            "provider": "github",
            "name": "github",
            "client_id": "foo",
            "secret": "bar",
            "key": "",
            "sites": [
            [
                "localhost"
            ]
            ]
        }
    },
...
```


### .env variables
* SECRET_KEY - Django secret key
* GITHUB_CLIENT_ID - App client ID generated in the steps above
* GITHUB_CLIENT_SECRET - App client secret key generated in the steps above
* GITHUB_USERNAME - GitHub username for testing
* GITHUB_PASSWORD - GitHub password for testing


## Built With

* [Django](https://docs.djangoproject.com/en/2.2/) - The web framework used


## Authors

* **Me, Atsushi Toda** - [GitHub](https://github.com/todaatsushi) - [My personal website](https://www.atsushi.dev)

## License

This project is licensed under the MIT License.
