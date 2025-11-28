
# AULA COMPLETA – INTRODUÇÃO AO JAVA SWING, MONTAGEM DA TELA, EVENTOS, MENU E SEGUNDA TELA

## Objetivos da Aula

Este material apresenta de forma detalhada:

- O que é Java Swing e como funciona.
- Como montar uma interface gráfica organizada.
- Como posicionar corretamente componentes na tela.
- Como cadastrar alunos e exibi‑los em uma lista.
- Como criar menus e abrir uma segunda tela.
- Exercícios finais para prática.

---

# 1. Introdução ao Java Swing

Java Swing é a biblioteca gráfica padrão do Java para construção de interfaces desktop.  
Com ela, é possível criar janelas, botões, campos de texto, menus, listas, tabelas e diálogos.

Alguns componentes importantes:

- `JFrame` – representa uma janela.
- `JLabel` – exibe textos estáticos.
- `JTextField` – campo de entrada de texto.
- `JButton` – botão clicável.
- `JList` – lista de elementos.
- `DefaultListModel` – estrutura de dados da lista.
- `JScrollPane` – adiciona barras de rolagem.
- `JMenuBar`, `JMenu`, `JMenuItem` – para criação de menus.
- `JOptionPane` – diálogos de alerta e mensagens.

Swing utiliza um sistema chamado "Gerenciadores de Layout" para organizar componentes na tela.

---

# 2. Layouts utilizados na aula

Nesta aula serão usados dois layouts principais.

## 2.1 BorderLayout

Divide a tela em cinco regiões: North, South, East, West e Center.

Representação:

```
 -------------------------------------
|                NORTH                |
 -------------------------------------
|   WEST   |         CENTER          | EAST
 -------------------------------------
|                SOUTH                |
 -------------------------------------
```

Características:

- O componente em CENTER expande automaticamente.
- Ideal para organizar uma janela principal.

## 2.2 GridLayout

Organiza componentes em formato de tabela (linhas x colunas).  
Usamos um `GridLayout(4, 2, 5, 5)`, que significa:

- 4 linhas
- 2 colunas
- 5px de espaçamento horizontal e vertical

Representação do nosso formulário:

```
| Label Nome       | Campo Nome       |
| Label Matrícula  | Campo Matrícula  |
| Label Curso      | Campo Curso      |
| (vazio)          | Botão Cadastrar  |
```

---

# 3. Estrutura do Sistema

O sistema será organizado em três classes:

| Classe      | Função                                   |
|-------------|-------------------------------------------|
| `Aluno`     | Representa os dados de um aluno           |
| `TelaAluno` | Interface gráfica principal               |
| `Main`      | Classe inicial que abre a tela principal |

---

# 4. Código Completo e Explicado

## 4.1 Classe Aluno

```java
public class Aluno {

    private String nome;
    private String matricula;
    private String curso;

    public Aluno(String nome, String matricula, String curso) {
        this.nome = nome;
        this.matricula = matricula;
        this.curso = curso;
    }

    public String getNome() {
        return nome;
    }

    public String getMatricula() {
        return matricula;
    }

    public String getCurso() {
        return curso;
    }

    @Override
    public String toString() {
        return matricula + " - " + nome + " (" + curso + ")";
    }
}
```

Explicação:

- A classe contém atributos.
- O método `toString` define como o aluno será exibido na `JList`.

---

## 4.2 Classe TelaAluno – Explicação Detalhada

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TelaAluno extends JFrame {

    private JTextField txtNome;
    private JTextField txtMatricula;
    private JTextField txtCurso;
    private JButton btnCadastrar;

    private DefaultListModel<Aluno> modeloListaAlunos;
    private JList<Aluno> listaAlunos;

    public TelaAluno() {
        setTitle("Cadastro de Alunos");
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        inicializarComponentes();
    }


    private void inicializarComponentes() {
        setLayout(new BorderLayout());

        JPanel painelFormulario = new JPanel();
        painelFormulario.setLayout(new GridLayout(4, 2, 5, 5));

        JLabel lblNome = new JLabel("Nome:");
        txtNome = new JTextField();

        JLabel lblMatricula = new JLabel("Matrícula:");
        txtMatricula = new JTextField();

        JLabel lblCurso = new JLabel("Curso:");
        txtCurso = new JTextField();

        btnCadastrar = new JButton("Cadastrar");

        painelFormulario.add(lblNome);
        painelFormulario.add(txtNome);

        painelFormulario.add(lblMatricula);
        painelFormulario.add(txtMatricula);

        painelFormulario.add(lblCurso);
        painelFormulario.add(txtCurso);

        painelFormulario.add(new JLabel());
        painelFormulario.add(btnCadastrar);

        modeloListaAlunos = new DefaultListModel<>();
        listaAlunos = new JList<>(modeloListaAlunos);
        JScrollPane scrollLista = new JScrollPane(listaAlunos);

        add(painelFormulario, BorderLayout.NORTH);
        add(scrollLista, BorderLayout.CENTER);

        configurarEventos();
    }

    private void configurarEventos() {
        btnCadastrar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                cadastrarAluno();
            }
        });
    }

    private void cadastrarAluno() {
        String nome = txtNome.getText().trim();
        String matricula = txtMatricula.getText().trim();
        String curso = txtCurso.getText().trim();

        if (nome.isEmpty() || matricula.isEmpty() || curso.isEmpty()) {
            JOptionPane.showMessageDialog(
                    this,
                    "Preencha todos os campos!",
                    "Erro",
                    JOptionPane.ERROR_MESSAGE
            );
            return;
        }

        Aluno aluno = new Aluno(nome, matricula, curso);
        modeloListaAlunos.addElement(aluno);

        txtNome.setText("");
        txtMatricula.setText("");
        txtCurso.setText("");
        txtNome.requestFocus();
    }
}
```

### Explicando o uso de BorderLayout

- A janela usa `BorderLayout`.
- O formulário ficará em `NORTH`.
- A lista ficará no `CENTER`.

---

### Montagem do formulário com GridLayout

```java
        JPanel painelFormulario = new JPanel();
        painelFormulario.setLayout(new GridLayout(4, 2, 5, 5));
