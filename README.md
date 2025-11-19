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

### 🔐 Gerenciamento de Ativos e Componentes

- **Cadastro completo de ativos** com identificador único
- Suporte para diferentes tipos de ativos (PLC, RTU, HMI, aplicações, etc.)
- Rastreamento de hierarquia de ativos (composição de sistemas)
- Registro de vulnerabilidades por ativo
- Classificação por criticidade e zona de rede (Field, Control, DMZ)

### 📊 Registro e Análise de Eventos de Segurança

- **Registro de eventos OT** com timestamp obrigatório
- Captura de protocolos industriais (Modbus, DNP3, IEC 61850, etc.)
- Detecção de anomalias em comunicações
- Associação de eventos a ativos específicos

### 🎯 Gestão de Indicadores e Ameaças

- **Cadastro de indicadores de segurança** (IP, hash, YARA, Sigma, etc.)
- Associação de indicadores a ameaças conhecidas
- Rastreamento de avistamentos (sightings) de indicadores
- Validação temporal de indicadores

### 🚨 Gestão de Casos e Evidências

- **Registro de casos de segurança** com status e responsáveis
- Cadeia de custódia (chain of custody) para evidências
- Armazenamento de evidências com hash SHA-256
- Rastreamento completo de ações sobre evidências

### 🎭 Modelagem de Cenários de Ataque

- **Criação de cenários de ataque** com técnicas MITRE ATT&CK
- Execução de cenários (attack runs) com rastreamento de resultados
- Associação de cenários a ativos e usuários
- Geração de IOCs a partir de execuções

### 👥 Gestão de Usuários

- Sistema de usuários com diferentes papéis (admin, analista, operador)
- Rastreamento de execuções de cenários por usuário

## 🏗️ Arquitetura e Modelo de Dados

O LogHound utiliza um modelo relacional baseado em PostgreSQL, modelado através do Prisma ORM. O sistema é composto pelas seguintes entidades principais:

### Entidades Principais

| Entidade | Descrição |
|----------|-----------|
| **Asset** | Ativos industriais (dispositivos, aplicações) |
| **OT_Event** | Eventos de segurança capturados em tempo real |
| **Indicator** | Indicadores de comprometimento (IOCs) |
| **ThreatEntity** | Entidades de ameaça (malware, campanhas, ferramentas) |
| **Sighting** | Avistamentos de indicadores em ativos |
| **Case** | Casos de segurança abertos para investigação |
| **Evidence** | Evidências forenses coletadas |
| **Custody** | Registros da cadeia de custódia |
| **AttackScenario** | Cenários de ataque modelados |
| **AttackRun** | Execuções de cenários de ataque |
| **AttackResult** | Resultados das execuções |
| **User** | Usuários do sistema |

### Relacionamentos Principais

```
Asset ──┬──> OT_Event (1:N)
        ├──> Sighting (1:N)
        └──> AttackRun (1:N)

Indicator ──┬──> Sighting (1:N)
            └──> IndicatorThreat (N:M) ──> ThreatEntity

Case ──> Evidence (1:N) ──> Custody (1:N)

AttackScenario ──> AttackRun (1:N) ──> AttackResult (1:N)
                                    └──> User (N:1)
                                    └──> Asset (N:1)
```

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
