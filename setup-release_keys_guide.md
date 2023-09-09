# How to setup release keys
Full guide here: [sbt-ci-release](https://github.com/olafurpg/sbt-ci-release)

### 1. Gen Key Pair
```shell
gpg --gen-key
```
**Data**
- Real name: `$PRJ_NAME-release-bot`
- Email: use your own email address
- For passphrase, generate a random password with a password manager

**Result**
```
pub   rsa2048 2018-06-10 [SC] [expires: 2020-06-09]
      $LONG_ID
uid                      $PRJ_NAME-release-bot bot <$EMAIL>
```

### 2. Set PRJ_NAME and LONG_ID

**Example**

#### UNIX
```shell
PRJ_NAME=example-release-bot
LONG_ID=6E8ED79B03AD527F1B281169D28FC818985732D9
```

#### Windows
```shell
set PRJ_NAME=example-release-bot
set LONG_ID=6E8ED79B03AD527F1B281169D28FC818985732D9
```

### 3. Export public key

#### macOS
**Clipboard**
```shell
gpg --armor --export $LONG_ID | pbcopy
```

#### linux
```shell
gpg --armor --export $LONG_ID | xclip
```

#### Windows
```shell
gpg --armor --export %LONG_ID%
```

**File**

#### macOS
```shell
gpg --armor --export $LONG_ID > $PRJ_NAME-release-bot-public.gpg
```

#### linux
```shell
gpg --armor --export $LONG_ID > $PRJ_NAME-release-bot-public.gpg
```

#### Windows
```shell
gpg --armor --export %LONG_ID% > %PRJ_NAME%-release-bot-public.gpg
```
### 4. Public the public key to keyserver
Copy the **PUBLIC KEY** and publish it in a public keyserver

Like this one:
[https://keyserver.ubuntu.com/](https://keyserver.ubuntu.com/)

### 5. Export private key in base64
**Clipboard**

#### macOS
```shell
gpg --armor --export-secret-keys $LONG_ID | base64 | pbcopy
```

#### Ubuntu (assuming GNU base64)
```shell
gpg --armor --export-secret-keys $LONG_ID | base64 -w0 | xclip
```

#### Windows
```shell
gpg --armor --export-secret-keys %LONG_ID% | openssl base64
```

**File**

#### macOS
```shell
gpg --armor --export-secret-keys $LONG_ID | base64 > $PRJ_NAME-release-bot-private.gpg
```

#### Ubuntu (assuming GNU base64)
```shell
gpg --armor --export-secret-keys $LONG_ID | base64 -w0 > $PRJ_NAME-release-bot-private.gpg
```

#### Windows
```shell
gpg --armor --export-secret-keys %LONG_ID% | openssl base64 > %PRJ_NAME%-release-bot-private.gpg
```

### 6. Put the private key in Github secrets
- Copy the **PRIVATE KEY** in _Base64_
- Create a secret in github named `PGP_SECRET` and store the base64 *PRIVATE KEY*
- Create a secret in github named `PGP_PASSPHRASE` and store the base64 *PRIVATE KEY* passphrase
