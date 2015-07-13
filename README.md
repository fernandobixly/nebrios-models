# nebrios-models
"Models" using NebriOS Processes

This app is intended for use in a NebriOS instance. Visit https://nebrios.com to sign up for free!

## Setup
Please ensure that all files are placed in the correct locations over SFTP.
* `libraries/nebriosmodels.py` should be copied to /libraries

## Usage

### Model definition

```
from passlib.hash import pbkdf2_sha512

from nebriosmodels import NebriOSModel, NebriOSField, NebriOSReference


class MyUser(NebriOSModel):

    username = NebriOSField(required=True)
    password = NebriOSField(required=True)

    def set_password(self, password):
        self.password = pbkdf2_sha512.encrypt(password)

    def validate_password(self, password):
        return pbkdf2_sha512.verify(password, self.password)


class MyCar(NebriOSModel):

    user = NebriOSReference(MyUser, required=True)
    license_plate = NebriOSField(required=True)
```

### Model usage

Regular model usage
```
>>> user = MyUser(username="testuser")
>>> user.set_password("test_password")
>>> user.save()
>>> user.validate_password("wrong_password")
False
>>> user.validate_password("test_password")
True
```

Referencing other model instances
```
>>> car = MyCar(user=user, license_plate="ABC123")
>>> car.user
<MyUser id 342>
>>> car.user = None
>>> car.save()  # This raises an exception, because the field user is required
>>> car.user = user
>>> car.save()
```

Retrieving model instances
```
try:
    user = User.get(username="testuser")
    # we can manipulate the object here
    cars = MyCar.filter(user=user)
    for car in cars:
        # We have a list of cars here
except:
    # exception thrown if model not found
```