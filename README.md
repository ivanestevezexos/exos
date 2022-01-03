
ISO8583 Parser for NodeJS

## Introduction

This library provides an easy way to pack and unpack ISO8583 messages. It is fully configurable allowing the optional change of the data element types and length used to pack and unpack the messages. Also its been modified to create ipm files acordign to mastercard standarts.
The message format is 4 bytes of message length, 1008 bytes of message and the last 2 bytes are null for closing the message.
In order to use it needs to be configured and provided wiht a json file that sends the message to be packed 
## Installation

```cmd
$ npm install ciso8583
```

## Usage

Using this library is fairly easy. Simply import and create an instance of it. Then take advantage of either the __pack()__ or __unpack()__ methods:

```javascript
const cISO8583 = require('ciso8583');

let iso8583Parser = new cISO8583();
```

### cISO8583.pack()

This method allows you to pack data elements supplied as a json object in the format dataElementNumber: fieldData alongside the message type indicator (MTI) to form the packed ISO8583 message. It returns an object that also contains the bitmap in both binary and hexadecimal form, the data element part of the message, the MTI and if it has a secondary bitmap type.
Each data element can be personalized for any kind of mastercard type of message.
After a server is installed an provided a curl must be send to the server for the library to process the message
Curl example: 
curl --request POST \
  --url http://localhost:8090/pos \
  --header 'Content-Type: application/json' \
  --data '{
  "0": "0100",
  "2": "4761739001010465",
  "3": "000000",
  "4": "000000003500",
  "11": "113905",
  "14": "2512",
  "18": "4111",
  "22": "051",
  "23": "001",
  "25": "00",
  "26": "12",
  "32": "777777",
  "33": "111111111",
  "35": "4761739001010465D25122011172745689",
  "37": "820611123456",
  "41": "00000001",
  "42": "TERMINAL_000001",
  "43": "TERMINAL  Merchant 1 0000000 NAIROBI KE ",
  "49": "404",
  "56": "1510",
  "123": "51010151134C101",
  "127": 
```javascript
const cISO8583 = require('ciso8583');

let iso8583Parser = new cISO8583();

let dataElements = {
    '2': '1234567890123456',
    '3': null,
    '4': null,
    '5': null,
    '6': null,
    '7': '0609173030',
    '8': null,
    '9': null,
    '10': null,
    '11': null,
    '12': null,
    '13': null,
    '14': null,
    '15': null,
    '16': null,
    '17': null,
    '18': null,
    '19': null,
    '20': null,
    '21': null,
    '22': '123',
    '23': null,
    '24': null,
    '25': null,
    '26': null,
    '27': null,
    '28': null,
    '29': null,
    '30': null,
    '31': null,
    '32': null,
    '33': null,
    '34': null,
    '35': null,
    '36': null,
    '37': null,
    '38': null,
    '39': null,
    '40': null,
    '41': null,
    '42': null,
    '43': null,
    '44': null,
    '45': null,
    '46': null,
    '47': null,
    '48': null,
    '49': null,
    '50': null,
    '51': null,
    '52': null,
    '53': null,
    '54': null,
    '55': null,
    '56': null,
    '57': null,
    '58': null,
    '59': null,
    '60': null,
    '61': null,
    '62': null,
    '63': '789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789',
    '64': null
};

let MTI = '0200';

let packed = iso8583Parser.pack(MTI, dataElements);

//This returns the packed ISO8583 message
console.log(packed.isoMessage);
```

The __packed__ object here also contains the MTI, bitmap in binary and hexadecimal form, the data element part of the message and whether the bitmap is secondary or not as shown below

```javascript
console.log(packed);
```

```json
{
  "error": false,
  "binaryBitmap": "0100001000000000000001000000000000000000000000000000000000000010",
  "hexadecimalBitmap": "4200040000000002",
  "secondaryBitmap": false,
  "dataElementPart": "1612345678901234560609173030123109789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789",
  "mti": "0200",
  "isoMessage": "020042000400000000021612345678901234560609173030123109789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789"
}
```

### cISO8583.unpack()

This method allows you to unpack an ISO8583 message. It returns the data elements as a json object in the format dataElementNumber: fieldData, the bitmap in binary form, the data element part of the message and if it has a secondary bitmap type or not.

```javascript
const cISO8583 = require('ciso8583');

let iso8583Parser = new cISO8583();

let iso8583Message = "020042000400000000021612345678901234560609173030123109789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789";

let unpacked = iso8583Parser.unpack(iso8583Message);

//This returns the data elements
console.log(unpacked.dataElements);
```

