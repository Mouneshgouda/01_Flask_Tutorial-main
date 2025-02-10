-3
- ### app.py
``` python
from flask import Flask, render_template, request, redirect, url_for, send_from_directory
import os

app = Flask(__name__)
UPLOAD_FOLDER = 'uploads'
os.makedirs(UPLOAD_FOLDER, exist_ok=True)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

@app.route('/')
def index():
    return render_template('upload.html')

@app.route('/upload', methods=['POST'])
def upload_file():
    if 'file' not in request.files:
        return "No file part"
    file = request.files['file']
    if file.filename == '':
        return "No selected file"
    file.save(os.path.join(app.config['UPLOAD_FOLDER'], file.filename))
    return redirect(url_for('download_page', filename=file.filename))

@app.route('/download/<filename>')
def download_page(filename):
    return render_template('download.html', filename=filename)

@app.route('/download/file/<filename>')
def download_file(filename):
    return send_from_directory(app.config['UPLOAD_FOLDER'], filename, as_attachment=True)

if __name__ == '__main__':
    app.run(debug=True)

```
- ### download.html
```html
<link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">

<h1>Download File</h1>
<p>Click the link below to download your file:</p>
<a href="/download/{{ filename }}">{{ filename }}</a>

```
- ### upload.html
```html
<link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">

<h1>Upload a File</h1>
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" value="Upload">
</form>



```



- #### File

  ### 2 Login and Register pages
### login
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <h2>Login</h2>
    <form action="dashboard.html" method="POST">
        <label>Email:</label><br>
        <input type="email" name="email" required><br>
        <label>Password:</label><br>
        <input type="password" name="password" required><br><br>
        <button type="submit">Login</button>
    </form>
    <p>Don't have an account? <a href="register.html">Register</a></p>
</body>
</html>
```
### register
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Register</title>
</head>
<body>
    <h2>Register</h2>
    <form action="login.html" method="POST">
        <label>Email:</label><br>
        <input type="email" name="email" required><br>
        <label>Password:</label><br>
        <input type="password" name="password" required><br>
        <label>Confirm Password:</label><br>
        <input type="password" name="confirm-password" required><br><br>
        <button type="submit">Register</button>
    </form>
    <p>Already have an account? <a href="login.html">Login</a></p>
</body>
</html>
```

