# Flask Full Stack Part 3: Flask and SQL Database Walkthrough

(course by Jose Portilla on Udemy)

Python and Flask can connect to a variety of SQL database engines, including PostgreSQL, MySQL, SQLite, and a lot more.

SQLite is a simple SQL database engine that comes with Flask, usually it can handle all the needs for a small application with daily hits around 100K. It can also handle more, it's not a hard cap.

To connect Python code to SQL code, use ORM (Object Relational Mapper). The most common is `SQLAlchemy`. `Flask-SQLAlchemy` is an extension to connect Flask to `SQLAlchemy`.

```bash
pip install Flask-SQLAlchemy
```

### Getting Started

We need the following steps

- Set up SQLite database in a Flask app
- Create a model in the Flask app
- Perform basic **CRUD** on our model

Here we will show the script for manual CRUD to help understand the concepts. In practice, Flask automates all these.

Use CLI tool to make database. Refer to `BasicModelApp.py` to check the model definition, and `SetUpDatabase.py` to see db creation script (usually db creation is through CLI).

A summary of model class definition:

- define `__tablename__`
- define column variables as class variables such as `id`, `name`, `age`, etc.
- define `__init__(self, ...)` method. *Note that `id` is automatically set and it does not need to be in `__init__()`*
- define `__repr__(self)` method
- (optional) if there is a relationship to another model/table, use `db.relationship('<another_model>', backref='<this_model>', lazy='dynamic')`

To add a row to the db,

```python
db.session.add(<row_obj>)
db.session.commit()
```

For SQLite and SQLAlchemy, integer `id` gets added automatically and starts at 1. The `.sqlite` file is the database, it's encoded and is not readable by a text editor.

CRUD example:

```python
from BasicModelApp import db,Puppy

###########################
###### CREATE ############
#########################
my_puppy = Puppy('Rufus',5)
db.session.add(my_puppy)
db.session.commit()

###########################
###### READ ##############
#########################
# Note lots of ORM filter options here.
# filter(), filter_by(), limit(), order_by(), group_by()
# Also lots of executor options
# all(), first(), get(), count(), paginate()

all_puppies = Puppy.query.all() # list of all puppies in table
print(all_puppies)
print('\n')
# Grab by id
puppy_one = Puppy.query.get(1)
print(puppy_one)
print(puppy_one.age)
print('\n')
# Filters
puppy_sam = Puppy.query.filter_by(name='Sammy') # Returns list
print(puppy_sam)
print('\n')
###########################
###### UPDATE ############
#########################

# Grab your data, then modify it, then save the changes.
first_puppy = Puppy.query.get(1)
first_puppy.age = 10
db.session.add(first_puppy)
db.session.commit()


###########################
###### DELETE ############
#########################
second_pup = Puppy.query.get(2)
db.session.delete(second_pup)
db.session.commit()


# Check for changes:
all_puppies = Puppy.query.all() # list of all puppies in table
print(all_puppies)
```

### Flask Migrate: Database Migration

When there are new columns to be added, only updating the model.py file won't update the database. We need database migration.

To install,

```bash
pip install Flask-Migrate
```

To import,

```python
from flask_migrate import Migrate
```

To use Flask Migrate, the first thing is to set the `FLASK_APP` variable. On Linux/Mac it goes,

```bash
export FLASK_APP=<myapp.py>
```

then import `Migrate` from `flask_migrate`, and add the line below before the class definition code.

```python
# Add on migration capabilities in order to run terminal commands
Migrate(app,db)
```

Migration steps:

```bash
# Set up the migration directory
flask db init
# Add some changes to the model class, e.g. add a column
# Set up the migration file. Similar to git commit
flask db migrate -m "<some message>"
# Update the database with the migration
flask db upgrade
```

### Flask Relationships

For multiple models (tables), we need "relationships" to link them together. Some concepts:

- Primary Key: unique identifier for a row
- Foreigh Key: the column that's another table's primary key

The key is to define `db.relationship(...)` to point a class variable `x` in `<parent_table>` (column) to another table. `x` is defined as `db.Column(db.<var_type>, db.ForeignKey('<parent_table.column>'))` in the other table. `<parent_table>` does not reference to other tables, other tables reference to it by having a column of `<parent_table>`'s primary key as `db.ForeignKey`.

Code example below

