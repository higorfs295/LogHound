# Documento de Requisitos de Dados (DRD)

## 1. Identificação

- *Projeto:* 
- *Tipo de documento:* Documento de Requisitos de Dados (DRD)
- *Instituição responsável:* Instituto de Informática – UFG
- *Autor:* Higor Ferreira & Matheus Marinho
- *Data:* 26/08/2025
- *Versão atual:* 0.1

---

## 2. Controle de Versão

| Versão | Data       | Autor           | Descrição da alteração                         |
|--------|------------|------------------|------------------------------------------------|
| 0.1    | 20/08/2025 | Cássio Rodrigues | Versão inicial do documento                    |
| 0.9    | 23/08/2025 | Cássio Rodrigues | Revisão e inclusão de exemplos de atributos    |
| 1.0    | 25/08/2025 | Cássio Rodrigues | Versão final aprovada                          |

---

## 3. Introdução

O presente documento tem como objetivo descrever os requisitos de dados para o *Sistema de Gestão das Provas do ENADE*. A proposta é registrar e organizar informações relativas às edições da avaliação, cursos participantes, provas aplicadas, questões, alternativas de resposta, estudantes envolvidos e seus desempenhos.

As regras de negócio são apresentadas em linguagem natural, sem segmentação explícita em elementos técnicos de modelagem. A interpretação e derivação dos componentes do Modelo Entidade-Relacionamento (MER) ficam a critério do analista que utilizar este documento como insumo para a etapa de modelagem.

---

## 4. Escopo

O sistema permitirá o registro histórico e organizado das edições do ENADE, identificando os cursos avaliados, provas aplicadas, estudantes que participaram, itens presentes em cada prova, alternativas disponíveis e respostas efetivamente fornecidas. Também será possível derivar informações adicionais, como a idade dos estudantes, ou consolidar relatórios de desempenho.

---

## 5. Regras de Negócio

A seguir estão as principais regras de negócio descritas em linguagem natural. Elas servem como base para a extração de entidades, atributos e relacionamentos no modelo de dados.

### 5.1 Edições

- Cada aplicação do ENADE é registrada como uma *edição* distinta, associada a um *ano* de realização e a uma *área de avaliação*.
- Uma *edição* agrupa as provas e os cursos avaliados naquele ano/área.

### 5.2 Cursos e Instituições

- Para cada edição, estarão vinculados os *cursos de graduação participantes*.
- Deve-se guardar a *denominação do curso* e a *instituição de ensino* a que pertence.

### 5.3 Estudantes

- Os estudantes vinculados aos cursos são cadastrados individualmente, com *identificação única por documento oficial*.
- Informações pessoais a armazenar: *nome, **data de nascimento,**gênero,**CPF*.
- É permitido que o estudante *possua mais de um endereço de e-mail*.
- O *nome* pode ser tratado de forma detalhada (por exemplo: primeiro nome e sobrenome separados).
- A *idade* não será armazenada explicitamente; deverá ser *calculada a partir da data de nascimento* quando necessário.
- Todo estudante deve estar sempre vinculado a um curso.

### 5.4 Provas

- Para cada edição do ENADE, *uma ou mais provas* podem ser aplicadas.
- A *prova* é sempre vinculada a uma *edição específica*; não pode existir prova sem edição.
- Cada prova é composta por um conjunto de *questões* (obrigatório ter várias questões).

### 5.5 Questões

- Cada questão contém um *enunciado* e deve estar associada a um *tipo* (objetiva ou discursiva).
- *Questões objetivas* possuem *alternativas de resposta*, vinculadas à questão correspondente.
- A existência de uma alternativa está condicionada à existência da questão; *não pode haver alternativa sem questão*.

### 5.6 Alternativas

- Cada alternativa é identificada por uma *letra* (A, B, C, D ou E) e possui um *texto explicativo*.
- Entre as alternativas de uma questão objetiva, *apenas uma deve ser marcada como correta*.

### 5.7 Respostas e Participação

- A participação do estudante é registrada como um *vínculo entre estudante e prova*.
- Para cada resposta, registrar: a *alternativa escolhida* (se objetiva), *tempo gasto* e demais dados relevantes.
- Presume-se que, após realizar a prova, todas as questões foram processadas — questões em branco devem ser registradas como tal.
- A resolução da prova por um estudante gera obrigatoriamente *respostas para todas as questões* presentes na prova (mesmo que em branco).

### 5.8 Restrições e Dependências

- A *prova* deve pertencer a uma *edição*.
- A *questão* só existe no contexto de uma *prova*.
- As *alternativas* só existem no contexto de uma *questão*.
- Todo estudante deve estar vinculado a um *curso*.

---

## 6. Considerações Finais

As regras de negócio aqui descritas permitem a elaboração de um modelo conceitual consistente para a gestão das provas do ENADE. Apesar de não haver detalhamento técnico explícito sobre entidades, relacionamentos ou atributos, os elementos estão implicitamente presentes na narrativa e podem ser derivados pelo analista responsável pela modelagem.

Dessa forma, o documento preserva clareza para stakeholders não técnicos, ao mesmo tempo em que fornece a base necessária para a modelagem formal de dados.

---

### Sumário (estrutural)

1. Identificação
2. Controle de Versão
3. Introdução
4. Escopo
5. Regras de Negócio
6. Considerações Finais
