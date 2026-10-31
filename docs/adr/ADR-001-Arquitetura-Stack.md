# ADR-001 — Arquitetura e stack do sistema de grupos de estudo

- **Situação:** proposta
- **Data:** 2026-10-30
- **Participantes:** Danielle, Gabriel Lopes, Pedro Franco, Pedro Henrique e Yuri

## Contexto

O projeto é um sistema para encontrar grupos de estudo. A equipe é uma turma
de estudantes, e a escolha da stack leva em conta o conhecimento prévio da
turma em Java.

## Decisão

Adotar um **monólito modular com fatias verticais** no backend, em
**Java + Spring Boot**, com banco de dados **PostgreSQL**. O frontend será uma
**SPA em React + TypeScript**, que consome o backend por API.

Cada módulo do monólito representa uma fatia vertical do negócio e reúne seu
próprio código de API, regras de negócio e acesso a dados.

## Alternativas consideradas

| Alternativa | Por que não foi escolhida |
|---|---|
| Microsserviços (validar) | Complexidade operacional e de deploy desproporcional para o tamanho da equipe e do projeto. |
| Monólito em camadas tradicional (validar) | As camadas técnicas espalham cada funcionalidade por vários pacotes. As fatias verticais mantêm cada funcionalidade em um só lugar. |
| Frontend renderizado no servidor, como Thymeleaf (validar) | A equipe optou por uma SPA para separar frontend e backend e trabalhar em paralelo. |

## Consequências

- **Positivas:**
  - A equipe já domina Java, o que reduz a curva de aprendizado.
  - Um único artefato de backend simplifica o desenvolvimento e o deploy.
  - Os módulos isolados permitem dividir o trabalho entre os membros e, se
    necessário, extrair um módulo para serviço próprio no futuro.
  - Frontend e backend podem evoluir em paralelo por meio do contrato da API.
- **Negativas:**
  - É preciso disciplina para manter as fronteiras entre módulos, senão o
    monólito vira um "big ball of mud".
  - Duas bases de código e duas toolchains (Java/Maven e Node/TypeScript)
    para manter.
  - Todos os módulos escalam e fazem deploy juntos.
- **Como verificar:**
  - Teste do Spring Modulith (complementado por ArchUnit, se necessário) no
    build desde o primeiro commit, falhando se um módulo usar classes internas
    de outro, ler tabelas de outro módulo ou criar dependência circular.
  - Testes por módulo, com os módulos vizinhos simulados.
  - Frontend: testes de acessibilidade.
  - Pull Request revisado por outro integrante
