# connex-entity-builder
A simple builder that reads Truffle generated JSON interfaces and generates imports and entities

## Install

`npm i @decent-bet/connex-entity-builder`

## How to use with Truffle migrations

1. Add library: `'const ContractImportBuilder = require('@decent-bet/connex-entity-builder')`
2. Create instance and set output
```javascript
module.exports = (web3, dbet, deployer, args) => {
    const builder = new ContractImportBuilder();
    builder.setOutput(`./npm/index.js`);
    return new MigrationScript(web3, dbet, deployer, builder, args)
}
```
3. Configure write event

```javascript
    builder.onWrite = (output) => {
        fs.writeFileSync(`./npm/index.js`, output)
    }
```

4. Add contracts. This will generate a `contract import` which has the following schema:

```javascript
{
    VERSION: string,
    [name]: {
        raw: {
            abi: "<_jsonInterface from Truffle ABI>"
        },
        address: {
            [chaintag]: "<address>"
        }
    }
}
```

The `addContract` method has the following parameters:

* **name**: The friendly name of the contract. 
* **contract**: Contract object from a Truffle migration.
* **address**: The deployed address of a contract.
* **chaintag**: The current chaintag or net being deployed.

### Example:
```javascript
builder.addContract("TokenContract", Token, token.options.address, chain);
```


## Pending

* Generate basic connex contract entities mapped to contract import
* Enable CI process to `npm publish` once a PR is merged to master