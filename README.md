# TD/RCM ifu generator

**T**ransfert des **d**éclarations de **r**evenus de **c**apitaux **m**obiliers par procédé informatique

[![Codeship](https://img.shields.io/codeship/c1590490-c92d-0134-076b-6a795a0b4831/master.svg)](https://app.codeship.com/projects/199198) [![npm](https://img.shields.io/npm/dt/td-rcm.svg)](https://www.npmjs.com/package/td-rcm) [![npm](https://img.shields.io/npm/v/td-rcm.svg)](https://www.npmjs.com/package/td-rcm) [![license](https://img.shields.io/github/license/lendix/node-td-rcm.svg)](https://github.com/Lendix/node-td-rcm/blob/master/LICENSE.md)

## Install

```JavaScript
npm i td-rcm
```

## Usage

```
import {
  IndicativeArea,
  IssuerAddress,
  D0Issuer,
  RecipientAddress,
  R1Recipient,
  TaxCredit,
  FixedIncomeProducts,
  CrowdfundingProducts,
  IncomeSubjectToIncomeTax,
  R2Amount,
  RCM
} from 'td-rcm';

const indicativeArea = new IndicativeArea({
  year: '2016',
  siret: '12345678912345',
  type: 1
});

/*
  Article déclarant D0
 */
const issuerAddress = new IssuerAddress({
  zipCode: '75009',
  streetNumber: '42',
  streetName: 'Rue des loutres',
  officeDistributor: 'Paris',
  issuanceDate: '20161224'
});

const d0 = new D0Issuer(indicativeArea.issuer({socialReason: 'Loutre SAS'}), issuerAddress);
/* */

/*
  Article Bénéficiaire R1
 */
const r1 = new R1Recipient({
  recipientIndicativeArea: indicativeArea.recipient({ recipientCode: 'B' }),
  recipientType: 2,
  recipient: {
    familyname: 'Henry',
    firstnames: 'Mattéo',
    sex: 2
  },
  birth: {
    year: 1980,
    month: 5,
    day: 22,
    departementCode: '69',
    city: 'Lyon'
  },
  recipientAddress: new RecipientAddress({
    zipCode: '75009',
    officeDistributor: 'Paris'
  })
}
/* */

/*
  Article Montant R2
 */
const taxCredit = new TaxCredit({AD: 9});
const fixedIncomeProducts = new FixedIncomeProducts({AR: 38});
const crowdfundingProducts = new CrowdfundingProducts({KR: 78, KS: 2});
const incomeSubjectToIncomeTax = new IncomeSubjectToIncomeTax({BU: 38});

const r2 = new R2Amount({
  amountIndicativeArea: indicativeArea.amountR2(), 
  taxCredit,
  incomeSubjectToIncomeTax,
  fixedIncomeProducts,
  crowdfundingProducts
  // ...
});
/* */

const rcm = new RCM(d0, {
  fullname: 'Robert Patron',
  phone: '0123456789',
  email: 'robert@patron.com'
});

rcm.addRecipient(r1, r2);

try {
  const ex = rcm.export(true);
}
catch (e) {
  console.log(e)
}

```

## Implemented items

We only implemented what we needed:

R3, R4 are not implemented
Only 5 blocks are implemented on R2

Don't hesitate to pull-request new block ;)
