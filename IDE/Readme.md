from flask import Flask, render_template, request, redirect
from flask_mysqldb import MySQL
import yaml

app = Flask(__name__)

db = yaml.load(open('db.yaml'))
app.config['MYSQL_HOST'] = db['mysql_host']
app.config['MYSQL_USER'] = db['mysql_user']
app.config['MYSQL_PASSWORD'] = db['mysql_password']
app.config['MYSQL_DB'] = db['mysql_db']

mysql = MySQL(app)


@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        clienteInfo = request.form
        nome = clienteInfo['nome']
        endereco = clienteInfo['endereco']
        telefone = clienteInfo['telefone']
        email = clienteInfo['email']
        cur = mysql.connection.cursor()
        cur.execute("INSERT INTO user(NOME, ENDERECO, TELEFONE, EMAIL) VALUES(%s, %s, %s, %s)", (NOME, ENDERECO, TELEFONE, EMAIL))
        mysql.connection.commit()
        cur.close()
        return redirect('/')
    return render_template('index.html')

if __name__ == "__main__":
    app.run(debug=True)
