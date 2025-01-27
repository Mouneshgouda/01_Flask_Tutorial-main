#### app.py
```
from flask import Flask, render_template
from flask_sqlalchemy import SQLAlchemy

# Initialize Flask app
app = Flask(__name__)

# Set up the database URI (SQLite)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///database.db'  # SQLite database
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # Disable modification tracking

# Initialize the database object
db = SQLAlchemy(app)

# Define the model (User table)
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    age = db.Column(db.Integer, nullable=False)
    city = db.Column(db.String(100), nullable=False)

    def __repr__(self):
        return f"<User {self.name}>"

# Route for home page
@app.route('/')
def index():
    # Query all users from the database
    users = User.query.all()
    return render_template('index.html', users=users)

# This function will be executed when the app starts
def add_sample_users():
    # Create sample users
    user1 = User(name='John Doe', age=28, city='New York')
    user2 = User(name='Jane Smith', age=34, city='San Francisco')
    user3 = User(name='Samuel Green', age=22, city='Chicago')
    user4 = User(name='Emily Brown', age=40, city='Miami')

    # Add users to the session and commit them to the database
    db.session.add_all([user1, user2, user3, user4])
    db.session.commit()

# Ensure the database is created and insert data if it's empty
with app.app_context():
    # Create the database and tables (if they don't exist)
    db.create_all()

    # Check if the database is empty (no users), then insert data
    if User.query.count() == 0:
        add_sample_users()

# Run the app
if __name__ == '__main__':
    app.run(debug=True)
```
### html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Users Table</title>
</head>
<body>
    <div class="container">
        <h1>Users Table</h1>
        <table border="1" cellspacing="0" cellpadding="5">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Age</th>
                    <th>City</th>
                </tr>
            </thead>
            <tbody>
                {% for user in users %}
                    <tr>
                        <td>{{ user.id }}</td>
                        <td>{{ user.name }}</td>
                        <td>{{ user.age }}</td>
                        <td>{{ user.city }}</td>
                    </tr>
                {% endfor %}
            </tbody>
        </table>
    </div>
</body>
</html>
```
