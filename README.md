# nebrios-models
"Models" using NebriOS Processes

This app is intended for use in a NebriOS instance. Visit https://nebrios.com to sign up for free!

## Setup
Please ensure that all files are placed in the correct locations over SFTP.
* `nebriosmodels.py` should be copied to /libraries

## Usage

### Model definition

```
from passlib.hash import pbkdf2_sha512

from nebriosmodels import NebriOSModel, NebriOSField, NebriOSReference


class MyUser(NebriOSModel):

    kind = 'myuser'

    username = NebriOSField(required=True)
    password = NebriOSField(required=True)

    def set_password(self, password):
        self.password = pbkdf2_sha512.encrypt(password)

    def validate_password(self, password):
        return pbkdf2_sha512.verify(password, self.password)


class MyCar(NebriOSModel):

    kind = 'car'
    user = NebriOSReference(MyUser, required=True)
```