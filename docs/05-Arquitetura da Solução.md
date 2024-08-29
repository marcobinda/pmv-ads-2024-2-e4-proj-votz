# Arquitetura da Solução

![Arquitetura da Solução](img/votz_arquitetura.png)

## Diagrama de Classes

![Diagrama de Classes](img/votz_diagramaclasses.png)

## Modelo ER

![Modelo Entidade-Relacionamento](img/votz_modeloER.png)

## Esquema Relacional

![Esquema Relacional](img/votz_esquemaRelacional.png)

## Modelo Físico


**Conexão com o MongoDB**

```
// db.js
const mongoose = require('mongoose');

const uri = 'mongodb://'user':'password'@server/database'

mongoose
  .connect(uri)
  .then(() => console.log('Connected to MongoDB!'))
  .catch(err => console.error('Error connecting to MongoDB:', err));

module.exports = mongoose;
```
**Criação das *collections***

```
// models.js
const mongoose = require('./db');
const Schema = mongoose.Schema;

// Administrator Schema
const adminSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
});

const Administrator = mongoose.model('Administrator', adminSchema);

// Candidate Schema
const candidateSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  profile: { type: String, required: true },
});

const Candidate = mongoose.model('Candidate', candidateSchema);

// Voter Schema
const voterSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  emailStatus:  { type: Boolean, default: false },
  voteStatus: { type: Boolean, default: false },
});

const Voter = mongoose.model('Voter', voterSchema);

// Auditor Schema
const auditorSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  profile: { type: String, required: true },
});

const Auditor = mongoose.model('Auditor', auditorSchema);

// Election Schema
const electionSchema = new Schema({
  title: { type: String, required: true },
  description: { type: String },
  startDate: { type: Date, required: true },
  endDate: { type: Date, required: true },
  isVoteRequired: { type: Boolean, default: false },
});

const Election = mongoose.model('Election', electionSchema);

// Voting Schema
const votingSchema = new Schema({
  electionId: { type: Schema.Types.ObjectId, ref: 'Election', required: true },
  voterId: [{ type: Schema.Types.ObjectId, ref: 'Voter' }],
  candidatesId: [{ type: Schema.Types.ObjectId, ref: 'Candidate' }],
  auditorId: [{ type: Schema.Types.ObjectId, ref: 'Auditor' }],
  dateTime: { type: Date, default: Date.now },
});

const Voting = mongoose.model('Voting', votingSchema);

// Report Schema
const reportSchema = new Schema({
  votingId: { type: Schema.Types.ObjectId, ref: 'Voting', required: true },
  reportType: { type: String, required: true },
  data: { type: Array, default: [] },
});

const Report = mongoose.model('Report', reportSchema);

// Receipt Schema
const receiptSchema = new Schema({
  voterId: [{ type: Schema.Types.ObjectId, ref: 'Voter', required: true }],
  electionId: { type: Schema.Types.ObjectId, ref: 'Election', required: true },
  dateTime: { type: Date, default: Date.now },
  details: { type: String },
});

const Receipt = mongoose.model('Receipt', receiptSchema);

module.exports = {
  Administrator,
  Candidate,
  Voter,
  Auditor,
  Election,
  Voting,
  Report,
  Receipt,
};

```

## Tecnologias Utilizadas

**Frontend Web**: ReactJS

**Frontend Mobile**: React Native

**Backend**: NodeJS

**Database**: MongoDB

## Hospedagem

Microsft Azure (à confirmar)

## Qualidade de Software

| **Característica**       | **Descrição**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Funcionalidade           | Deve permitir o cadastro e gerenciamento de eleições, candidatos, eleitores e auditores. <br> Deve permitir agendar o início e o término da eleição. <br> Deve possibilitar a apuração dos votos e a exibição de relatórios e gráficos de apuração. <br> Deve permitir a exportação de relatórios em formatos padrão, como CSV. <br> Deve possibilitar a customização do layout da tela de votação. <br> Deve permitir o envio de comprovantes de votação.                 |
| Confiabilidade           | Deve ser capaz de operar de forma contínua durante o período eleitoral, sem interrupções não planejadas. <br> Deve garantir que os dados de votação sejam armazenados de forma segura e que nenhuma perda de dados ocorra. <br> Deve ter uma taxa de disponibilidade de pelo menos 99% durante o período eleitoral.                                                           |
| Usabilidade              | Deve possuir interface intuitiva e fácil de usar, com orientações claras para eleitores, administradores e auditores. <br> Deve haver documentação adequada e tutoriais disponíveis para orientar os usuários no uso do sistema. <br> Deve fornecer feedback apropriado para ações dos usuários, como confirmação de votos e mensagens de erro claras. <br> Deve possuir layout personalizável para atender às necessidades de diferentes eleições e usuários.                                                   |
| Eficiência de Desempenho | Deve ser capaz de processar o registro de votos e gerar relatórios rapidamente, mesmo com um grande número de eleitores e candidatos. <br> Deve suportar o acesso simultâneo de múltiplos usuários (eleitores, administradores, auditores) sem degradação significativa do desempenho.                                                                                                                                                                                                                                    |
| Compatibilidade          | Deve ser compatível com os principais navegadores da web e dispositivos móveis. <br> Deve suportar diferentes formatos de entrada e saída para importação e exportação de dados, como planilhas CSV.                                                                                                                                                                                                |
| Segurança                | Deve garantir que apenas usuários autorizados possam acessar funcionalidades específicas. <br> Deve armazenar dados de autenticação de forma segura usando técnicas de hashing. <br> Deve haver proteção contra ataques comuns de segurança, como SQL injection, cross-site scripting (XSS) e cross-site request forgery (CSRF). <br> Deve criptografar dados sensíveis, como informações de identificação pessoal (PII) e votos.               |
| Manutenibilidade         | Deve seguir boas práticas de desenvolvimento, incluindo padrões de codificação, modularidade e uso de comentários apropriados. <br> Deve haver documentação completa do código, arquitetura e design do sistema para facilitar a manutenção futura. <br> Deve ser projetado para permitir a fácil adição de novas funcionalidades ou ajustes sem impacto negativo no sistema existente. <br> Deve haver uma suite de testes para garantir que as alterações de código não introduzam novos bugs. |
| Portabilidade            | Deve utilizar tecnologias padrão da indústria que sejam amplamente suportadas em diversas plataformas. <br> Deve ser possível mover o sistema de um ambiente de desenvolvimento para um ambiente de produção com o mínimo de configuração necessária.                                                                                                                                                                                                                                     |