```

Explicação:

- `GridLayout` organiza os campos e rótulos em formato de tabela.
- Usamos 4 linhas e 2 colunas.

---

### Criação dos componentes

```java
        JLabel lblNome = new JLabel("Nome:");
        txtNome = new JTextField();

        JLabel lblMatricula = new JLabel("Matrícula:");
        txtMatricula = new JTextField();

        JLabel lblCurso = new JLabel("Curso:");
        txtCurso = new JTextField();

        btnCadastrar = new JButton("Cadastrar");
```

Explicação:

- Campos de texto armazenam os dados digitados.
- O botão enviará os dados para cadastro.

---

### Adicionando os itens ao formulário

```java
        painelFormulario.add(lblNome);
        painelFormulario.add(txtNome);

        painelFormulario.add(lblMatricula);
        painelFormulario.add(txtMatricula);

        painelFormulario.add(lblCurso);
        painelFormulario.add(txtCurso);

        painelFormulario.add(new JLabel());
        painelFormulario.add(btnCadastrar);
```

Explicação:

- O `JLabel()` vazio serve apenas para ocupar espaço e alinhar o botão.

---

### Criando a lista de alunos

```java
        modeloListaAlunos = new DefaultListModel<>();
        listaAlunos = new JList<>(modeloListaAlunos);
        JScrollPane scrollLista = new JScrollPane(listaAlunos);
```

Explicação:

- `DefaultListModel` armazena os dados da lista.
- `JList` exibe os dados.
- `JScrollPane` adiciona rolagem.

---

### Adicionando à janela

```java
        add(painelFormulario, BorderLayout.NORTH);
        add(scrollLista, BorderLayout.CENTER);
        configurarEventos();
    }
```

---

## 4.3 Configuração dos eventos

```java
    private void configurarEventos() {
        btnCadastrar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                cadastrarAluno();
            }
        });
    }
```

Explicação:

- `ActionListener` permite reagir a um clique.
- Quando o usuário clica no botão, o método `cadastrarAluno` é executado.

---

## 4.4 Método cadastrarAluno

```java
    private void cadastrarAluno() {
        String nome = txtNome.getText().trim();
        String matricula = txtMatricula.getText().trim();
        String curso = txtCurso.getText().trim();

        if (nome.isEmpty() || matricula.isEmpty() || curso.isEmpty()) {
            JOptionPane.showMessageDialog(
                    this,
                    "Preencha todos os campos!",
                    "Erro",
                    JOptionPane.ERROR_MESSAGE
            );
            return;
        }

        Aluno aluno = new Aluno(nome, matricula, curso);
        modeloListaAlunos.addElement(aluno);

        txtNome.setText("");
        txtMatricula.setText("");
        txtCurso.setText("");
        txtNome.requestFocus();
    }
}
```

Explicação:

- Valida os dados.
- Cria o objeto `Aluno`.
- Adiciona o aluno ao modelo da lista.
- Limpa os campos.
- Foca novamente no campo Nome.

---

# 5. Classe Main

```java
import javax.swing.SwingUtilities;

public class Main {

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                TelaAluno tela = new TelaAluno();
                tela.setVisible(true);
            }
        });
    }
}
```

Explicação:

- `invokeLater` garante que a interface seja criada na thread de GUI.
- Exibe a janela.

---

# 6. Como compilar e executar

No terminal:

```
javac Aluno.java TelaAluno.java Main.java
java Main
```

---

# 7. Exercício Final – Adicionando Menu e Segunda Tela

Você fará:

1. Criar `TelaSobre`.
2. Criar um menu na `TelaAluno`.
3. Abrir a segunda tela ao clicar no menu.

---

## 7.1 Classe TelaSobre

```java
import javax.swing.*;
import java.awt.*;

public class TelaSobre extends JFrame {

    public TelaSobre() {
        setTitle("Sobre");
        setSize(300, 200);
        setLocationRelativeTo(null);

        JLabel texto = new JLabel(
            "<html><center>Sistema de Cadastro de Alunos<br>Seu Nome<br>Sua Turma</center></html>",
            SwingConstants.CENTER
        );

        add(texto, BorderLayout.CENTER);
    }
}
```

---

## 7.2 Criando o menu na TelaAluno

Dentro de `inicializarComponentes()`:

```java
JMenuBar barraMenu = new JMenuBar();
JMenu menuAjuda = new JMenu("Ajuda");
JMenuItem itemSobre = new JMenuItem("Sobre");

menuAjuda.add(itemSobre);
barraMenu.add(menuAjuda);

setJMenuBar(barraMenu);
```

---

## 7.3 Abrindo a segunda tela

No método `configurarEventos()`:

```java
itemSobre.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        TelaSobre ts = new TelaSobre();
        ts.setVisible(true);
    }
});
```

---

# 8. Conclusão

Você aprendeu:

- Como funciona o Swing.
- Como organizar a tela com BorderLayout e GridLayout.
- Como criar eventos.
- Como manipular uma lista.
- Como criar menus.
- Como abrir novas janelas.

---


