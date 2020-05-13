## Laboratory work IX

<a href="https://yandex.ru/efir/?stream_id=vYrKRcFKi46o"><img src="https://raw.githubusercontent.com/tp-labs/lab09/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению процесса создания артефактов на примере **Github Release**

```sh
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
Cloning into 'lab09'...
remote: Enumerating objects: 98, done.
remote: Counting objects: 100% (98/98), done.
remote: Compressing objects: 100% (65/65), done.
Receiving objects:  39% (39/ reused 98 (delta 32), pack-reused 0Receiving objects:  38% (38/98), 1.16 MiB | 1.13 MiB/s
Receiving objects: 100% (98/98), 1.17 MiB | 1.14 MiB/s, done.
Resolving deltas: 100% (32/32), done.
$ export GITHUB_TOKEN=0dc94d9c9fe7556bc832fbc82e5b22c21b58328c 
$ export GITHUB_USERNAME=scorpy2013
$ export PACKAGE_MANAGER=<пакетный менеджер>
$ export GPG_PACKAGE_NAME=gpg2
```

```sh
# for *-nix system
$ $PACKAGE_MANAGER install xclip
$ alias gsed=sed
$ alias pbcopy='xclip -selection clipboard'
$ alias pbpaste='xclip -selection clipboard -o'
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/Documents/GitHub/scorpy2013/lab09 ~/Documents/GitHub/scorpy2013/lab09
$ source scripts/activate
$ go get github.com/aktau/github-release
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
Cloning into 'projects/lab09'...
remote: Enumerating objects: 76, done.
remote: Counting objects: 100% (76/76), done.
remote: Compressing objects: 100% (52/52), done.
remote: Total 76 (delta 24), reused 72 (delta 23), pack-reused 0
Receiving objects: 100% (76/76), 950.15 KiB | 1.68 MiB/s, done.
Resolving deltas: 100% (24/24), done.
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```

```sh
$ gsed -i 's/lab08/lab09/g' README.md
```

```sh
$ $PACKAGE_MANAGER install ${GPG_PACKAGE_NAME}
$ gpg --list-secret-keys --keyid-format LONG
gpg: directory '/c/Users/User/.gnupg' created
gpg: keybox '/c/Users/User/.gnupg/pubring.kbx' created
gpg: /c/Users/User/.gnupg/trustdb.gpg: trustdb created
$ gpg --full-generate-key
gpg (GnuPG) 2.2.20-unknown; Copyright (C) 2020 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection?
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
Key does not expire at all
Is this correct? (y/N)
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: scorpy2013
Email address: scorpy2013@gmail.com
Comment: Alexander
You selected this USER-ID:
    "scorpy2013 (Alexander) <scorpy2013@gmail.com>"
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: agent_genkey failed: Operation cancelled
Key generation failed: Operation cancelled
$ gpg --list-secret-keys --keyid-format LONG
$ gpg -K ${GITHUB_USERNAME}
$ GPG_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep ssb | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}')
$ GPG_SEC_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep sec | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}')
$ gpg --armor --export ${GPG_KEY_ID} | pbcopy
$ pbpaste
$ open https://github.com/settings/keys
$ git config user.signingkey ${GPG_SEC_KEY_ID}
$ git config gpg.program gpg
```

```sh
$ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
$ echo 'export GPG_TTY=$(tty)' >> ~/.profile
```

```sh
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```

```sh
$ travis login --auto
$ travis enable
```

```sh
$ git tag -s v0.1.0.0
#
# Write a message for tag:
#   v0.1.0.0
# Lines starting with '#' will be ignored.
$ git tag -v v0.1.0.0
$ git show v0.1.0.0
fatal: ambiguous argument 'v0.1.0.0': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
$ git push origin master --tags
Everything up-to-date
```

```sh
$ github-release --version
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ github-release release \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```

```sh
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m` 
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz
$ github-release upload \
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```

```sh
$ github-release info -u ${GITHUB_USERNAME} -r lab09
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ tar -ztf ${PACKAGE_FILENAME}
```

## Report

```sh
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
Cloning into 'tasks/lab09'...
remote: Enumerating objects: 30, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 98 (delta 9), reused 14 (delta 4), pack-reused 68
Receiving objects: 100% (98/98), 1.18 MiB | 778.00 KiB/s, done.
Resolving deltas: 100% (31/31), done.
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2015-2020 The ISC Authors
```
