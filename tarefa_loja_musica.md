# 📝 Tarefa – Loja de Música em Java

## 🎯 Objetivo
Implementar um sistema simples de **loja de música** em Java, utilizando **herança, associação, composição e enum** com base no **diagrama de classes** fornecido.  

---

## 📊 Diagrama de Classes

```mermaid
classDiagram
    class Produto {
        <<abstract>>
        - int codigo
        - String nome
        - double preco
        - Categoria categoria
        + Produto(int codigo, String nome, double preco, Categoria categoria)
        + getCodigo() int
        + getNome() String
        + getPreco() double
        + getCategoria() Categoria
        + setPreco(double preco) void
        + exibirDetalhes() void
    }

    class Instrumento {
        - String tipo
        - String marca
        + Instrumento(int codigo, String nome, double preco, Categoria categoria, String tipo, String marca)
        + getTipo() String
        + getMarca() String
        + exibirDetalhes() void
    }

    class Acessorio {
        - String material
        + Acessorio(int codigo, String nome, double preco, Categoria categoria, String material)
        + getMaterial() String
        + exibirDetalhes() void
    }

    class Funcionario {
        - int id
        - String nome
        + Funcionario(int id, String nome)
        + cadastrarProduto(Produto p) void
        + consultarProduto(int codigo) Produto
        + gerarVenda() Venda
        + consultarVenda(int id) Venda
    }

    class Venda {
        - int id
        - Date data
        - List~ItemVenda~ itens
        - double valorTotal
        + Venda(int id, Date data)
        + adicionarProduto(Produto p, int qtd) void
        + calcularTotal() double
        + getValorTotal() double
    }

    class ItemVenda {
        - Produto produto
        - int quantidade
        - double subtotal
        + ItemVenda(Produto produto, int quantidade)
        + calcularSubtotal() double
        + getProduto() Produto
        + getQuantidade() int
    }

    enum Categoria {
        CORDAS
        PERCUSSAO
        SOPRO
        ELETRONICO
        ACESSORIOS
    }

    Produto <|-- Instrumento
    Produto <|-- Acessorio
    Venda "1" *-- "many" ItemVenda
    ItemVenda "1" --> "1" Produto
    Funcionario "1" --> "*" Venda
    Produto --> Categoria
```

---

## 🛠️ Instruções de Implementação

1. **Crie todas as classes e o enum** conforme o diagrama:  
   - `Produto` (classe **abstrata**)  
   - `Instrumento` (herda de Produto)  
   - `Acessorio` (herda de Produto)  
   - `ItemVenda`  
   - `Venda`  
   - `Funcionario`  
   - `Categoria` (**enum**)  

2. Cada classe deve ter:  
   - **Atributos privados**.  
   - **Construtor** conforme o diagrama.  
   - **Métodos getters e setters** quando necessário.  
   - O método `exibirDetalhes()` para mostrar informações no console.  

3. A classe `Produto` deve ser abstrata e incluir um atributo do tipo `Categoria`.  

4. A classe `Venda` deve:  
   - Permitir **adicionar produtos** (criando `ItemVenda`).  
   - Ter um método `calcularTotal()` que soma todos os subtotais.  

5. A classe `Funcionario` deve simular:  
   - **Cadastrar produtos** em memória (pode ser uma lista ou array).  
   - **Consultar produto** por código.  
   - **Gerar venda**.  
   - **Consultar venda** por id.  

6. Crie uma classe `Main` com o método `main(String[] args)` para testar o sistema:  
   - Cadastre alguns produtos (`Instrumento` e `Acessorio`) com categorias do `enum`.  
   - Crie uma venda adicionando produtos.  
   - Exiba o valor total da venda.  
   - Consulte e exiba os produtos e vendas cadastrados.  

---

## 📌 O que deve ser entregue

- Arquivos Java:  
  - `Produto.java`  
  - `Instrumento.java`  
  - `Acessorio.java`  
  - `ItemVenda.java`  
  - `Venda.java`  
  - `Funcionario.java`  
  - `Categoria.java` (**enum**)  
  - `Main.java`  

- O sistema deve **compilar e rodar** sem erros.  
- No console deve ser possível ver:  
  - Produtos cadastrados.  
  - Vendas realizadas.  
  - Valor total calculado da venda.  

---

## ✅ Critérios de Avaliação

- Implementação fiel ao diagrama.  
- Uso correto de **herança, enum e composição**.  
- Código organizado, com nomes claros.  
- Funcionamento completo do fluxo: **cadastrar → vender → consultar**.  

---

👉 **Dica do Prof. Marcos**: comece pelas classes mais simples (`Produto`, `Instrumento`, `Acessorio`, `Categoria`), depois faça `ItemVenda` e `Venda`, e só no final implemente o `Funcionario` e a `Main`.  
