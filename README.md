# Solidity Parser

A Solidity parser in Javascript. So we can evaluate and alter Solidity code without resorting to cruddy preprocessing.  

### :warn: WARNING :warn: 

This is pre-alpha software. The goal of it is to take Solidity code as input and return an object as output that can be used to correctly describe that Solidity code. The structure of the resultant object is highly likely to change as the parser's features get filled out. **This parser is set to ignore Solidity constructs it's not yet able to handle.** Or, it might just error. So watch out.

### Usage

**Library**

```
npm install solidity-parser
```

Then, in your code:

```javascript
var SolidityParser = require("solidity-parser");

// Parse Solidity code as a string:
var result = SolidityParser.parse("contract { ... }");

// Or, parse a file: 
var result = SolidityParser.parseFile("./path/to/file.sol");
```

**Command Line** (for convenience)

```
$ solidity-parser ./path/to/file.js 
```

### Results

Consider this solidity code as input:

```
import "Foo.sol";

contract MyContract {
  mapping (uint => address) public addresses;
}
```

You'll receiving the following (or something very similar) as output. Note that the structure of mappings could be made more clear, and this will likely be changed in the future. 

```json
{
  "type": "Program",
  "body": [
    {
      "type": "ImportStatement",
      "value": "Foo.sol"
    },
    {
      "type": "ContractStatement",
      "name": "MyContract",
      "is": [],
      "body": [
        {
          "type": "ExpressionStatement",
          "expression": {
            "type": "DeclarativeExpression",
            "name": "addresses",
            "literal": {
              "type": "Type",
              "literal": {
                "type": "MappingExpression",
                "from": {
                  "type": "Type",
                  "literal": "uint",
                  "members": [],
                  "array_parts": []
                },
                "to": {
                  "type": "Type",
                  "literal": "address",
                  "members": [],
                  "array_parts": []
                }
              },
              "members": [],
              "array_parts": []
            },
            "is_constant": false,
            "is_public": true
          }
        }
      ]
    }
  ]
}
```

### Test

In a checkout of the project, run:

```
$ npm test
```

### License

MIT