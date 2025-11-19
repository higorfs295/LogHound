# 🐕 LogHound

> Sistema de monitoramento, análise e gestão de eventos de segurança para ambientes OT/ICS

[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)
[![Prisma](https://img.shields.io/badge/Prisma-6.14-blue.svg)](https://www.prisma.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-blue.svg)](https://www.postgresql.org/)

## 📋 Índice

- [Sobre o Projeto](#-sobre-o-projeto)
- [Características Principais](#-características-principais)
- [Arquitetura e Modelo de Dados](#-arquitetura-e-modelo-de-dados)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Instalação e Configuração](#-instalação-e-configuração)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Requisitos Funcionais](#-requisitos-funcionais)
- [Restrições de Integridade](#-restrições-de-integridade)
- [Contribuindo](#-contribuindo)

## 🎯 Sobre o Projeto

O **LogHound** é um sistema robusto desenvolvido para fornecer funcionalidades de fácil acesso e manuseio por parte do usuário, garantindo capacidade de monitoramento, análise e gestão de eventos de segurança em ambientes de Tecnologia Operacional (OT) e Sistemas de Controle Industrial (ICS).

O sistema utiliza o **modelo relacional** como paradigma de modelagem de dados, oferecendo simplicidade conceitual, base matemática sólida e flexibilidade para realização de consultas complexas, essenciais no contexto de segurança cibernética industrial.

### Objetivos

- Centralizar o gerenciamento de ativos industriais e suas vulnerabilidades
- Registrar e analisar eventos de segurança em tempo real
- Rastrear indicadores de comprometimento (IOCs) e ameaças
- Modelar e visualizar cenários de ataque
- Gerenciar casos de segurança e evidências forenses
- Fornecer relatórios e análises para tomada de decisão

## ✨ Características Principais

### 🔐 Gerenciamento de Ativos

- **Cadastro completo de ativos** com identificador único
- Suporte para diferentes tipos de ativos (dispositivos físicos ou aplicações)
- Atributos essenciais: nome e tipo para classificação
- Rastreamento de ativos afetados por incidentes de segurança

### 📊 Registro e Gestão de Incidentes

- **Registro de incidentes de segurança** com data/hora obrigatória
- Controle de status dos incidentes (aberto, fechado, em análise, etc.)
- Associação de incidentes a usuários responsáveis pelo registro/tratamento
- Rastreamento temporal completo de eventos

### 🎯 Gestão de Ameaças

- **Cadastro de tipos de ameaças** (malware, negação de serviço, etc.)
- Associação de ameaças a incidentes através do relacionamento ternário OCORRÊNCIA
- Modelagem da relação complexa entre incidente, ativo e ameaça
- Suporte para múltiplas ameaças por incidente e múltiplos ativos afetados

### 🚨 Gestão de Evidências

- **Registro de evidências** vinculadas a incidentes específicos
- Armazenamento de arquivos/logs como evidências
- Descrição detalhada de cada evidência
- **Entidade fraca:** Evidências dependem de incidentes para existir
- Exclusão em cascata: remoção de incidente remove suas evidências

### 👥 Gestão de Usuários

- Sistema de usuários com identificação única
- Rastreamento de usuários responsáveis por incidentes
- Informações básicas: nome e e-mail
- Relacionamento 1:N com incidentes (um usuário pode registrar vários incidentes)

### 🔗 Modelagem de Relacionamentos Complexos

- **Relacionamento ternário OCORRÊNCIA:** Modela a ocorrência de um incidente relacionando simultaneamente incidente, ativo(s) e ameaça(s)
- **Relacionamento identificador POSSUI:** Garante que evidências sempre estejam vinculadas a um incidente
- Integridade referencial garantida em todos os relacionamentos

## 🏗️ Arquitetura e Modelo de Dados

O LogHound utiliza o **modelo relacional** como paradigma de modelagem de dados, oferecendo simplicidade conceitual, base matemática sólida e flexibilidade para realização de consultas complexas. O sistema é modelado através do Prisma ORM e implementado em PostgreSQL.

### Modelo Entidade-Relacionamento (MER)

O modelo conceitual do LogHound é composto pelas seguintes entidades:

#### Entidades Fortes

| Entidade | Atributos | Descrição |
|----------|-----------|-----------|
| **USUÁRIO** | `ID_Usuario` (PK), `Nome`, `E-mail` | Usuários do sistema que registram e tratam incidentes |
| **INCIDENTE** | `ID_Incidente` (PK), `Data/Hora`, `Status` | Eventos de segurança registrados no sistema |
| **ATIVO** | `ID_Ativo` (PK), `Nome`, `Tipo` | Ativos do sistema (dispositivos ou aplicações) |
| **AMEAÇA** | `ID_Ameaça` (PK), `Nome` | Tipos de ameaças de segurança (malware, negação de serviço, etc.) |

#### Entidade Fraca

| Entidade | Atributos | Descrição |
|----------|-----------|-----------|
| **EVIDÊNCIA** | `ID_Evidência` (PK parcial), `Arquivo/Log`, `Descrição` | Evidências coletadas que comprovam um incidente |

### Relacionamentos

#### 1. REGISTRA (Usuário → Incidente)
- **Cardinalidade:** 1:N (Um usuário pode registrar vários incidentes)
- **Descrição:** Relaciona o usuário responsável pelo registro/tratamento do incidente

#### 2. OCORRÊNCIA (Relacionamento Ternário)
- **Entidades:** INCIDENTE, ATIVO, AMEAÇA
- **Cardinalidades:**
  - INCIDENTE (1) : Um incidente está relacionado a uma ocorrência específica
  - ATIVO (N) : Um incidente pode afetar múltiplos ativos
  - AMEAÇA (N) : Um incidente pode estar relacionado a múltiplas ameaças
- **Descrição:** Modela a ocorrência de um incidente de segurança, relacionando qual ameaça afetou quais ativos

#### 3. POSSUI (Relacionamento Identificador - Incidente → Evidência)
- **Cardinalidade:** 1:N (Um incidente pode possuir várias evidências)
- **Tipo:** Relacionamento identificador (EVIDÊNCIA é entidade fraca)
- **Descrição:** Relaciona evidências coletadas a um incidente específico. A exclusão de um incidente implica na exclusão de todas as suas evidências.

### Diagrama de Relacionamentos

```
USUÁRIO (1) ──[REGISTRA]──> (N) INCIDENTE (1) ──[POSSUI]──> (N) EVIDÊNCIA
                                 │
                                 │
                    [OCORRÊNCIA] │
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              (N) ATIVO                  (N) AMEAÇA
```

### Características do Modelo

- **Simplicidade Conceitual:** Modelo focado nas entidades essenciais do domínio
- **Integridade Referencial:** Garante que incidentes só existam com ativos e ameaças válidos
- **Entidade Fraca:** EVIDÊNCIA depende de INCIDENTE para existir
- **Relacionamento Ternário:** OCORRÊNCIA modela a complexidade de relacionar incidente, ativo e ameaça simultaneamente

## 🛠️ Tecnologias Utilizadas

- **Node.js** - Runtime JavaScript
- **Prisma ORM** - ORM para modelagem e acesso ao banco de dados
- **PostgreSQL** - Banco de dados relacional
- **TypeScript/JavaScript** - Linguagem de programação

## 🚀 Instalação e Configuração

### Pré-requisitos

- Node.js 18+ instalado
- PostgreSQL instalado e rodando
- npm ou yarn

### Passos de Instalação

1. **Clone o repositório**
   ```bash
   git clone <url-do-repositorio>
   cd LogHound
   ```

2. **Instale as dependências**
   ```bash
   npm install
   ```

3. **Configure o banco de dados**
   
   Crie um arquivo `.env` na raiz do projeto com a seguinte configuração:
   ```env
   DATABASE_URL="postgresql://usuario:senha@localhost:5432/loghound?schema=public"
   ```

4. **Execute as migrações**
   ```bash
   npx prisma migrate dev
   ```

5. **Gere o cliente Prisma**
   ```bash
   npx prisma generate
   ```

6. **Opcional: Visualize o banco de dados**
   ```bash
   npx prisma studio
   ```

## 📁 Estrutura do Projeto

```
LogHound/
├── prisma/
│   └── schema.prisma          # Schema do banco de dados
├── node_modules/              # Dependências
├── package.json               # Configuração do projeto
├── package-lock.json          # Lock de dependências
├── .env                       # Variáveis de ambiente (não versionado)
├── README.md                  # Este arquivo
├── DRD.md                     # Documento de Requisitos de Dados
└── MER.png                    # Diagrama do Modelo Entidade-Relacionamento
```

## 📋 Requisitos Funcionais

### 2.1 Gerenciamento de Ativos e Seus Componentes

- ✅ Cadastro de ativos com identificador único
- ✅ Suporte para tipos: dispositivo (hardware) e aplicação (software)
- ✅ Rastreamento de hierarquia de ativos (composição)
- ✅ Registro de vulnerabilidades por ativo
- ✅ Relacionamento N:M entre ativos e vulnerabilidades

### 2.2 Registro e Análise de Eventos de Segurança

- ✅ Registro de indicadores de segurança (logs, alertas)
- ✅ Associação obrigatória de indicadores a ativos
- ✅ Data e hora obrigatórias para todos os eventos
- ✅ Registro de incidentes a partir de indicadores
- ✅ Associação de incidentes a múltiplos ativos
- ✅ Classificação de incidentes por tipo de ameaça

### 2.3 Gestão de Ameaças, Incidentes e Evidências

- ✅ Cadastro de tipos de ameaças (malware, negação de serviço, etc.)
- ✅ Validação: incidente requer ativo e ameaça relacionados
- ✅ Adição de evidências a incidentes
- ✅ Vinculação obrigatória de evidências a incidentes

### 2.4 Modelagem e Visualização de Cenários de Ataque

- ✅ Modelagem de cenários de ataque
- ✅ Relacionamento entre ativos vulneráveis, ameaças, incidentes e evidências
- ✅ Visualização de relações complexas

### 2.5 Consultas e Relatórios

- ✅ Contagem de ameaças por tipo
- ✅ Número de incidentes por ativo
- ✅ Total de vulnerabilidades por ativo
- ✅ Listagem de incidentes por período, agrupados por ativo
- ✅ Identificação do ativo com maior número de vulnerabilidades
- ✅ Busca de evidências por tipo de incidente e data

## 🔒 Restrições de Integridade

### 2.2.1 Integridade da Entidade

- ✅ Identificador único para cada ativo, incidente, ameaça e cenário
- ✅ Combinação única de tipo de vulnerabilidade e ativo

### 2.2.2 Integridade Referencial

- ✅ Incidente só pode ser registrado se ativo(s) e ameaça existirem
- ✅ Evidência não pode ser cadastrada sem incidente
- ✅ Exclusão em cascata: incidente → evidências

### 2.2.3 Integridade de Domínio

- ✅ Data e hora obrigatórias para indicadores
- ✅ Status de incidente em conjunto predefinido
- ✅ Identificador de ativo em formato padronizado

### 2.2.4 Integridade Definida pelo Usuário

- ✅ Cenário de ataque requer combinação válida de ativos e ameaças vinculada a incidente
- ✅ Data de evidência não pode ser anterior à data de início do incidente
- ✅ Identificador único de ativo filho dentro do contexto do ativo pai

## 🤝 Contribuindo

Contribuições são bem-vindas! Para contribuir com o projeto:

1. Faça um fork do repositório
2. Crie uma branch para sua feature (`git checkout -b feature/nova-funcionalidade`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova funcionalidade'`)
4. Push para a branch (`git push origin feature/nova-funcionalidade`)
5. Abra um Pull Request

### Padrões de Código

- Siga as convenções do Prisma para modelagem
- Documente funções e classes complexas
- Mantenha commits descritivos e organizados

## 📝 Licença

Este projeto está sob a licença ISC.

## 👥 Autores

- **Higor Ferreira**
- **Matheus Marinho**

---

**Instituto de Informática – UFG**

*Sistema desenvolvido para gestão e análise de eventos de segurança em ambientes OT/ICS*
