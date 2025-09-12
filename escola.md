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
  - Curso curso
  - List~Disciplina~ disciplinas
  + getMatricula()
  + getCurso()
  + getDisciplinas() List~Disciplina~
  + matricular(Disciplina d)
}

class Professor {
  - Departamento departamento
  + getDepartamento()
}

class Disciplina {
  - String codigo
  - String nome
  - TipoDisciplina tipo
  - Professor professor
  - List~Estudante~ estudantes
  + adicionarEstudante(Estudante e)
  + getEstudantes() List~Estudante~
}

class Curso {
  <<enum>>
  ADS
  ENG_ELETRICA
  COMPUTACAO
}

class Departamento {
  <<enum>>
  COMPUTACAO
  MATEMATICA
  ELETRICA
}

class TipoDisciplina {
  <<enum>>
  OBRIGATORIA
  OPTATIVA
}

Pessoa <|-- Estudante
Pessoa <|-- Professor
Disciplina o--> Professor
Disciplina o--> Estudante
Estudante --> Curso
Professor --> Departamento
Disciplina --> TipoDisciplina
