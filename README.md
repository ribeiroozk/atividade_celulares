# 📱 Sistema de Assistência Técnica de Celulares

Sistema web para gerenciar **clientes, aparelhos e agendamentos de reparo** de uma assistência técnica de celulares, com autenticação de usuários e proteção de dados pessoais (LGPD).

Projeto desenvolvido como atividade avaliativa do curso técnico do SENAI, a partir do sistema base da Oficina Mecânica.

**Autor:** Arthur

---

## 🧰 Tecnologias

| Camada | Tecnologia |
|---|---|
| Back-end | Python 3.10+ e Flask 3.0.3 |
| Banco de dados | MySQL 8+ (`mysql-connector-python`) |
| Segurança | bcrypt (senha e CPF) e sessão com expiração |
| Front-end | HTML, CSS e JavaScript puro |

---

## ✅ Funcionalidades

- **Login** com validação de falha (mensagem de erro e campos vazios) e sessão que expira em 30 minutos.
- **Tela principal** com o nome do usuário logado, acesso aos módulos e botão **Sair** (logout).
- **Clientes:** cadastrar, buscar (nome, e-mail, telefone ou CPF), editar e excluir.
- **Aparelhos:** cadastrar (marca, modelo, IMEI, cor e defeito relatado) sempre associado a um cliente, editar e excluir.
- **Agendamentos:** listagem ordenada por data com os dados do cliente e do aparelho, cadastro e reagendamento.

### 🔒 Regras de negócio e segurança

- O **CPF é armazenado criptografado** (hash bcrypt), nunca em texto puro.
- As senhas dos usuários também são guardadas com hash bcrypt.
- Não é possível cadastrar CPF ou IMEI repetidos.
- Cliente com aparelhos e aparelho com agendamentos não podem ser excluídos.
- Consultas SQL parametrizadas (proteção contra SQL Injection).
- Páginas e API protegidas: sem sessão válida, o usuário volta para o login.

---

## 🗂️ Estrutura do projeto

```
entrega/
├── README.md
├── documentacao.pdf        # Anexo 1 (requisitos e infraestrutura)
├── der.png                 # Diagrama Entidade-Relacionamento
├── assistencia_db.sql      # Criação e população do banco
└── sistema/
    ├── app.py              # Ponto de entrada
    ├── db.py               # Conexão com o MySQL
    ├── autenticacao.py     # Hash bcrypt e sessão
    ├── rotas.py            # Rotas das páginas e da API
    ├── clientes.py         # Regras de clientes
    ├── aparelhos.py        # Regras de aparelhos
    ├── agendamento.py      # Regras de agendamentos
    ├── testar_conexao.py   # Teste da conexão com o banco
    ├── requirements.txt
    ├── static/             # app.js e style.css
    └── templates/          # login, principal, clientes, aparelhos, agendamentos
```

---

## 🗃️ Banco de dados

Banco `assistencia_db` com 5 tabelas, cada uma com 3 registros de exemplo:

`usuarios` · `clientes` · `aparelhos` · `servicos` · `agendamentos`

![DER](der.png)

Relacionamentos:

- 1 cliente possui N aparelhos
- 1 aparelho possui N agendamentos
- 1 serviço aparece em N agendamentos

---

## ▶️ Como executar

**Pré-requisitos:** Python 3.10+ e MySQL 8+ em execução.

### 1. Criar e popular o banco

```powershell
mysql -u root -p < assistencia_db.sql
```

### 2. Instalar as dependências

```powershell
cd sistema
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

No Linux/Mac, ative o ambiente com `source venv/bin/activate`.

### 3. Configurar a conexão

```powershell
$env:DB_USER="root"
$env:DB_PASSWORD="sua_senha"
$env:SECRET_KEY="uma-chave-longa"
```

Variáveis opcionais (valores padrão): `DB_HOST=localhost`, `DB_PORT=3306`, `DB_NAME=assistencia_db`.

### 4. Testar a conexão

```powershell
python testar_conexao.py
```

Esperado: `Conexão OK: {'banco': 'assistencia_db', 'versao': '8.x'}`

### 5. Iniciar o sistema

```powershell
python app.py
```

Acesse **http://localhost:5000**.

---

## 🔑 Usuários de teste

| Usuário | Senha | Nome |
|---|---|---|
| `admin` | `admin123` | Administrador |
| `carlos` | `carlos123` | Carlos Técnico |
| `ana` | `ana12345` | Ana Atendente |

CPFs de exemplo para testar a busca: `529.982.247-25`, `111.444.777-35` e `390.533.447-05`.

---

## 🧪 Roteiro de testes

O arquivo `sistema/ROTEIRO.md` traz o passo a passo completo de teste: login (falha, vazio e sucesso), tela principal, clientes (busca e edição), aparelhos, agendamentos e expiração de sessão.

---

## 📄 Entregas da atividade

| Nº | Entrega | Arquivo |
|---|---|---|
| 1 | Documentação de software | `documentacao.pdf` |
| 2 | Diagrama Entidade-Relacionamento | `der.png` |
| 3 | Script de criação e população do BD | `assistencia_db.sql` |
| 4 a 8 | Código-fonte (login, principal, clientes, aparelhos e agendamentos) | pasta `sistema` |
