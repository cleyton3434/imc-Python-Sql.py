# imc-Python-Sql.py
Atividade PYTHON UNIS * IMC + SQL LITE 


import sqlite3

# Conectar ao banco de dados SQLite
conn = sqlite3.connect('imc_database.db')

# Criar uma tabela / armazenar os dados
conn.execute('''
    CREATE TABLE IF NOT EXISTS imc_data (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        altura REAL NOT NULL,
        peso REAL NOT NULL,
        imc REAL NOT NULL
    )
''')


def salvar_dados(nome, altura, peso):
    # Calcular IMC
    imc = peso / (altura ** 2)

    # Inserir dados na tabela
    conn.execute('''
        INSERT INTO imc_data (nome, altura, peso, imc)
        VALUES (?, ?, ?, ?)
    ''', (nome, altura, peso, imc))

    # Commit para salvar as alterações no banco de dados
    conn.commit()


def listar_dados():
    # Consultar todos os dados da tabela
    cursor = conn.execute('SELECT * FROM imc_data')

    for row in cursor.fetchall():
        print(row)


def main():
    print("Bem-vindo à calculadora de IMC!")

    nome = input("Digite seu nome: ")
    altura = float(input("Digite sua altura em metros: "))
    peso = float(input("Digite seu peso em quilogramas: "))

    salvar_dados(nome, altura, peso)
    print("Dados salvos com sucesso!")

    print("\nLista de dados:")
    listar_dados()

    # Fechar a conexão com o banco de dados
    conn.close()


if __name__ == "__main__":
    main()
