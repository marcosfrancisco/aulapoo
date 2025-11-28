# AULA – INTRODUÇÃO AO JAVA SWING + CADASTRO DE ALUNOS + MENU E SEGUNDA TELA

## Objetivos da Aula

Ao final desta atividade, você deverá ser capaz de:

- Entender o que é Java Swing e para que ele serve.
- Criar uma janela (`JFrame`) com campos, botão e lista.
- Cadastrar alunos em memória (sem banco de dados).
- Exibir os alunos cadastrados na interface.
- Criar um menu e abrir uma nova tela a partir dele.

## 1. O que é o Java Swing?

O Java Swing é a biblioteca que permite criar janelas e interfaces gráficas no Java.

Com Swing, você consegue criar:

- Janelas → `JFrame`
- Botões → `JButton`
- Campos de texto → `JTextField`
- Rótulos → `JLabel`
- Listas → `JList`
- Caixas de mensagens → `JOptionPane`

Ele já vem no Java. Não precisa instalar nada além do JDK.

## 2. Componentes importantes usados na aula

- `JFrame` → a janela principal.
- `JLabel` → textos (rótulos).
- `JTextField` → campo de digitação.
- `JButton` → botão de ação.
- `JList` → lista de elementos.
- `DefaultListModel` → onde os itens da lista ficam armazenados.
- Layouts:
  - `BorderLayout` → organiza a janela em regiões.
  - `GridLayout` → organiza componentes em grade.

## 3. Estrutura do sistema

| Classe       | Função                                      |
|--------------|----------------------------------------------|
| `Aluno`      | representa os dados de um aluno              |
| `TelaAluno`  | interface gráfica com campos, botão e lista  |
| `Main`       | ponto de entrada da aplicação                |

## 4. Passo a passo do código

Crie os arquivos:

```
Aluno.java
TelaAluno.java
Main.java
```

## 4.1 Classe `Aluno.java`

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

## 4.2 Classe `TelaAluno.java`

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

## 4.3 Classe `Main.java`

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

## 5. Como compilar e executar

No terminal:

```bash
javac Aluno.java TelaAluno.java Main.java
java Main
```

## 6. Exercício Final – Criar menu e abrir segunda tela

Você deve:

1. Criar um menu usando `JMenuBar`.
2. Criar um menu chamado "Ajuda".
3. Adicionar o item "Sobre".
4. Criar a classe `TelaSobre`.
5. Fazer o item "Sobre" abrir a nova tela.

### 6.1 Classe `TelaSobre.java`

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

### 6.2 Adicionando menu na `TelaAluno`

```java
JMenuBar barraMenu = new JMenuBar();
JMenu menuAjuda = new JMenu("Ajuda");
JMenuItem itemSobre = new JMenuItem("Sobre");

menuAjuda.add(itemSobre);
barraMenu.add(menuAjuda);

setJMenuBar(barraMenu);
```

### 6.3 Abrindo a segunda tela

```java
itemSobre.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        TelaSobre ts = new TelaSobre();
        ts.setVisible(true);
    }
});
```
