```mermaid
classDiagram
class Pessoa {
  - String nome
  - String cpf
  + getNome()
  + getCpf()
}

class Estudante {
  - int matricula
  - List~Disciplina~ disciplinas
  + getMatricula()
  + matricular(Disciplina d)
}

class Professor {
  - String area
  + getArea()
}

class Disciplina {
  - String codigo
  - String nome
  - Professor professor
  - List~Estudante~ estudantes
  + adicionarEstudante(Estudante e)
  + listarEstudantes()
}

Pessoa <|-- Estudante
Pessoa <|-- Professor
Disciplina o--> Professor
Disciplina o--> Estudante
