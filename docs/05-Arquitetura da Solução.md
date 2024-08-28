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

const uri = 'mongodb://'user':'password'@server/collection'

mongoose
  .connect(uri)
  .then(() => console.log('Connected to Mon!'))
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

Descreva aqui qual(is) tecnologias você vai usar para resolver o seu problema, ou seja, implementar a sua solução. Liste todas as tecnologias envolvidas, linguagens a serem utilizadas, serviços web, frameworks, bibliotecas, IDEs de desenvolvimento, e ferramentas.

Apresente também uma figura explicando como as tecnologias estão relacionadas ou como uma interação do usuário com o sistema vai ser conduzida, por onde ela passa até retornar uma resposta ao usuário.

## Hospedagem

Explique como a hospedagem e o lançamento da plataforma foi feita.

> **Links Úteis**:
>
> - [Website com GitHub Pages](https://pages.github.com/)
> - [Programação colaborativa com Repl.it](https://repl.it/)
> - [Getting Started with Heroku](https://devcenter.heroku.com/start)
> - [Publicando Seu Site No Heroku](http://pythonclub.com.br/publicando-seu-hello-world-no-heroku.html)

## Qualidade de Software

Conceituar qualidade de fato é uma tarefa complexa, mas ela pode ser vista como um método gerencial que através de procedimentos disseminados por toda a organização, busca garantir um produto final que satisfaça às expectativas dos stakeholders.

No contexto de desenvolvimento de software, qualidade pode ser entendida como um conjunto de características a serem satisfeitas, de modo que o produto de software atenda às necessidades de seus usuários. Entretanto, tal nível de satisfação nem sempre é alcançado de forma espontânea, devendo ser continuamente construído. Assim, a qualidade do produto depende fortemente do seu respectivo processo de desenvolvimento.

A norma internacional ISO/IEC 25010, que é uma atualização da ISO/IEC 9126, define oito características e 30 subcaracterísticas de qualidade para produtos de software.
Com base nessas características e nas respectivas sub-características, identifique as sub-características que sua equipe utilizará como base para nortear o desenvolvimento do projeto de software considerando-se alguns aspectos simples de qualidade. Justifique as subcaracterísticas escolhidas pelo time e elenque as métricas que permitirão a equipe avaliar os objetos de interesse.

> **Links Úteis**:
>
> - [ISO/IEC 25010:2011 - Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models](https://www.iso.org/standard/35733.html/)
> - [Análise sobre a ISO 9126 – NBR 13596](https://www.tiespecialistas.com.br/analise-sobre-iso-9126-nbr-13596/)
> - [Qualidade de Software - Engenharia de Software 29](https://www.devmedia.com.br/qualidade-de-software-engenharia-de-software-29/18209/)
