---
author: internat
datetime: 2023-04-14
title: Flask-sqlAlchemy
slug: flask-sqlalchemy
featured: false
draft: false
tags:
  - python
  - sqlite
ogImage: ""
description: flask sqlalchemy
---

# Flask sqlAlchemy

![image](https://flask-sqlalchemy.palletsprojects.com/en/2.x/_images/flask-sqlalchemy-title.png)

### Import the necessary modules

```py
from datetime import datetime
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
```

### Create a Flask application

```py
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///example.db'  # Specify the database URI
db = SQLAlchemy(app)  # Initialize SQLAlchemy
```

### Define the User model

```py
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(20), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    image_file = db.Column(db.String(20), nullable=False, default='default.jpg')
    password = db.Column(db.String(60), nullable=False)
    posts = db.relationship('Post', backref='author', lazy=True)

    def __repr__(self):
        return f'User {self.username} - {self.email}'
```

### Define the Post model

```py
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), unique=True, nullable=False)
    date_posted = db.Column(db.DateTime, nullable=False, default=datetime.utcnow)
    content = db.Column(db.Text, nullable=False)
    user_id = db.Column(db.Integer, db.ForeignKey('user.id'), nullable=False)

    def __repr__(self):
        return f'Post {self.title} - {self.date_posted}'
```

### Add a new user to the database in python console

```py
new_user = User(username='JohnDoe', email='johndoe@example.com', password='password')
db.session.add(new_user)
db.session.commit()
```

### Add a new post associated with the user to the database in python console

```py
new_post = Post(title='First Post', content='Hello World!', user_id=new_user.id)
db.session.add(new_post)
db.session.commit()
```

### Add user and post from route

```py
@app.route('/create-user')
def create_user():
    # Add a new user to the database
    new_user = User(username='JohnDoe', email='johndoe@example.com', password='password')
    db.session.add(new_user)
    db.session.commit()

    # Add a new post associated with the user to the database
    new_post = Post(title='First Post', content='Hello World!', user_id=new_user.id)
    db.session.add(new_post)
    db.session.commit()
    return redirect('/')
```

[see full code](https://github.com/sonukuldeep/flask-tutorial/tree/sqlalchemy)
