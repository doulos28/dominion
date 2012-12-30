Dominion Randomizer Webapp
---------------------------

Overview
========
It's useful to have a very quick randomizer when playing the [Dominion card game](http://www.riograndegames.com/games.html?id=278). There are many existing randomizer websites, but I am not satisfied; I want to strive to make the user interaction better. The goal of this randomizer is to make randomizing a desired deck of cards as fast and easy as possible.

API
===
You can get details about any dominion card by submitting a GET request to [dominion.olinapps.com/api/cards](http://dominion.olinapps.com/api/cards), and [dominion.olinapps.com/api/cards/card_id](http://dominion.olinapps.com/api/cards/1) (where card_id is the integer id of a card you want, e.g. [dominion.olinapps.com/api/cards/100](http://dominion.olinapps.com/api/cards/100)). You can make a GET request to [dominion.olinapps.com/api/expansions](http://dominion.olinapps.com/api/expansions) to get the expansions.

Filter results using these optional parameters:

```
    "min_cost_treasure"
    "max_cost_treasure"
    "min_cost_potion"
    "max_cost_potions"
    "min_plus_actions"
    "max_plus_actions"
    "min_plus_treasure"
    "max_plus_treasure"
    "min_plus_cards"
    "max_plus_cards"
    "min_plus_buys"
    "max_plus_buys"
    "min_victory_points"
    "max_victory_points"
    "min_trashes"
    "max_trashes"
    "is_attack"
    "is_reaction"
    "min_treasure"
    "max_treasure"
    "min_victory_points"
    "max_victory_points"
```


Technical Overview
=================
I created a webapp using [Flask](http://flask.pocoo.org/) on the backend and javascript on the frontend. The backend contains the database with cards in it, and an API that can be accessed by the frontend in order to get and modify cards. The backend contains randomization logic, and this should be built-in to the API eventually, but the frontend contains all the needed logic to act as a stand-alone app. It saves all the card-data in `localStorage.cards` and will function as an offline-app when appcaching is enabled.

Installation instructions
=========================
If you're interested in modifying this webapp, you need to download the code, set up a database, and install the dependencies.

### Requirements
*   python 2.7
*   [virtualenv](http://pypi.python.org/pypi/virtualenv)
*   [pip](http://www.pip-installer.org/en/latest/installing.html)
*   [postgresql](http://www.postgresql.org/download/) (or another SQL database that's compatible with SQLAlchemy)

### Recommended
*   [virtualenv-wrapper](http://www.doughellmann.com/projects/virtualenvwrapper/)

Use the installation method of your choice to get all the requirements on your computer. Mac users: I recommend using [homebrew](http://mxcl.github.com/homebrew/) to install what you can. Linux users: I recommend using you're favorite package manager (apt-get, yum, pacman, etc). Windows users: [chocolatey](http://chocolatey.org/) package manager may help.

Create a virtualenv for this project, and activate it. (I recommend installing virtualenv-wrapper to make this easier).

```bash
# get the code
git clone git://github.com/cypressf/dominion.git
# enter the directory you just cloned
cd dominion
# install the python packages required by the server
pip install -r requirements.txt
```

Now open up `app.py` and edit the line that says `app.config['SQLALCHEMY_DATABASE_URI'] = os.environ['HEROKU_POSTGRESQL_BLUE_URL']`. You'll need to change it to something like `app.config['SQLALCHEMY_DATABASE_URI'] = yourdatabaseurlgoeshere`.

Edit the line `_ADMIN_NAME = "cypressf"` to read `_ADMIN_NAME = "your_prefered_admin_username"`

Now open up a python shell. We're going to create your database and add the admin account for the website.
```python
from app import db, User
db.create_all()
admin = User("your_prefered_admin_username", "password")
db.session.add(admin)
db.session.commit()
```

Now run the server:
```bash
python app.py
````
And visit [http://0.0.0.0:5000/](http://0.0.0.0:5000/). You should see a website that looks like [dominion.olinapps.com](http://dominion.olinapps.com).

If you have trouble along the way, don't panic. Ask me for help! I will be able to fix your problems.