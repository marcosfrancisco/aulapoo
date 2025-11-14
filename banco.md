# 🎓 AULA COMPLETA -- Java + MySQL no VS Code

## 🏁 INTRODUÇÃO

Hoje vamos aprender a criar um pequeno sistema Java que salva dados
dentro de um banco MySQL usando o VS Code.

Ao final desta aula, você será capaz de:

-   Conectar o Java ao MySQL usando JDBC\
-   Entender como funciona a conexão\
-   Criar classes Java para representar dados\
-   Enviar comandos SQL\
-   Ler dados do banco e transformar em objetos\
-   Rodar tudo no VS Code sem Maven

Vamos criar as seguintes classes:

    Aluno.java
    Conexao.java
    AlunoRepository.java
    Main.java

Estrutura recomendada:

    meu-projeto-java/
       ├── Aluno.java
       ├── Conexao.java
       ├── AlunoRepository.java
       └── Main.java

------------------------------------------------------------------------

## 🔌 COMO FUNCIONA A CONEXÃO JAVA → MYSQL?

O Java não conversa diretamente com MySQL. Eles falam linguagens
diferentes.

Por isso usamos o **Driver JDBC**, que funciona como um "tradutor".

### 🧠 Fluxo da comunicação

    Seu Código Java
          ↓
    JDBC (API padrão do Java)
          ↓
    Driver JDBC do MySQL
          ↓
    MySQL

### 🧩 O que faz cada parte?

-   **JDBC**: API padrão para conectar em bancos.\
-   **Driver JDBC** (mysql-connector): traduz JDBC → MySQL.\
-   **Class.forName(...)**: carrega o driver na memória.\
-   **Connection**: túnel aberto com o banco.\
-   **Statement**: envia comandos SQL.\
-   **ResultSet**: recebe resultados de SELECT.

------------------------------------------------------------------------

## 🗄️ 1. PREPARANDO O BANCO

Execute no MySQL:

``` sql
CREATE DATABASE escola;
USE escola;

CREATE TABLE aluno (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100)
);
```

------------------------------------------------------------------------

## 💻 2. ORGANIZAÇÃO DO PROJETO NO VS CODE

Não é preciso criar um "projeto oficial".\
Basta criar uma pasta com os arquivos `.java`.

Adicionar o driver:

1.  Baixar o MySQL Connector/J\
2.  No VS Code → JAVA PROJECTS → Referenced Libraries → Add Jar\
3.  Selecione `mysql-connector-j-8.0.xx.jar`

------------------------------------------------------------------------

## 🔌 3. CLASSE **Conexao.java**

``` java
import java.sql.Connection;
import java.sql.DriverManager;

public class Conexao {

    public static Connection getConnection() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            String url = "jdbc:mysql://localhost:3306/escola";
            String usuario = "root";
            String senha = "123456";

            return DriverManager.getConnection(url, usuario, senha);

        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

------------------------------------------------------------------------

## 👤 4. CLASSE **Aluno.java**

``` java
public class Aluno {
    private int id;
    private String nome;
    private String email;

    public Aluno(String nome, String email) {
        this.nome = nome;
        this.email = email;
    }

    public Aluno(int id, String nome, String email) {
        this.id = id;
        this.nome = nome;
        this.email = email;
    }

    public int getId() { return id; }
    public String getNome() { return nome; }
    public String getEmail() { return email; }

    @Override
    public String toString() {
        return id + " - " + nome + " (" + email + ")";
    }
}
```

------------------------------------------------------------------------

## 💾 5. CLASSE **AlunoRepository.java**

``` java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class AlunoRepository {

    public void salvar(Aluno aluno) {
        try {
            Connection conn = Conexao.getConnection();
            Statement stmt = conn.createStatement();

            String sql = "INSERT INTO aluno (nome, email) VALUES ('"
                    + aluno.getNome() + "', '" + aluno.getEmail() + "')";

            stmt.executeUpdate(sql);

            stmt.close();
            conn.close();

            System.out.println("Aluno salvo com sucesso!");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public List<Aluno> listarTodos() {
        List<Aluno> lista = new ArrayList<>();

        try {
            Connection conn = Conexao.getConnection();
            Statement stmt = conn.createStatement();

            String sql = "SELECT id, nome, email FROM aluno";
            ResultSet rs = stmt.executeQuery(sql);

            while (rs.next()) {
                int id = rs.getInt("id");
                String nome = rs.getString("nome");
                String email = rs.getString("email");

                lista.add(new Aluno(id, nome, email));
            }

            rs.close();
            stmt.close();
            conn.close();

        } catch (Exception e) {
            e.printStackTrace();
        }

        return lista;
    }
}
```

------------------------------------------------------------------------

## ▶️ 6. CLASSE **Main.java**

``` java
import java.util.List;

public class Main {
    public static void main(String[] args) {

        AlunoRepository repo = new AlunoRepository();

        Aluno a = new Aluno("Ana Silva", "ana@gmail.com");
        repo.salvar(a);

        List<Aluno> alunos = repo.listarTodos();

        System.out.println("\n=== Lista de alunos ===");
        for (Aluno x : alunos) {
            System.out.println(x);
        }
    }
}
```

------------------------------------------------------------------------

## 🧪 7. TESTANDO NO MYSQL

No MySQL, execute:

``` sql
SELECT * FROM aluno;
```

------------------------------------------------------------------------

## 🎯 8. EXERCÍCIOS

1.  Adicione o campo **idade** na tabela e no código.\
2.  Crie o método **deletarPorId(int id)**.\
3.  Crie o método **buscarPorId(int id)**.\
4.  Crie uma classe **Curso** e repita tudo.

------------------------------------------------------------------------

## 🏆 FINALIZAÇÃO

Parabéns!\
Você aprendeu:

-   Como o Java se conecta ao MySQL\
-   Como usar o driver JDBC\
-   Como criar classes Java com persistência\
-   Como enviar SQL pelo Java\
-   Como recuperar dados do banco

Essa é a base de qualquer aplicação real Java + banco de dados.
