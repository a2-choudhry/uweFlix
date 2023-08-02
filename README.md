# UWEFlix
UWEFLIX is a Docker containerized Django project that powers an online cinema booking system. This application provides a userfriendly website where it can easily access movie listings, show timings, and other essential information for booking their favorite movies at cinemas. Leveraging the power of containerization through Docker, UWEFLIX ensures seamless deployment and scalability, making it a reliable and efficient solution for cinema management and moviegoers alike

Program is written in Python and integrated with Docker, Django and MySQL.

To access this, please download the zip file and run this accordingly:


## Run
```sh
$ docker compose up
$ docker ps # check are the 2 containers running (db, app)
```

App should be available at:
- http://localhost:8000
- http://localhost:8000/admin

## Useful commands

```sh
$ docker compose build # rebuild images when changing the Dockerfile
$ docker compose down && docker volume prune # stop containers and delete the data
$ docker system prune --all # remove all stopped containers and images
```

From within the container.
```sh
$ docker exec -it app bash
python manage.py
python manage.py makemigrations
python manage.py migrate
python manage.py loaddata initial.yaml
python manage.py dumpdata --format=yaml > ./dump_file.yaml
```

## Users

There are number of users, the only superuser added is:

```
username: superuser
username: cinema_manager1
username: club_manager1
username: customer1
username: student1

password: NnwNeE88
```


## Design

- Theaters can have multiple screens.
- Screen can have only 1 movie assigned.
- Screen type has been added to handle the ticket prices.
- Screen is occupied if the movie is assigned within the "starts_at" and "ends_at" date.
- Same movie can be added to different screens at the same time.
- There is one category per movie.
- When the movie is added to the screen, the tickets are generated behind the scenes without customer data, meaning they are not yet sold.
- When the movie is removed from the screen, the tickets are removed behind the scenes regardless of if they have been sold or assigned to the customer.
- Movie becomes inactive when the current time passes the "ends_at" date on save.
- Django User model has been extended with extra fields and roles.
- Roles are tied to groups. Setting the role sets the group with the same name to the user for easier management.
- Django Group model has been extended with an extra "is_active" field and 4 groups have been added to the initial data fixture:
  - Cinema Manager:
    - can handle any cinema related records, it should not be mistaken with superuser and handling of all admin records like sessions
    - can view and buy tickets with 10% discount
  - Club Manager: can view and buy tickets with 10% discount
  - Customer: can view and buy tickets without discount
  - Student: no functionality yet

## Future possibilities
- The "screen.is_occupied" and "movie.is_active" are handled on save which in real life would not be enough. As the time passes, every minute we would need to have a running background task which would update those values accordingly.
- It could be possible to assign the same movie multiple times to one screen during different times. e.g. Run the same movie from 18-20 and 20-22 hours.
- Movies could be added to multiple categories.
- Removing the movie from the screen should not delete the sold tickets or there should be an UI warning.
- Changing the number of seats for the screen should add or remove unsold tickets.
- Groups "is_active" doesn't have functionality at this point.
- Some of the users like customers and students might not need to be the staff members. In those cases the registration, login and ticket handling could be done outside of django administration.
- Frontend speed optimization, minimization, etc..
- Tests.