The __unpacked__ object here also contains the bitmap in binary form, the data element part of the message and whether the bitmap is secondary or not as shown below

```javascript
console.log(unpacked);
```

```json
{
  "error": false,
  "dataElementPart": "1612345678901234560609173030123109789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789",
  "binaryBitmap": "0100001000000000000001000000000000000000000000000000000000000010",
  "secondaryBitmap": false,
  "dataElements": {
    "2": "1234567890123456",
    "3": null,
    "4": null,
    "5": null,
    "6": null,
    "7": "0609173030",
    "8": null,
    "9": null,
    "10": null,
    "11": null,
    "12": null,
    "13": null,
    "14": null,
    "15": null,
    "16": null,
    "17": null,
    "18": null,
    "19": null,
    "20": null,
    "21": null,
    "22": "123",
    "23": null,
    "24": null,
    "25": null,
    "26": null,
    "27": null,
    "28": null,
    "29": null,
    "30": null,
    "31": null,
    "32": null,
    "33": null,
    "34": null,
    "35": null,
    "36": null,
    "37": null,
    "38": null,
    "39": null,
    "40": null,
    "41": null,
    "42": null,
    "43": null,
    "44": null,
    "45": null,
    "46": null,
    "47": null,
    "48": null,
    "49": null,
    "50": null,
    "51": null,
    "52": null,
    "53": null,
    "54": null,
    "55": null,
    "56": null,
    "57": null,
    "58": null,
    "59": null,
    "60": null,
    "61": null,
    "62": null,
    "63": "789ABC1000123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789",
    "64": null
  },
  "mti": "0200"
}
```

### Optional Configuration
This library allows easy customization of the data elements types, length and more. When optional configuration is specified, it overrides the default configuration of the element it was provided for. The optional configuration usually takes the format of the configuration _(see configuration format and description section below)_ of each data element.

Optional configuration is specified as shown below:

```javascript
const cISO8583 = require('ciso8583');

const optionalConfig = {
    "52": {
        "fixedLength": true,
        "contentLength": 16,
        "minLength": 0,
        "maxLength": 0,
        "contentType": "ans",
        "slug": null,
        "nestedElements": {}
    }
};

//Pass this config as a parameter into the constructor when initializing the isoParser

let iso8583ParserWithConfig = new cISO8583(optionalConfig);
```

The __pack()__ and __unpack()__ methods would now use the configuration specified for the data element above


## Configuration Format

The configuration of the data elements is written in JSON. It is of the format

```json
{
    "2": {
        "fixedLength": false,
        "contentLength": 2,
        "minLength": 0,
        "maxLength": 19,
        "contentType": "n",
        "slug": null,
        "nestedElements": {}
    },
    "3": {
        "fixedLength": true,
        "contentLength": 6,
        "minLength": 0,
        "maxLength": 0,
        "contentType": "n",
        "slug": null,
        "nestedElements": {}
    }
}
```

### Configuration Description

From the format explained above these are the description of the various fields found in the configuration

1. __fixedLength__ (boolean): Denotes if the length of the element is fixed or not
2. __contentLength__ (integer): For fixed length types this tells us the length of the element. Otherwise the number of length characters present in the data. This means:
  Fixed Length Types: the length would be the value.
  Variable Length Types: The number of L's would be the value here, so for LLLVAR this would be 3 and for LLVAR this would be 2
3. __minLength__ (integer): Denotes the minimum length for variable length types
4. __maxLength__ (integer): Denotes the maximum length for variable length types
5. __contentType__ (string): Denotes the content type of the field, the possible values and their description are below
  * n: Numeric values only
  * an: Alpha numeric
  * ans: Alpha numeric and special characters
  x+n: Numeric (amount) values, where the first byte is either 'C' to indicate a positive or Credit value, or 'D' to indicate a negative or Debit value, followed by the numeric value (using n digits)
  * s: Special characters only
  * as: Alpha and special characters
  * ns: Numeric and special characters
  * b: Binary data
  * z: Tracks 2 and 3 code set as defined in ISO/IEC 7813 and ISO/IEC 4909 respectively
6. __slug__ (string): Optional name for element _TODO: add slug feature_
7. __nestedElements__ (object): Nested elements inside data element _TODO: add nested element feature_

Once de message is packed and saved in a file with the .ipm extension you can check the correct format in the file parser, it should return a similar message like the screenshot that has been provided.
### SOURCES FOR DOCUMENTATION

https://mciutil.readthedocs.io/en/stable/usage.html
https://dokumen.tips/documents/ipm-format-quick-reference.html
Blocked File Documentation (from mastercard)
IPM CLEARING FORMATS 2020 (chapters 1 and 2 )