# oath-toolkit-wrapper(a.k.a. `mfa`)
## Overview
This is a wrapper command for oath-toolkit. This command allows you to get TOTP from CLI w/o any hassle.

## Usage
```sh
$ mfa -h
usage: mfa [-h | --help] [-[no]-c | --[no]-copy] [-a <account>| --account <account>] [-l | --list]
  -v, --version                      Prints the version.
  -h, --help                         Prints this message.
  -[no]-c, --[no]-copy               Copies the generated token to the Clipboard.(default)
  -a <account>, --account <account>  Copies the generated token of <account> to the Clipboard.
  -l, --list                         Prints a list of available authenticator accounts.
```

## Dependencies
To use `mfa` command, first, the dependencies as follows should be installed:

- [jq](https://github.com/stedolan/jq)
- [oath-toolkit](https://gitlab.com/oath-toolkit/oath-toolkit/tree/master)
- [OpenSSL](https://www.openssl.org/)
- [peco](https://github.com/peco/peco)

If the dependencies have not been installed, run the following:

```sh
$ brew install jq oath-toolkit openssl@1.1 peco
```

## Installation
Clone this repository:

```sh
$ git clone git@github.com:Sean0628/oath-toolkit-wrapper.git ~/.oath-toolkit-wrapper
```

To use `mfa` command, edit the `$PATH` to include paths to the wrapper command.
zsh:
```sh
$ echo 'export PATH="$HOME/.oath-toolkit-wrapper/bin:$PATH" >> ~/.zshrc
```

bash:
```sh
$ echo 'export PATH="$HOME/.oath-toolkit-wrapper/bin:$PATH" >> ~/.bash_profile
```

## Configuration
### Secret key
Follow the step by step instructions on the link below to extract your secret keys:
- [Generating Authy passwords on other authenticators](https://gist.github.com/gboudreau/94bb0c11a6209c82418d01a59d958c93#ok-thats-nice-but-i-want-to-get-rid-of-authy-now)

#### Create secret file
1. Copy and paste your secret keys, and generate your own `secrets.json` file. cf. [Sample format](https://github.com/Sean0628/oath-toolkit-wrapper/blob/main/tmp/secrets.sample.json)

2. Encrypt `secrets.json` with OpenSSL for security purposes. Run the following:
```sh
$ openssl enc -aes-256-cbc -a -salt -in secrets.json -out secrets.json.enc
```

You will be asked for the password. This password will be used when decrypt the secret file also.

3. Place encrypted `secrets.json` into `config/` directory:
```sh
$ mv secrets.json.enc /path/to/oath-toolkit-wrapper/config
```

4. Remove redundant `secrets.json` file:
```sh
$ rm secrets.json
```

### [Optional]Password
It is possible to omit entering password each time you use `mfa` command by creating `config/credentials.json`.

#### Create credential file
1. Generate your own `credentials.json` file. cf. [Sample format](https://github.com/Sean0628/oath-toolkit-wrapper/blob/main/tmp/credentials.sample.json) 

This password should be the same password which is used to encrypt `secrets.json` file.

2. Place `credentials.json` into `config/` directory:
```sh
$ mv credentials.json /path/to/oath-toolkit-wrapper/config
```

3. Set permission and change the owner for security purposes:
```sh
$ chmod 400 config/credentials.json
$ sudo chown root config/credentials.json
```