```python
class Puppy(db.Model):

    __tablename__ = 'puppies'

    id = db.Column(db.Integer,primary_key = True)
    name = db.Column(db.Text)
    # This is a one-to-many relationship
    # A puppy can have many toys
    toys = db.relationship('Toy',backref='puppy',lazy='dynamic')
    # This is a one-to-one relationship
    # A puppy only has one owner, thus uselist is False.
    # Strong assumption of 1 dog per 1 owner and vice versa.
    owner = db.relationship('Owner',backref='puppy',uselist=False)

    def __init__(self,name):
        # Note how a puppy only needs to be initalized with a name!
        self.name = name


    def __repr__(self):
        if self.owner:
            return f"Puppy name is {self.name} and owner is {self.owner.name}"
        else:
            return f"Puppy name is {self.name} and has no owner assigned yet."

    def report_toys(self):
        print("Here are my toys!")
        for toy in self.toys:
            print(toy.item_name)


class Toy(db.Model):

    __tablename__ = 'toys'

    id = db.Column(db.Integer,primary_key = True)
    item_name = db.Column(db.Text)
    # Connect the toy to the puppy that owns it.
    # We use puppies.id because __tablename__='puppies'
    puppy_id = db.Column(db.Integer,db.ForeignKey('puppies.id'))

    def __init__(self,item_name,puppy_id):
        self.item_name = item_name
        self.puppy_id = puppy_id


class Owner(db.Model):

    __tablename__ = 'owners'

    id = db.Column(db.Integer,primary_key= True)
    name = db.Column(db.Text)
    # We use puppies.id because __tablename__='puppies'
    puppy_id = db.Column(db.Integer,db.ForeignKey('puppies.id'))

    def __init__(self,name,puppy_id):
        self.name = name
        self.puppy_id = puppy_id
```

Note that one-to-one relationship has `uselist=False`. Default is `uselist=True` i.e. one-to-many, if not specified.

`lazy='dynamic'` is a more advanced usage. Leave it as it for now.

----
Schema:

Table `puppies`: `id`, `name` (parent table)

Table `toys`: `id`, `item_name`, `puppy_id` (foreign key, refers to `puppies.id`)

Table `owners`: `id`, `name`, `puppy_id` (foreign key, refers to `puppies.id`)

----

Now we experiment with adding some records with relationships.

```python
from models import db,Puppy,Owner,Toy

# Create 2 puppies
rufus = Puppy("Rufus")
fido = Puppy("Fido")

# Add puppies to database
db.session.add_all([rufus,fido])
db.session.commit()

# Check with a query, this prints out all the puppies!
print(Puppy.query.all())

# Grab Rufus from database
# Grab all puppies with the name "Rufus", returns a list, so index [0]
# Alternative is to use .first() instead of .all()[0]
rufus = Puppy.query.filter_by(name='Rufus').all()[0]

# Create an owner to Rufus
# Owner __init__ takes in name and foreign key Puppy id which is unique
jose = Owner("Jose",rufus.id)

# Give some Toys to Rufus
# Toy __init__ takes in item name and foreign key Puppy id which is unique
toy1 = Toy('Chew Toy',rufus.id)
toy2 = Toy("Ball",rufus.id)

# Commit these changes to the database
# Note that all_all can take different types of objects at once!!
db.session.add_all([jose,toy1,toy2])
db.session.commit()

# Let's now grab rufus again after these additions
rufus = Puppy.query.filter_by(name='Rufus').first()
print(rufus)
# Output:
# Puppy name is Rufus and owner is Jose

# Show toys
print(rufus.report_toys())
# Output:
# Chew Toy
# Ball

# You can also delete things from the database:
find_pup = Puppy.query.get(1)
db.session.delete(find_pup)
db.session.commit()
# But note that if deleted, do not try to delete again, or it will error
```

### Connect to Flask Template - Databases in Views

Refer to "03-Databases-in-Views" folder for a complete puppy adoption site example from scratch.

A **view function** is just a function with `app.route` in Flask, it responds to client requests.

First, create the `site.py` or `app.py`, then the necessary files such as `forms.py` and `models.py`. Next, create the `templates` folder and `base.html` in it, add necessary html files such as `home.html`, `add.html`, `delete.html`, etc.

Then add the model classes for the database same as in the last section.

The next step is to add `@app.routes` functions to the form view, such as

```python
@app.route('/add', methods=['GET', 'POST'])
def add_pup():
    form = AddForm()
    if form.validate_on_submit():
        name = form.name.data
        new_pup = Puppy(name)
        db.session.add(new_pup)
        db.session.commit()

        return redirect(url_for('list_pup'))

    return render_template('add.html', form=form)
```

and other views such as `/list` and `/delete`. Refer to example in the code folder.









