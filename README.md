[![PyPI version](https://badge.fury.io/py/mail-parser.svg)](https://badge.fury.io/py/mail-parser)
[![Build Status](https://travis-ci.org/SpamScope/mail-parser.svg?branch=develop)](https://travis-ci.org/SpamScope/mail-parser)
[![Coverage Status](https://coveralls.io/repos/github/SpamScope/mail-parser/badge.svg?branch=develop)](https://coveralls.io/github/SpamScope/mail-parser?branch=develop)

# mail-parser

## Overview

mail-parser is a wrapper for [email](https://docs.python.org/2/library/email.message.html) Python Standard Library. It's the key module of [SpamScope](https://github.com/SpamScope/spamscope).

From version 1.0.0rc1 mail-parser supports Python 3.

## Description

mail-parser takes as input a raw mail and generates a parsed object. This object is a tokenized mail with the all parts of mail and some indicator:
  - body
  - headers
  - subject
  - from
  - to
  - attachments
  - message id
  - date
  - charset mail
  - sender IP address

We have also two indicator:
  - anomalies: mail without message id or date
  - [defects](https://docs.python.org/2/library/email.message.html#email.message.Message.defects): mail with some not compliance RFC part

### Defects
These defects can be used to evade the antispam filter. An example are the mails with a malformed boundary that can hide a not legitimate epilogue (often malware).
This library can take these epilogues.


### Apache 2 Open Source License
mail-parser can be downloaded, used, and modified free of charge. It is available under the Apache 2 license.
[![Donate](https://www.paypal.com/en_US/i/btn/btn_donateCC_LG.gif "Donate")](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VEPXYP745KJF2)


## Authors

### Main Author
Fedele Mantuano (**Twitter**: [@fedelemantuano](https://twitter.com/fedelemantuano))


## Installation

Clone repository

```
git clone https://github.com/SpamScope/mail-parser.git
```

and install mail-parser with `setup.py`:

```
cd mail-parser

python setup.py install
```

or use `pip`:

```
pip install mail-parser
```

## Usage in a project

Import `MailParser` class:

```
from mailparser import MailParser

parser = MailParser()
parser.parse_from_file(f)
parser.parse_from_string(raw_mail)
```

Then you can get all parts

```
parser.body
parser.headers
parser.message_id
parser.to_
parser.from_
parser.subject
parser.text_plain_list: only text plain mail parts in a list
parser.attachments_list: list of all attachments
parser.date_mail
parser.parsed_mail_obj: tokenized mail in a object
parser.parsed_mail_json: tokenized mail in a JSON
parser.defects: defect RFC not compliance
parser.defects_category: only defects categories
parser.has_defects
parser.anomalies
parser.has_anomalies
parser.get_server_ipaddress(trust="my_server_mail_trust")
```

## Usage from command-line

If you installed mailparser with `pip` or `setup.py` you can use it with command-line.

These are all swithes:

```
usage: mailparser.py [-h] (-f FILE | -s STRING) [-j] [-b] [-a] [-r] [-t] [-m]
                   [-u] [-d] [-n] [-i Trust mail server string] [-p] [-z] [-v]

Wrapper for email Python Standard Library

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  Raw email file (default: None)
  -s STRING, --string STRING
                        Raw email string (default: None)
  -j, --json            Show the JSON of parsed mail (default: False)
  -b, --body            Print the body of mail (default: False)
  -a, --attachments     Print the attachments of mail (default: False)
  -r, --headers         Print the headers of mail (default: False)
  -t, --to              Print the to of mail (default: False)
  -m, --from            Print the from of mail (default: False)
  -u, --subject         Print the subject of mail (default: False)
  -d, --defects         Print the defects of mail (default: False)
  -n, --anomalies       Print the anomalies of mail (default: False)
  -i Trust mail server string, --senderip Trust mail server string
                        Extract a reliable sender IP address heuristically
                        (default: None)
  -p, --mail-hash       Print mail fingerprints without headers (default:
                        False)
  -z, --attachments-hash
                        Print attachments with fingerprints (default: False)
  -v, --version         show program's version number and exit

It takes as input a raw mail and generates a parsed object.
```

Example:

```shell
$ mailparser -f example_mail -j
```

This example will show you the tokenized mail in a JSON pretty format.
