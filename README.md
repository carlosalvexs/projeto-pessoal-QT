# 📚 StudyFlow

> Aplicativo de console (CLI) em Node.js para gerenciamento inteligente de estudos com gamificação.

---

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Elicitação de Requisitos](#-elicitação-de-requisitos)
  - [Requisitos Funcionais](#-requisitos-funcionais)
  - [Requisitos Não Funcionais](#-requisitos-não-funcionais)
  - [Regras de Negócio](#-regras-de-negócio)
- [Arquitetura do Projeto](#-arquitetura-do-projeto)
- [Funcionalidades](#-funcionalidades)
- [Cálculo de Prioridade](#-cálculo-de-prioridade)
- [Sistema de Gamificação](#-sistema-de-gamificação)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Estrutura de Pastas](#-estrutura-de-pastas)
- [Como Executar](#-como-executar)
- [Testes](#-testes)

---

## 🎯 Visão Geral

O **StudyFlow** é uma ferramenta CLI desenvolvida em Node.js puro, pensada para estudantes que precisam organizar disciplinas, gerenciar prazos de provas e acompanhar seu progresso por meio de um sistema de gamificação com XP e níveis.

---

## 📌 Elicitação de Requisitos

### ✅ Requisitos Funcionais

| ID | Requisito | Prioridade |
|----|-----------|------------|
| RF01 | O sistema deve permitir o cadastro de disciplinas com nome e nível de dificuldade (1–10) | Alta |
| RF02 | O sistema deve permitir o cadastro de avaliações vinculadas a uma disciplina, informando tipo, peso e dias restantes | Alta |
| RF03 | O sistema deve permitir o registro de horas estudadas por sessão | Alta |
| RF04 | O sistema deve listar todas as disciplinas cadastradas com seus dados | Média |
| RF05 | O sistema deve calcular e exibir as prioridades de estudo com base em peso, dificuldade, tipo e prazo | Alta |
| RF06 | O sistema deve exibir o XP atual e o nível do usuário | Média |
| RF07 | O sistema deve conceder XP ao usuário a cada hora estudada registrada | Alta |
| RF08 | O sistema deve calcular a evolução de nível com base no XP acumulado | Média |
| RF09 | O sistema deve oferecer um menu interativo no terminal com opções numeradas | Alta |
| RF10 | O sistema deve permitir encerrar a aplicação pelo menu | Baixa |

---

### 🔒 Requisitos Não Funcionais

| ID | Requisito | Categoria |
|----|-----------|-----------|
| RNF01 | O sistema deve ser executado em ambiente Node.js sem dependências externas de banco de dados | Portabilidade |
| RNF02 | O código deve ser organizado em camadas separadas (controller, service, database, view) | Manutenibilidade |
| RNF03 | O sistema deve ser testável com cobertura de testes unitários via Jest | Qualidade |
| RNF04 | A interface deve ser acessível via terminal (readline) sem necessidade de navegador | Usabilidade |
| RNF05 | O sistema deve responder a cada ação do usuário em menos de 1 segundo | Desempenho |
| RNF06 | O código deve seguir boas práticas de JavaScript modular e legível | Manutenibilidade |

---

### 📐 Regras de Negócio

| ID | Regra |
|----|-------|
| RN01 | A dificuldade de uma disciplina deve ser um valor inteiro entre 1 e 10 |
| RN02 | O tipo de avaliação aceito é: `Prova`, `Projeto`, `Trabalho`, `Seminário` ou `Outros` |
| RN03 | A prioridade de estudo é calculada pela fórmula: `(peso × dificuldade × fatorTipo) / diasRestantes` |
| RN04 | O fator por tipo de avaliação é fixo: Prova = 3.0 · Projeto = 2.5 · Trabalho = 2.0 · Seminário = 1.5 · Outros = 1.0 |
| RN05 | A cada hora estudada registrada, o usuário recebe exatamente 10 XP |
| RN06 | A progressão de nível segue a tabela definida de XP acumulado (ver seção de Gamificação) |
| RN07 | Os dados são mantidos em memória (array) durante a execução; não há persistência entre sessões |

---

## 🏗️ Arquitetura do Projeto

O projeto segue uma arquitetura em camadas com separação clara de responsabilidades:

```
index.js
  └── Controller  →  recebe input do usuário
        └── Services  →  aplica a lógica de negócio
              └── Database  →  mantém os dados em memória
```

| Camada | Arquivo | Responsabilidade |
|--------|---------|-----------------|
| Entrada | `index.js` | Inicializa a aplicação |
| Controller | `studyController.js` | Gerencia o menu e interação via `readline` |
| Service | `studyService.js` | CRUD de disciplinas e avaliações |
| Service | `recommendationService.js` | Calcula prioridades de estudo |
| Service | `achievementService.js` | Gerencia XP e níveis |
| Database | `studyDatabase.js` | Armazenamento em memória |
| View | `menuView.js` | Renderização do menu no terminal |

---

## ⚙️ Funcionalidades

| Opção | Descrição |
|-------|-----------|
| `1` | Adicionar disciplina (nome + dificuldade 1–10) |
| `2` | Adicionar avaliação (disciplina, tipo, peso, dias restantes) |
| `3` | Registrar horas estudadas |
| `4` | Listar todas as disciplinas com seus dados |
| `5` | Ver prioridades de estudo |
| `6` | Ver XP e Nível atual |
| `0` | Sair da aplicação |

---

## 📊 Cálculo de Prioridade

A urgência de estudo para cada avaliação é determinada pela seguinte fórmula:

```
Prioridade = (peso × dificuldade × fatorTipo) / diasRestantes
```

**Fatores por tipo de avaliação:**

| Tipo | Fator |
|------|-------|
| Prova | 3.0 |
| Projeto | 2.5 |
| Trabalho | 2.0 |
| Seminário | 1.5 |
| Outros | 1.0 |

> Quanto **maior** o resultado, **mais urgente** é o estudo para aquela avaliação.

---

## 🎮 Sistema de Gamificação

A cada hora de estudo registrada, o usuário ganha **10 XP**. A progressão de nível segue a tabela abaixo:

| Nível | XP Necessário |
|-------|--------------|
| 🥉 Nível 1 | 0 – 99 XP |
| 🥈 Nível 2 | 100 – 299 XP |
| 🥇 Nível 3 | 300 – 399 XP |
| 🏅 Nível 4 | 400 – 499 XP |
| 🏆 Nível 5 | 500+ XP |

---

## 🛠️ Tecnologias Utilizadas

- [Node.js](https://nodejs.org/) — Runtime JavaScript
- **JavaScript puro** — Sem frameworks adicionais
- [Jest](https://jestjs.io/) — Testes unitários
- **readline** — Interface interativa no terminal

---

## 📁 Estrutura de Pastas

```
studyflow/
├── .github/
├── src/
│   ├── controllers/
│   │   └── studyController.js
│   ├── database/
│   │   └── studyDatabase.js
│   ├── services/
│   │   ├── achievementService.js
│   │   ├── recommendationService.js
│   │   └── studyService.js
│   ├── tests/
│   │   ├── achievement.test.js
│   │   ├── recommendation.test.js
│   │   └── study.test.js
│   └── views/
│       └── menuView.js
├── .gitignore
├── index.js
├── package.json
└── package-lock.json
```

---

## ▶️ Como Executar

**Pré-requisito:** Node.js instalado na máquina.

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/studyflow.git

# Acesse a pasta do projeto
cd studyflow

# Instale as dependências
npm install

# Execute a aplicação
node index.js
```

---

## 🧪 Testes

O projeto utiliza **Jest** para testes unitários das camadas de serviço.

```bash
# Rodar todos os testes
npm test
```

Arquivos de teste disponíveis:

- `achievement.test.js` — Testa o sistema de XP e níveis
- `recommendation.test.js` — Testa o cálculo de prioridades
- `study.test.js` — Testa o CRUD de disciplinas e avaliações

---

## 👨‍💻 Autor

Desenvolvido como projeto pessoal para praticar **arquitetura limpa**, **separação de camadas** e construção de ferramentas úteis no dia a dia com Node.js.

---

> *"Organize seus estudos, ganhe XP, suba de nível."* 🚀
