from flask import Flask, render_template, request, redirect , url_for, session
import peewee, os

app = Flask(__name__)
app.config ["SECRET_KEY"] = "bla"

db = peewee.SqliteDatabase('pessoas.db')

class BaseModel(peewee.Model):
    class Meta:
        database = db

class Pessoa(BaseModel):
    nome = peewee.CharField()
    cpf = peewee.CharField()
    def __str__(self):
        return self.nome+","+self.cpf 

class Material(BaseModel):
    nome= peewee.CharField()
    valor= peewee.CharField()
    quantidade= peewee.CharField()
    def __str__(self):
        return self.nome+","+self.valor+","self.quantidade

class Pedido(BaseModel):
    nome_mobilia= peewee.ForeignKeyField()
    valor= peewee.CharField()
    quantidade= peewee.CharField()
    cpf= peewee.ForeignKeyField()
    def __str__(self):
        return self.nome_mobilia+","+self.valor+","self.quantidade+","self.cpf

class Mobilia(BaseModel):
    nome_mobilia= peewee.CharField()
    valor= peewee.CharField()
    materiais= peewee.CharField()
    def __str__(self):
        return self.nome_mobilia+","+self.valor+","self.materiais

    
db.connect()
db.create_tables([Pessoa]) 

lista_de_pessoas=[]


@app.route("/")
def inicio():
    return render_template("inicio.html")

@app.route("/listar_pessoas")
def listar_pessoas():
    return render_template("listar_pessoas.html", geral = lista_de_pessoas)    

@app.route("/inserir_pessoa")
def inserir_pessoas():
    return render_template("form_inserir_pessoa.html")  

@app.route("/form_alterar_pessoa")
def for_alterar_pessoa():
    procurado = request.args.get("nome")
    for pe in lista_de_pessoas:
        if pe.nome == procurado:
            return render_template("form_alterar_pessoa.html", informacoes = pe)
    return "Não achei: " + procurado + " :("

@app.route("/alterar_pessoa", methods = ['POST'])
def alterar_pessoa():
    procurado = request.form["nome_original"]
    nome = request.form ["nome"]
    cpf = request.form ["cpf"]
    fenix = Pessoa (nome, cpf)
    for i in range(len(lista_de_pessoas)):
        if lista_de_pessoas[i].nome == procurado:
            lista_de_pessoas[i] = fenix
            return redirect (url_for("listar_pessoas"))
    return "Não achei: " + procurado + " :("

@app.route("/cadastrar_pessoa", methods = ['POST'])
def add():
    
    novapessoa = Pessoa.create(nome = request.form["nome"], cpf = request.form["cpf"])
    pessoas = Pessoa.select()
    return render_template ("listar_pessoas.html", geral = pessoas)
    #
    #return "ok"
    #return redirect ( url_for ("listar_pessoas"))

@app.route("/excluir_pessoa")
def excluir_pessoa():
    achou = None
    nome = request.args.get("nome")
    for p in lista_de_pessoas:
        if p.nome == nome:
            achou = p 
            break
    if achou != None:
        lista_de_pessoas.remove(achou) 
    return render_template("exibir_mensagem.html")


@app.route("/form_login")
def form_login():
    return render_template("form_login.html")  

@app.route("/login", methods = ['POST'])
def login():
    login = request.form["login"]
    senha = request.form["senha"]
    if login == "franke" and senha == "123":
        session ["usuario"] = login
        return redirect("/listar_pessoas")
    else:
        return "Login/ senha inválida"

@app.route("/logout")
def logout():
    session.pop("usuario")
    return redirect("/")

app.run()# debug = True)
