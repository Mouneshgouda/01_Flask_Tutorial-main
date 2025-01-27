```python
from flask import Flask, render_template, request, redirect, url_for
from flask_sqlalchemy import SQLAlchemy
import pymysql

# Fix to make MySQL work with SQLAlchemy
pymysql.install_as_MySQLdb()

app = Flask(__name__)

# MySQL Database Configuration
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mgpatils@localhost/mydb'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
db = SQLAlchemy(app)

# Define a User Model for SQLAlchemy
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(100), unique=True, nullable=False)
    email = db.Column(db.String(100), unique=True, nullable=False)

# Home route to display all users
@app.route('/')
def index():
    users = User.query.all()  # Query all users
    return render_template('index.html', users=users)

# Route to add a new user
@app.route('/add_user', methods=['POST'])
def add_user():
    if request.method == 'POST':
        username = request.form['username']
        email = request.form['email']
        new_user = User(username=username, email=email)
        db.session.add(new_user)
        db.session.commit()  # Commit changes to the database
        return redirect(url_for('index'))

if __name__ == '__main__':
    app.run(debug=True)
```
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask MySQL Example</title>
</head>
<body>
    <h1>Users</h1>

    <!-- Display all users -->
    <ul>
        {% for user in users %}
        <li>{{ user.username }} - {{ user.email }}</li>
        {% endfor %}
    </ul>

    <!-- Form to add a new user -->
    <h2>Add New User</h2>
    <form action="/add_user" method="POST">
        <label for="username">Username:</label>
        <input type="text" id="username" name="username" required><br><br>
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required><br><br>
        <input type="submit" value="Add User">
    </form>
</body>
</html>
```
