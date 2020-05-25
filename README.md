# connextconfig

512Mb RAM
1 vCPU
10Gb Disk

Expose ports 22, 80, 443, 4221, 4222


```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo nano /etc/fstab
```
Add this to `/etc/fstab`
```
/swapfile swap swap defaults 0 0
```
Then continue
```
sudo apt update
sudo apt upgrade -y

sudo apt install docker.io -y
sudo usermod -aG docker ubuntu
sudo systemctl enable docker
sudo reboot
```
reconnect
```
sudo apt install make -y
sudo apt install jq -y

rm -rf indra
git clone https://github.com/connext/indra.git
cd indra
INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" INDRA_UI="headless" make start
```

Debugging:
```
bash ops/logs.sh node
bash ops/logs.sh webserver
make dls
```

```
ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
=============
[Makefile] => Start building builder
docker build --file ops/builder/Dockerfile --build-arg SOLC_VERSION=0.5.11   --tag indra_builder ops/builder
ERRO[0000] failed to dial gRPC: cannot connect to the Docker daemon. Is 'docker daemon' running on this host?: dial unix /var/run/docker.sock: connect: permission denied 
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/build?buildargs=%7B%22SOLC_VERSION%22%3A%220.5.11%22%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=dv50omjy4bqqv53541o15r41b&shmsize=0&t=indra_builder&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
make: *** [Makefile:216: builder] Error 1
ubuntu@nathan:~/indra$ sudo reboot
Connection to 89.145.161.66 closed by remote host.
Connection to 89.145.161.66 closed.
ubuntu@cibulka:~$ ssh -i .ssh/exoscale-rowans ubuntu@89.145.161.66
Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-26-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 System information disabled due to load higher than 2.0


0 updates can be installed immediately.
0 of these updates are security updates.


Last login: Mon May 25 08:44:34 2020 from 103.79.161.57
ubuntu@nathan:~$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
make: *** No rule to make target 'start'.  Stop.
ubuntu@nathan:~$ cd indra/
ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
=============
[Makefile] => Start building builder
docker build --file ops/builder/Dockerfile --build-arg SOLC_VERSION=0.5.11   --tag indra_builder ops/builder
Sending build context to Docker daemon  3.584kB
Step 1/15 : ARG SOLC_VERSION
Step 2/15 : FROM ethereum/solc:$SOLC_VERSION-alpine as solc
0.5.11-alpine: Pulling from ethereum/solc
050382585609: Pull complete 
34fcb7784a32: Pull complete 
Digest: sha256:4ee8a106e78a6c479be43d99e19e09c62e1b324811fb28634fc0dafc30eb2be0
Status: Downloaded newer image for ethereum/solc:0.5.11-alpine
 ---> a7b1f30b2e6c
Step 3/15 : FROM node:12.13.0-alpine3.10
12.13.0-alpine3.10: Pulling from library/node
89d9c30c1d48: Pull complete 
cb4880ccba47: Pull complete 
abc31ffc07f9: Pull complete 
2137f333b9e3: Pull complete 
Digest: sha256:ae1822c17b0087cb1eea794e5a293d56cc1fe01f01ef5494d0687c1ef9584239
Status: Downloaded newer image for node:12.13.0-alpine3.10
 ---> 69c8cc9212ec
Step 4/15 : WORKDIR /root
 ---> Running in 30c55dcdea52
Removing intermediate container 30c55dcdea52
 ---> 6cf4314c099f
Step 5/15 : ENV HOME /root
 ---> Running in 3614188782fe
Removing intermediate container 3614188782fe
 ---> 66057a925005
Step 6/15 : RUN apk add --update --no-cache bash curl g++ gcc git jq make python python3
 ---> Running in 116bdb7d187e
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
(1/32) Installing ncurses-terminfo-base (6.1_p20190518-r2)
(2/32) Installing ncurses-libs (6.1_p20190518-r2)
(3/32) Installing readline (8.0.0-r0)
(4/32) Installing bash (5.0.0-r0)
Executing bash-5.0.0-r0.post-install
(5/32) Installing ca-certificates (20191127-r0)
(6/32) Installing nghttp2-libs (1.39.2-r0)
(7/32) Installing libcurl (7.66.0-r0)
(8/32) Installing curl (7.66.0-r0)
(9/32) Installing binutils (2.32-r0)
(10/32) Installing gmp (6.1.2-r1)
(11/32) Installing isl (0.18-r0)
(12/32) Installing libgomp (8.3.0-r0)
(13/32) Installing libatomic (8.3.0-r0)
(14/32) Installing mpfr3 (3.1.5-r1)
(15/32) Installing mpc1 (1.1.0-r0)
(16/32) Installing gcc (8.3.0-r0)
(17/32) Installing musl-dev (1.1.22-r3)
(18/32) Installing libc-dev (0.7.1-r0)
(19/32) Installing g++ (8.3.0-r0)
(20/32) Installing expat (2.2.8-r0)
(21/32) Installing pcre2 (10.33-r0)
(22/32) Installing git (2.22.4-r0)
(23/32) Installing oniguruma (6.9.4-r0)
(24/32) Installing jq (1.6-r0)
(25/32) Installing make (4.2.1-r2)
(26/32) Installing libbz2 (1.0.6-r7)
(27/32) Installing libffi (3.2.1-r6)
(28/32) Installing gdbm (1.13-r1)
(29/32) Installing sqlite-libs (3.28.0-r3)
(30/32) Installing python2 (2.7.18-r0)
(31/32) Installing xz-libs (5.2.4-r0)
(32/32) Installing python3 (3.7.5-r1)
Executing busybox-1.30.1-r2.trigger
Executing ca-certificates-20191127-r0.trigger
OK: 271 MiB in 48 packages
Removing intermediate container 116bdb7d187e
 ---> 96e4c201519f
Step 7/15 : RUN npm config set unsafe-perm true
 ---> Running in 8ec18cbf6e82
Removing intermediate container 8ec18cbf6e82
 ---> 3660cd89fb6d
Step 8/15 : RUN npm install -g npm@6.14.4
 ---> Running in 635f666191df
/usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
/usr/local/bin/npx -> /usr/local/lib/node_modules/npm/bin/npx-cli.js
+ npm@6.14.4
added 12 packages from 6 contributors, removed 7 packages and updated 46 packages in 10.897s
Removing intermediate container 635f666191df
 ---> 59c95ee5e5c2
Step 9/15 : RUN npm install -g lerna@3.20.2
 ---> Running in eb79b542a658
npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated mkdirp-promise@5.0.1: This package is broken and no longer maintained. 'mkdirp' itself supports promises now, please switch to that.
/usr/local/bin/lerna -> /usr/local/lib/node_modules/lerna/cli.js
+ lerna@3.20.2
added 754 packages from 389 contributors in 28.032s
Removing intermediate container eb79b542a658
 ---> 7f9b2f482713
Step 10/15 : RUN python3 -m pip install --upgrade --no-cache-dir pip virtualenv
 ---> Running in ac68d3c0d632
Collecting pip
  Downloading https://files.pythonhosted.org/packages/43/84/23ed6a1796480a6f1a2d38f2802901d078266bda38388954d01d3f2e821d/pip-20.1.1-py2.py3-none-any.whl (1.5MB)
Collecting virtualenv
  Downloading https://files.pythonhosted.org/packages/57/6e/a13442adf18bada682f88f55638cd43cc7a39c3e00fdcf898ca4ceaeb682/virtualenv-20.0.21-py2.py3-none-any.whl (4.7MB)
Collecting distlib<1,>=0.3.0 (from virtualenv)
  Downloading https://files.pythonhosted.org/packages/7d/29/694a3a4d7c0e1aef76092e9167fbe372e0f7da055f5dcf4e1313ec21d96a/distlib-0.3.0.zip (571kB)
Collecting appdirs<2,>=1.4.3 (from virtualenv)
  Downloading https://files.pythonhosted.org/packages/3b/00/2344469e2084fb287c2e0b57b72910309874c3245463acd6cf5e3db69324/appdirs-1.4.4-py2.py3-none-any.whl
Collecting six<2,>=1.9.0 (from virtualenv)
  Downloading https://files.pythonhosted.org/packages/ee/ff/48bde5c0f013094d729fe4b0316ba2a24774b3ff1c52d924a8a4cb04078a/six-1.15.0-py2.py3-none-any.whl
Collecting importlib-metadata<2,>=0.12; python_version < "3.8" (from virtualenv)
  Downloading https://files.pythonhosted.org/packages/ad/e4/891bfcaf868ccabc619942f27940c77a8a4b45fd8367098955bb7e152fb1/importlib_metadata-1.6.0-py2.py3-none-any.whl
Collecting filelock<4,>=3.0.0 (from virtualenv)
  Downloading https://files.pythonhosted.org/packages/93/83/71a2ee6158bb9f39a90c0dea1637f81d5eef866e188e1971a1b1ab01a35a/filelock-3.0.12-py3-none-any.whl
Collecting zipp>=0.5 (from importlib-metadata<2,>=0.12; python_version < "3.8"->virtualenv)
  Downloading https://files.pythonhosted.org/packages/b2/34/bfcb43cc0ba81f527bc4f40ef41ba2ff4080e047acb0586b56b3d017ace4/zipp-3.1.0-py3-none-any.whl
Installing collected packages: pip, distlib, appdirs, six, zipp, importlib-metadata, filelock, virtualenv
  Found existing installation: pip 19.2.3
    Uninstalling pip-19.2.3:
      Successfully uninstalled pip-19.2.3
  Running setup.py install for distlib: started
    Running setup.py install for distlib: finished with status 'done'
Successfully installed appdirs-1.4.4 distlib-0.3.0 filelock-3.0.12 importlib-metadata-1.6.0 pip-20.1.1 six-1.15.0 virtualenv-20.0.21 zipp-3.1.0
Removing intermediate container ac68d3c0d632
 ---> 53d3ba8f32ce
Step 11/15 : COPY --from=solc /usr/local/bin/solc /usr/local/bin/solc
 ---> 8347673c026d
Step 12/15 : RUN curl https://raw.githubusercontent.com/vishnubob/wait-for-it/ed77b63706ea721766a62ff22d3a251d8b4a6a30/wait-for-it.sh > /bin/wait-for && chmod +x /bin/wait-for
 ---> Running in 253fcaa07309
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5224  100  5224    0     0  16743      0 --:--:-- --:--:-- --:--:-- 16743
Removing intermediate container 253fcaa07309
 ---> d8a207698219
Step 13/15 : COPY entry.sh /entry.sh
 ---> dff5b9fbd883
Step 14/15 : ENV PATH="./node_modules/.bin:${PATH}"
 ---> Running in 2046d8e3e97c
Removing intermediate container 2046d8e3e97c
 ---> b0521c160445
Step 15/15 : ENTRYPOINT ["bash", "/entry.sh"]
 ---> Running in aeedc294fe74
Removing intermediate container aeedc294fe74
 ---> abd5bc24bc9c
Successfully built abd5bc24bc9c
Successfully tagged indra_builder:latest
[Makefile] => Finished building builder in 66 seconds
=============

=============
[Makefile] => Start building node-modules
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "lerna bootstrap --hoist --no-progress"
Running command as 0:0 (target user: 1000:1000)
lerna notice cli v3.20.2
lerna info Bootstrapping 15 packages
lerna WARN EHOIST_PKG_VERSION "bots" package depends on ts-node@8.10.1, which differs from the hoisted ts-node@8.9.0.
lerna WARN EHOIST_PKG_VERSION "@connext/cf-core" package depends on typescript-memoize@1.0.0-alpha.3, which differs from the hoisted typescript-memoize@^1.0.0-alpha.3.
lerna info Installing external dependencies
lerna info hoist Installing hoisted dependencies into root
lerna info hoist Pruning hoisted dependencies
lerna info hoist Finished pruning hoisted dependencies
lerna info hoist Finished bootstrapping root
lerna info Symlinking packages and binaries
lerna success Bootstrapped 15 packages
Fixing permissions for 1000:1000
[Makefile] => Finished building node-modules in 218 seconds
=============

=============
[Makefile] => Start building types
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/types && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/types@6.6.1 build /root/modules/types
> rm -rf ./dist/* && ./node_modules/.bin/tsc -b . && ./node_modules/.bin/rollup -c


./src/index.ts → dist/index.js, dist/index.esm.js, dist/index-iife.js...
(!) Unresolved dependencies
https://rollupjs.org/guide/en/#warning-treating-module-as-external-dependency
ethers/utils (imported by src/basic.ts)
ethers (imported by src/basic.ts)
ethers/providers (imported by src/basic.ts)
ethers/constants (imported by src/constants.ts)
eventemitter3 (imported by src/events.ts)
(!) Missing global variable names
Use output.globals to specify browser global variable names corresponding to external modules
ethers/utils (guessing 'utils')
ethers (guessing 'ethers')
ethers/providers (guessing 'providers')
ethers/constants (guessing 'constants')
eventemitter3 (guessing 'EventEmitter')
created dist/index.js, dist/index.esm.js, dist/index-iife.js in 5.2s
Fixing permissions for 1000:1000
[Makefile] => Finished building types in 13 seconds
=============

=============
[Makefile] => Start building utils
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/utils && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/utils@6.6.1 build /root/modules/utils
> rm -rf ./dist/* && ./node_modules/.bin/tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building utils in 5 seconds
=============

=============
[Makefile] => Start building channel-provider
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/channel-provider && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/channel-provider@6.6.1 build /root/modules/channel-provider
> rm -rf ./dist/* && ./node_modules/.bin/tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building channel-provider in 5 seconds
=============

=============
[Makefile] => Start building messaging
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/messaging && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/messaging@6.6.1 build /root/modules/messaging
> rm -rf ./dist/* && tsc -p . && rollup -c


src/index.ts → dist/index.js...
created dist/index.js in 2s
Fixing permissions for 1000:1000
[Makefile] => Finished building messaging in 6 seconds
=============

=============
[Makefile] => Start building contracts
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/contracts && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/contracts@3.1.0 build /root/modules/contracts
> rm -rf ./dist/* && npm run compile && npm run transpile


> @connext/contracts@3.1.0 compile /root/modules/contracts
> npx buidler compile

Compiling...
Downloading compiler version 0.6.7


contracts/funding/state-deposit-holders/MultisigTransfer.sol:32:13: Warning: Failure condition of 'send' ignored. Consider using 'transfer' instead.
            recipient.send(amount);
            ^--------------------^



contracts/funding/test-fixtures/DelegateProxy.sol:20:13: Warning: Failure condition of 'send' ignored. Consider using 'transfer' instead.
            recipient.send(amount);
            ^--------------------^

Compiled 54 contracts successfully

> @connext/contracts@3.1.0 transpile /root/modules/contracts
> tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building contracts in 25 seconds
=============

=============
[Makefile] => Start building store
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/store && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/store@6.6.1 build /root/modules/store
> rm -rf ./dist/* && ./node_modules/.bin/tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building store in 6 seconds
=============

=============
[Makefile] => Start building cf-core
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/cf-core && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/cf-core@6.6.1 build /root/modules/cf-core
> rm -rf ./dist/* && ./node_modules/.bin/tsc -b .

Fixing permissions for 1000:1000
[Makefile] => Finished building cf-core in 10 seconds
=============

=============
[Makefile] => Start building apps
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/apps && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/apps@6.6.1 build /root/modules/apps
> tsc -b . && ./node_modules/.bin/rollup -c


./src/index.ts → dist/index.js, dist/index.esm.js, dist/index-iife.js...
(!) Unresolved dependencies
https://rollupjs.org/guide/en/#warning-treating-module-as-external-dependency
@connext/types (imported by src/middleware.ts, src/DepositApp/registry.ts, src/DepositApp/validation.ts, src/SimpleLinkedTransferApp/registry.ts, src/HashLockTransferApp/registry.ts, src/SimpleSignedTransferApp/registry.ts, src/SimpleTwoPartySwapApp/registry.ts, src/WithdrawApp/registry.ts, src/WithdrawApp/withdrawCommitment.ts, src/shared/registry.ts, src/shared/validation.ts)
@connext/utils (imported by src/DepositApp/validation.ts, src/SimpleLinkedTransferApp/validation.ts, src/HashLockTransferApp/validation.ts, src/SimpleSignedTransferApp/validation.ts, src/SimpleTwoPartySwapApp/validation.ts, src/WithdrawApp/validation.ts, src/shared/registry.ts, src/shared/validation.ts)
@connext/contracts (imported by src/DepositApp/validation.ts, src/WithdrawApp/withdrawCommitment.ts)
ethers (imported by src/DepositApp/validation.ts)
ethers/constants (imported by src/DepositApp/validation.ts, src/SimpleLinkedTransferApp/registry.ts, src/HashLockTransferApp/registry.ts, src/SimpleSignedTransferApp/registry.ts, src/SimpleTwoPartySwapApp/registry.ts, src/WithdrawApp/registry.ts, src/WithdrawApp/validation.ts, src/shared/validation.ts)
ethers/utils (imported by src/SimpleTwoPartySwapApp/validation.ts, src/WithdrawApp/withdrawCommitment.ts)
(!) Missing global variable names
Use output.globals to specify browser global variable names corresponding to external modules
@connext/types (guessing 'types')
@connext/utils (guessing 'utils')
ethers/constants (guessing 'constants')
@connext/contracts (guessing 'contracts')
ethers (guessing 'ethers')
ethers/utils (guessing 'utils$1')
(!) Conflicting re-exports
src/index.ts re-exports 'SimpleLinkedTransferAppRegistryInfo' from both src/index.ts and src/SimpleLinkedTransferApp/registry.ts (will be ignored)
src/index.ts re-exports 'SimpleSignedTransferAppRegistryInfo' from both src/index.ts and src/SimpleSignedTransferApp/registry.ts (will be ignored)
src/index.ts re-exports 'SimpleTwoPartySwapAppRegistryInfo' from both src/index.ts and src/SimpleTwoPartySwapApp/registry.ts (will be ignored)
src/index.ts re-exports 'DepositAppRegistryInfo' from both src/index.ts and src/DepositApp/registry.ts (will be ignored)
created dist/index.js, dist/index.esm.js, dist/index-iife.js in 7s
Fixing permissions for 1000:1000
[Makefile] => Finished building apps in 13 seconds
=============

=============
[Makefile] => Start building client
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/client && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/client@6.6.1 build /root/modules/client
> rm -rf ./dist/* && ./node_modules/.bin/tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building client in 7 seconds
=============

=============
[Makefile] => Start building bot
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/bot && npm run build"
Running command as 0:0 (target user: 1000:1000)

> bots@0.0.1 build /root/modules/bot
> rm -rf dist && tsc --project tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building bot in 6 seconds
=============

=============
[Makefile] => Start building database
docker build --file ops/database/db.dockerfile   --tag indra_database ops/database
Sending build context to Docker daemon  35.33kB
Step 1/6 : FROM postgres:9.6.17-alpine
9.6.17-alpine: Pulling from library/postgres
cbdbe7a5bc2a: Pull complete 
b52a8a2ca21a: Pull complete 
e36a19831e31: Pull complete 
9e78cc7ff334: Pull complete 
2e90d39e4948: Pull complete 
9a3d698e123a: Pull complete 
f9855f4bfed5: Pull complete 
4da4790e2631: Pull complete 
d0f971fb23fb: Pull complete 
Digest: sha256:45c500504bf317e3beb902312a38659bbf36236df762f5e7255af66a38e0023d
Status: Downloaded newer image for postgres:9.6.17-alpine
 ---> 312f723f9cfd
Step 2/6 : WORKDIR /root
 ---> Running in e39b5e965d62
Removing intermediate container e39b5e965d62
 ---> cfbd3d01cbc2
Step 3/6 : RUN chown -R postgres:postgres /root
 ---> Running in ff804202dec7
Removing intermediate container ff804202dec7
 ---> 0082a97c5d93
Step 4/6 : RUN apk add --update --no-cache coreutils groff less mailcap py-pip && pip install --upgrade awscli
 ---> Running in 51c564c4dc2b
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
(1/16) Installing libacl (2.2.53-r0)
(2/16) Installing libattr (2.4.48-r0)
(3/16) Installing coreutils (8.31-r0)
(4/16) Installing libgcc (9.2.0-r4)
(5/16) Installing libstdc++ (9.2.0-r4)
(6/16) Installing groff (1.22.4-r0)
(7/16) Installing less (551-r0)
(8/16) Installing mailcap (2.1.48-r0)
(9/16) Installing libbz2 (1.0.8-r1)
(10/16) Installing expat (2.2.9-r1)
(11/16) Installing libffi (3.2.1-r6)
(12/16) Installing gdbm (1.13-r1)
(13/16) Installing sqlite-libs (3.30.1-r2)
(14/16) Installing python2 (2.7.18-r0)
(15/16) Installing py-setuptools (42.0.2-r0)
(16/16) Installing py2-pip (18.1-r0)
Executing busybox-1.31.1-r9.trigger
OK: 78 MiB in 44 packages
The directory '/root/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/root/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting awscli
  Downloading https://files.pythonhosted.org/packages/22/64/e5e2a1c8c277c99377e34943d802a389637a102cd3923bd6608b5ec220ea/awscli-1.18.66-py2.py3-none-any.whl (3.1MB)
Collecting botocore==1.16.16 (from awscli)
  Downloading https://files.pythonhosted.org/packages/17/ac/b21f4aba98f239ee5341d79c64bb502a64ad8ac98331bf0d9568707c6576/botocore-1.16.16-py2.py3-none-any.whl (6.2MB)
Collecting docutils<0.16,>=0.10 (from awscli)
  Downloading https://files.pythonhosted.org/packages/3a/dc/bf2b15d1fa15a6f7a9e77a61b74ecbbae7258558fcda8ffc9a6638a6b327/docutils-0.15.2-py2-none-any.whl (548kB)
Collecting rsa<=3.5.0,>=3.1.2 (from awscli)
  Downloading https://files.pythonhosted.org/packages/e1/ae/baedc9cb175552e95f3395c43055a6a5e125ae4d48a1d7a924baca83e92e/rsa-3.4.2-py2.py3-none-any.whl (46kB)
Collecting s3transfer<0.4.0,>=0.3.0 (from awscli)
  Downloading https://files.pythonhosted.org/packages/69/79/e6afb3d8b0b4e96cefbdc690f741d7dd24547ff1f94240c997a26fa908d3/s3transfer-0.3.3-py2.py3-none-any.whl (69kB)
Collecting PyYAML<5.4,>=3.10; python_version != "3.4" (from awscli)
  Downloading https://files.pythonhosted.org/packages/64/c2/b80047c7ac2478f9501676c988a5411ed5572f35d1beff9cae07d321512c/PyYAML-5.3.1.tar.gz (269kB)
Collecting colorama<0.4.4,>=0.2.5; python_version != "3.4" (from awscli)
  Downloading https://files.pythonhosted.org/packages/c9/dc/45cdef1b4d119eb96316b3117e6d5708a08029992b2fee2c143c7a0a5cc5/colorama-0.4.3-py2.py3-none-any.whl
Collecting urllib3<1.26,>=1.20; python_version != "3.4" (from botocore==1.16.16->awscli)
  Downloading https://files.pythonhosted.org/packages/e1/e5/df302e8017440f111c11cc41a6b432838672f5a70aa29227bf58149dc72f/urllib3-1.25.9-py2.py3-none-any.whl (126kB)
Collecting python-dateutil<3.0.0,>=2.1 (from botocore==1.16.16->awscli)
  Downloading https://files.pythonhosted.org/packages/d4/70/d60450c3dd48ef87586924207ae8907090de0b306af2bce5d134d78615cb/python_dateutil-2.8.1-py2.py3-none-any.whl (227kB)
Collecting jmespath<1.0.0,>=0.7.1 (from botocore==1.16.16->awscli)
  Downloading https://files.pythonhosted.org/packages/07/cb/5f001272b6faeb23c1c9e0acc04d48eaaf5c862c17709d20e3469c6e0139/jmespath-0.10.0-py2.py3-none-any.whl
Collecting pyasn1>=0.1.3 (from rsa<=3.5.0,>=3.1.2->awscli)
  Downloading https://files.pythonhosted.org/packages/62/1e/a94a8d635fa3ce4cfc7f506003548d0a2447ae76fd5ca53932970fe3053f/pyasn1-0.4.8-py2.py3-none-any.whl (77kB)
Collecting futures<4.0.0,>=2.2.0; python_version == "2.7" (from s3transfer<0.4.0,>=0.3.0->awscli)
  Downloading https://files.pythonhosted.org/packages/d8/a6/f46ae3f1da0cd4361c344888f59ec2f5785e69c872e175a748ef6071cdb5/futures-3.3.0-py2-none-any.whl
Collecting six>=1.5 (from python-dateutil<3.0.0,>=2.1->botocore==1.16.16->awscli)
  Downloading https://files.pythonhosted.org/packages/ee/ff/48bde5c0f013094d729fe4b0316ba2a24774b3ff1c52d924a8a4cb04078a/six-1.15.0-py2.py3-none-any.whl
Installing collected packages: docutils, urllib3, six, python-dateutil, jmespath, botocore, pyasn1, rsa, futures, s3transfer, PyYAML, colorama, awscli
  Running setup.py install for PyYAML: started
    Running setup.py install for PyYAML: finished with status 'done'
Successfully installed PyYAML-5.3.1 awscli-1.18.66 botocore-1.16.16 colorama-0.4.3 docutils-0.15.2 futures-3.3.0 jmespath-0.10.0 pyasn1-0.4.8 python-dateutil-2.8.1 rsa-3.4.2 s3transfer-0.3.3 six-1.15.0 urllib3-1.25.9
Removing intermediate container 51c564c4dc2b
 ---> 96dce1e8758e
Step 5/6 : COPY . .
 ---> 0e7b572eb7cd
Step 6/6 : ENTRYPOINT ["bash", "entry.sh"]
 ---> Running in 1b5256cbad3e
Removing intermediate container 1b5256cbad3e
 ---> e7dc64b28f85
Successfully built e7dc64b28f85
Successfully tagged indra_database:latest
docker tag indra_database indra_database:451b4a36
[Makefile] => Finished building database in 16 seconds
=============

=============
[Makefile] => Start building proxy
docker build --file ops/proxy/indra/Dockerfile   --tag indra_proxy ops
Sending build context to Docker daemon  156.7kB
Step 1/7 : FROM haproxy:2.1.3-alpine
2.1.3-alpine: Pulling from library/haproxy
aad63a933944: Pull complete 
abeb1f185d76: Pull complete 
ff727bf9a3b4: Pull complete 
Digest: sha256:b8a2d427311d747c7114405d5bc0fe6cea61db362fc5376c1a834386ca8fe83a
Status: Downloaded newer image for haproxy:2.1.3-alpine
 ---> 11622145bc56
Step 2/7 : WORKDIR /root
 ---> Running in 34c9e3a78fcb
Removing intermediate container 34c9e3a78fcb
 ---> 69c39123dfce
Step 3/7 : ENV HOME /root
 ---> Running in f082424b3782
Removing intermediate container f082424b3782
 ---> 05414a832d6c
Step 4/7 : RUN apk add --update --no-cache bash ca-certificates certbot curl iputils openssl
 ---> Running in 0111ab1efe76
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.11/community/x86_64/APKINDEX.tar.gz
(1/51) Installing ncurses-terminfo-base (6.1_p20200118-r4)
(2/51) Installing ncurses-libs (6.1_p20200118-r4)
(3/51) Installing readline (8.0.1-r0)
(4/51) Installing bash (5.0.11-r1)
Executing bash-5.0.11-r1.post-install
(5/51) Installing ca-certificates (20191127-r1)
(6/51) Installing libbz2 (1.0.8-r1)
(7/51) Installing expat (2.2.9-r1)
(8/51) Installing libffi (3.2.1-r6)
(9/51) Installing gdbm (1.13-r1)
(10/51) Installing xz-libs (5.2.4-r0)
(11/51) Installing sqlite-libs (3.30.1-r2)
(12/51) Installing python3 (3.8.2-r0)
(13/51) Installing py3-setuptools (42.0.2-r0)
(14/51) Installing py3-cparser (2.19-r4)
(15/51) Installing py3-cffi (1.13.2-r0)
(16/51) Installing py3-idna (2.8-r3)
(17/51) Installing py3-asn1crypto (1.2.0-r1)
(18/51) Installing py3-six (1.13.0-r0)
(19/51) Installing py3-cryptography (2.8-r1)
(20/51) Installing py3-openssl (19.1.0-r0)
(21/51) Installing py3-josepy (1.2.0-r3)
(22/51) Installing py3-pbr (5.4.4-r0)
(23/51) Installing py3-mock (2.0.0-r6)
(24/51) Installing py3-tz (2019.3-r2)
(25/51) Installing py3-pyrfc3339 (1.1-r3)
(26/51) Installing py3-chardet (3.0.4-r3)
(27/51) Installing py3-certifi (2019.9.11-r2)
(28/51) Installing py3-urllib3 (1.25.7-r1)
(29/51) Installing py3-requests (2.22.0-r0)
(30/51) Installing py3-requests-toolbelt (0.9.1-r1)
(31/51) Installing py3-acme (1.0.0-r0)
(32/51) Installing py3-configargparse (0.15.2-r0)
(33/51) Installing py3-configobj (5.0.6-r7)
(34/51) Installing py3-distro (1.4.0-r3)
(35/51) Installing py3-distutils-extra (2.42-r1)
(36/51) Installing py3-future (0.18.2-r0)
(37/51) Installing py3-parsedatetime (2.5-r0)
(38/51) Installing py3-zope-interface (4.7.1-r0)
(39/51) Installing py3-zope-proxy (4.3.3-r0)
(40/51) Installing py3-zope-deferredimport (4.3.1-r2)
(41/51) Installing py3-zope-deprecation (4.4.0-r3)
(42/51) Installing py3-zope-event (4.4-r4)
(43/51) Installing py3-zope-hookable (5.0.0-r0)
(44/51) Installing py3-zope-component (4.6-r0)
(45/51) Installing certbot (1.0.0-r0)
(46/51) Installing nghttp2-libs (1.40.0-r0)
(47/51) Installing libcurl (7.67.0-r0)
(48/51) Installing curl (7.67.0-r0)
(49/51) Installing libcap (2.27-r0)
(50/51) Installing iputils (20190709-r0)
(51/51) Installing openssl (1.1.1g-r0)
Executing busybox-1.31.1-r9.trigger
Executing ca-certificates-20191127-r1.trigger
OK: 97 MiB in 68 packages
Removing intermediate container 0111ab1efe76
 ---> 3ec06d369ec5
Step 5/7 : RUN curl https://raw.githubusercontent.com/vishnubob/wait-for-it/ed77b63706ea721766a62ff22d3a251d8b4a6a30/wait-for-it.sh > /bin/wait-for && chmod +x /bin/wait-for
 ---> Running in 7c7dfd25abc0
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5224  100  5224    0     0  18013      0 --:--:-- --:--:-- --:--:-- 17951
Removing intermediate container 7c7dfd25abc0
 ---> 7dee5fa58e59
Step 6/7 : COPY proxy/indra/ /root/
 ---> 9328634934b4
Step 7/7 : ENTRYPOINT ["bash", "/root/entry.sh"]
 ---> Running in d71e580c52b0
Removing intermediate container d71e580c52b0
 ---> 4d538fab927b
Successfully built 4d538fab927b
Successfully tagged indra_proxy:latest
docker tag indra_proxy indra_proxy:451b4a36
[Makefile] => Finished building proxy in 11 seconds
=============

=============
[Makefile] => Start building node
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/node && npm run build && touch src/main.ts"
Running command as 0:0 (target user: 1000:1000)

> indra-node@6.6.1 build /root/modules/node
> rm -rf ./dist/* && tsc -p tsconfig.json

Fixing permissions for 1000:1000
[Makefile] => Finished building node in 10 seconds
=============

=============
[Makefile] => Start building test-runner
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "cd modules/test-runner && npm run build"
Running command as 0:0 (target user: 1000:1000)

> @connext/test-runner@0.0.1 build /root/modules/test-runner
> webpack --config ops/webpack.config.js

Building staging-mode bundle
Hash: 69b08fecfaac4cd3d777
Version: webpack 4.43.0
Time: 8902ms
Built at: 05/25/2020 9:04:25 AM
                                        Asset       Size  Chunks             Chunk Names
    channelProvider/channelProvider.test.d.ts   61 bytes          [emitted]  
channelProvider/channelProvider.test.d.ts.map  153 bytes          [emitted]  
        clientConnect/clientConnect.test.d.ts   59 bytes          [emitted]  
    clientConnect/clientConnect.test.d.ts.map  147 bytes          [emitted]  
        createChannel/createChannel.test.d.ts   59 bytes          [emitted]  
    createChannel/createChannel.test.d.ts.map  147 bytes          [emitted]  
                    deposit/deposit.test.d.ts   53 bytes          [emitted]  
                deposit/deposit.test.d.ts.map  129 bytes          [emitted]  
             deposit/depositOffline.test.d.ts   60 bytes          [emitted]  
         deposit/depositOffline.test.d.ts.map  143 bytes          [emitted]  
              deposit/depositRights.test.d.ts   59 bytes          [emitted]  
          deposit/depositRights.test.d.ts.map  141 bytes          [emitted]  
            flows/multichannelStore.test.d.ts   63 bytes          [emitted]  
        flows/multichannelStore.test.d.ts.map  147 bytes          [emitted]  
          flows/multiclientTransfer.test.d.ts   65 bytes          [emitted]  
      flows/multiclientTransfer.test.d.ts.map  151 bytes          [emitted]  
                     flows/transfer.test.d.ts   54 bytes          [emitted]  
                 flows/transfer.test.d.ts.map  129 bytes          [emitted]  
      getAppRegistry/getAppRegistry.test.d.ts   60 bytes          [emitted]  
  getAppRegistry/getAppRegistry.test.d.ts.map  150 bytes          [emitted]  
    getStateChannel/getStateChannel.test.d.ts   61 bytes          [emitted]  
getStateChannel/getStateChannel.test.d.ts.map  153 bytes          [emitted]  
                                   index.d.ts   1.09 KiB          [emitted]  
                               index.d.ts.map  721 bytes          [emitted]  
               rebalance/collateral.test.d.ts   56 bytes          [emitted]  
           rebalance/collateral.test.d.ts.map  137 bytes          [emitted]  
                 rebalance/profiles.test.d.ts   54 bytes          [emitted]  
             rebalance/profiles.test.d.ts.map  133 bytes          [emitted]  
                  rebalance/reclaim.test.d.ts   53 bytes          [emitted]  
              rebalance/reclaim.test.d.ts.map  131 bytes          [emitted]  
          restoreState/restoreState.test.d.ts   58 bytes          [emitted]  
      restoreState/restoreState.test.d.ts.map  144 bytes          [emitted]  
                                   setup.d.ts   46 bytes          [emitted]  
                               setup.d.ts.map  104 bytes          [emitted]  
                          swap/swap.test.d.ts   50 bytes          [emitted]  
                      swap/swap.test.d.ts.map  120 bytes          [emitted]  
                   swap/swapOffline.test.d.ts   57 bytes          [emitted]  
               swap/swapOffline.test.d.ts.map  134 bytes          [emitted]  
                              tests.bundle.js   6.09 MiB   tests  [emitted]  tests
             transfer/asyncTransfer.test.d.ts   59 bytes          [emitted]  
         transfer/asyncTransfer.test.d.ts.map  142 bytes          [emitted]  
      transfer/asyncTransferOffline.test.d.ts   66 bytes          [emitted]  
  transfer/asyncTransferOffline.test.d.ts.map  156 bytes          [emitted]  
       transfer/concurrentTransfers.test.d.ts   65 bytes          [emitted]  
   transfer/concurrentTransfers.test.d.ts.map  154 bytes          [emitted]  
         transfer/getLinkedTransfer.test.d.ts   63 bytes          [emitted]  
     transfer/getLinkedTransfer.test.d.ts.map  150 bytes          [emitted]  
          transfer/hashLockTransfer.test.d.ts   62 bytes          [emitted]  
      transfer/hashLockTransfer.test.d.ts.map  148 bytes          [emitted]  
            transfer/signedTransfer.test.d.ts   60 bytes          [emitted]  
        transfer/signedTransfer.test.d.ts.map  144 bytes          [emitted]  
     transfer/signedTransferOffline.test.d.ts   67 bytes          [emitted]  
 transfer/signedTransferOffline.test.d.ts.map  158 bytes          [emitted]  
                         util/assertions.d.ts   71 bytes          [emitted]  
                     util/assertions.d.ts.map  161 bytes          [emitted]  
                    util/channelProvider.d.ts  534 bytes          [emitted]  
                util/channelProvider.d.ts.map  492 bytes          [emitted]  
                             util/client.d.ts  964 bytes          [emitted]  
                         util/client.d.ts.map  623 bytes          [emitted]  
                          util/constants.d.ts   2.57 KiB          [emitted]  
                      util/constants.d.ts.map    1.4 KiB          [emitted]  
                                 util/db.d.ts  382 bytes          [emitted]  
                             util/db.d.ts.map  318 bytes          [emitted]  
                                util/env.d.ts  467 bytes          [emitted]  
                            util/env.d.ts.map  155 bytes          [emitted]  
                        util/ethprovider.d.ts  719 bytes          [emitted]  
                    util/ethprovider.d.ts.map  523 bytes          [emitted]  
         util/helpers/asyncTransferAsset.d.ts  433 bytes          [emitted]  
     util/helpers/asyncTransferAsset.d.ts.map  485 bytes          [emitted]  
              util/helpers/backupService.d.ts  333 bytes          [emitted]  
          util/helpers/backupService.d.ts.map  403 bytes          [emitted]  
                util/helpers/fundChannel.d.ts  366 bytes          [emitted]  
            util/helpers/fundChannel.d.ts.map  287 bytes          [emitted]  
           util/helpers/rebalanceProfile.d.ts  297 bytes          [emitted]  
       util/helpers/rebalanceProfile.d.ts.map  275 bytes          [emitted]  
       util/helpers/requestDepositRights.d.ts  232 bytes          [emitted]  
   util/helpers/requestDepositRights.d.ts.map  232 bytes          [emitted]  
                  util/helpers/swapAsset.d.ts  415 bytes          [emitted]  
              util/helpers/swapAsset.d.ts.map  455 bytes          [emitted]  
        util/helpers/withdrawFromChannel.d.ts  281 bytes          [emitted]  
    util/helpers/withdrawFromChannel.d.ts.map  269 bytes          [emitted]  
                              util/index.d.ts  591 bytes          [emitted]  
                          util/index.d.ts.map  481 bytes          [emitted]  
                          util/messaging.d.ts   3.75 KiB          [emitted]  
                      util/messaging.d.ts.map   3.08 KiB          [emitted]  
                               util/misc.d.ts  111 bytes          [emitted]  
                           util/misc.d.ts.map  142 bytes          [emitted]  
                               util/nats.d.ts  219 bytes          [emitted]  
                           util/nats.d.ts.map  244 bytes          [emitted]  
                              util/types.d.ts  532 bytes          [emitted]  
                          util/types.d.ts.map  510 bytes          [emitted]  
                  withdraw/withdraw.test.d.ts   54 bytes          [emitted]  
              withdraw/withdraw.test.d.ts.map  132 bytes          [emitted]  
           withdraw/withdrawOffline.test.d.ts   61 bytes          [emitted]  
       withdraw/withdrawOffline.test.d.ts.map  146 bytes          [emitted]  
Entrypoint tests = tests.bundle.js
[./src/channelProvider/channelProvider.test.ts] 3.3 KiB {tests} [built]
[./src/clientConnect/clientConnect.test.ts] 4.57 KiB {tests} [built]
[./src/createChannel/createChannel.test.ts] 2.66 KiB {tests} [built]
[./src/deposit/deposit.test.ts] 8.04 KiB {tests} [built]
[./src/deposit/depositOffline.test.ts] 5.75 KiB {tests} [built]
[./src/deposit/depositRights.test.ts] 3.18 KiB {tests} [built]
[./src/flows/multichannelStore.test.ts] 12.2 KiB {tests} [built]
[./src/flows/multiclientTransfer.test.ts] 3.97 KiB {tests} [built]
[./src/flows/transfer.test.ts] 4.62 KiB {tests} [built]
[./src/getAppRegistry/getAppRegistry.test.ts] 1.58 KiB {tests} [built]
[./src/getStateChannel/getStateChannel.test.ts] 5.38 KiB {tests} [built]
[./src/index.ts] 1.05 KiB {tests} [built]
[./src/rebalance/collateral.test.ts] 1.83 KiB {tests} [built]
[./src/rebalance/profiles.test.ts] 1.98 KiB {tests} [built]
[./src/rebalance/reclaim.test.ts] 4.4 KiB {tests} [built]
    + 659 hidden modules
Fixing permissions for 1000:1000
[Makefile] => Finished building test-runner in 11 seconds
=============

INDRA_UI=daicard bash ops/start-dev.sh
Swarm initialized: current node (lcyave24ryn800vtzlzd4wn9q) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1wb7k0k3ixbjfqtb15wrzenv3xh7p3iuyjero3iojdnbh55ypy-f2ialjh480ozsq2uox1ouda40 89.145.161.66:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

Fetching chainId from https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86
ops/start-dev.sh: line 72: jq: command not found
(23) Failed writing body
ops/start-dev.sh: line 75: jq: command not found
Failed to fetch chainId from provider https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86
make: *** [Makefile:56: start-daicard] Error 1
ubuntu@nathan:~/indra$ sudo apt install jq
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following package was automatically installed and is no longer required:
  wmdocker
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  libjq1 libonig5
The following NEW packages will be installed:
  jq libjq1 libonig5
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 313 kB of archives.
After this operation, 1062 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://de.archive.ubuntu.com/ubuntu focal/universe amd64 libonig5 amd64 6.9.4-1 [142 kB]
Get:2 http://de.archive.ubuntu.com/ubuntu focal/universe amd64 libjq1 amd64 1.6-1 [121 kB]
Get:3 http://de.archive.ubuntu.com/ubuntu focal/universe amd64 jq amd64 1.6-1 [50.2 kB]
Fetched 313 kB in 0s (1698 kB/s)
Selecting previously unselected package libonig5:amd64.
(Reading database ... 63468 files and directories currently installed.)
Preparing to unpack .../libonig5_6.9.4-1_amd64.deb ...
Unpacking libonig5:amd64 (6.9.4-1) ...
Selecting previously unselected package libjq1:amd64.
Preparing to unpack .../libjq1_1.6-1_amd64.deb ...
Unpacking libjq1:amd64 (1.6-1) ...
Selecting previously unselected package jq.
Preparing to unpack .../archives/jq_1.6-1_amd64.deb ...
Unpacking jq (1.6-1) ...
Setting up libonig5:amd64 (6.9.4-1) ...
Setting up libjq1:amd64 (1.6-1) ...
Setting up jq (1.6-1) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9) ...
ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
INDRA_UI=daicard bash ops/start-dev.sh
Fetching chainId from https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86
Got chainId 4, using token 0x16655FAf612D714039F92c408407D46c5A394a6C
indra: Pulling from provide/nats-server
4167d3e14976: Pull complete 
b103ef1b79b2: Pull complete 
95440de44ab7: Pull complete 
91d0377beb2d: Pull complete 
91e2c0f07446: Pull complete 
Digest: sha256:fcc376acc151cc19297ed4613f08e642bff5d59a416b494842ae66eeac91b95d
Status: Downloaded newer image for provide/nats-server:indra
docker.io/provide/nats-server:indra
5-alpine: Pulling from library/redis
cbdbe7a5bc2a: Already exists 
dc0373118a0d: Pull complete 
cfd369fe6256: Pull complete 
3e45770272d9: Pull complete 
558de8ea3153: Pull complete 
a2c652551612: Pull complete 
Digest: sha256:83a3af36d5e57f2901b4783c313720e5fa3ecf0424ba86ad9775e06a9a5e35d0
Status: Downloaded newer image for redis:5-alpine
docker.io/library/redis:5-alpine
Created secret called indra_database_dev with id w8foaf81nrw5rdedwhlgdeqbx
Created ATTACHABLE network with id 51gtzjdycp939mjg4smnixsjv
Creating service indra_ethprovider
Creating service indra_database
Creating service indra_nats
Creating service indra_redis
Creating service indra_proxy
Creating service indra_webserver
Creating service indra_node
Waiting for the indra stack to wake up... Good Morning!
ubuntu@nathan:~/indra$ ls
LICENSE  Makefile  README.md  cypress  cypress.json  dev.env  docs  lerna.json  modules  node_modules  ops  package-lock.json  package.json  prod.env  tsconfig.json
ubuntu@nathan:~/indra$ make dls
ID                  NAME                MODE                REPLICAS            IMAGE                       PORTS
kgraouz1f99e        indra_database      global              1/1                 indra_database:latest       *:5432->5432/tcp
awjqobo4p50u        indra_ethprovider   replicated          1/1                 indra_builder:latest        *:8545->8545/tcp
kk317qiugfzz        indra_nats          replicated          1/1                 provide/nats-server:indra   *:4221-4222->4221-4222/tcp
rle4nrcjuq8y        indra_node          replicated          1/1                 indra_builder:latest        *:8080->8080/tcp
26lxb0lktzdw        indra_proxy         replicated          1/1                 indra_proxy:latest          *:3000->80/tcp
iykeure2qtps        indra_redis         replicated          1/1                 redis:5-alpine              *:6379->6379/tcp
nu0padvg0a7b        indra_webserver     replicated          1/1                 indra_builder:latest        
=====
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                                         NAMES
2b33b8411b10        indra_builder:latest        "bash modules/node/o…"   2 minutes ago       Up 2 minutes                                                      indra_node.1.hyqg8a1vik88tx7cskyh6uk1s
2b8771e7554c        indra_builder:latest        "npm start"              2 minutes ago       Up 2 minutes                                                      indra_webserver.1.3gy6e10rim5of4lhf7jk2ueyu
c678ffff7cc6        indra_proxy:latest          "bash /root/entry.sh"    2 minutes ago       Up 2 minutes                                                      indra_proxy.1.yyjzoamak6mkmt28ketbkbndr
1dab9a55e8a1        redis:5-alpine              "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        6379/tcp                                      indra_redis.1.22o332g7ebifrdekjtkz3bbp3
7d65a247201e        provide/nats-server:indra   "/bin/nats-server -D…"   2 minutes ago       Up 2 minutes        4221-4222/tcp, 5222/tcp, 6222/tcp, 8222/tcp   indra_nats.1.udyvn6s6l2j4f76joeb033pzc
8dbd9cce34dc        indra_database:latest       "bash entry.sh"          2 minutes ago       Up 2 minutes        5432/tcp                                      indra_database.lcyave24ryn800vtzlzd4wn9q.zafny1gc5i0rzbyrf9k9rg8oo
73d2f855de43        indra_builder:latest        "bash modules/contra…"   2 minutes ago       Up 2 minutes                                                      indra_ethprovider.1.jp2d3df5ax5hviknh8f4jrmfj
ubuntu@nathan:~/indra$ bash ops/logs.sh node
ID                          NAME                IMAGE                  NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
hyqg8a1vik88tx7cskyh6uk1s   indra_node.1        indra_builder:latest   nathan              Running             Running 2 minutes ago                       
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.109Z [Main] TypeOrmModule dependencies initialized
2020-05-25T09:11:27.110Z [Main] OnchainTransactionModule dependencies initialized
Signed 672-byte bearer authorization token for subject: indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR
2020-05-25T09:11:27.117Z [Main] CollateralModule dependencies initialized
2020-05-25T09:11:27.117Z [Main] AuthModule dependencies initialized
2020-05-25T09:11:27.119Z [Main] MessagingModule dependencies initialized
2020-05-25T09:11:27.120Z [CFCoreProvider] Derived address from mnemonic: 0x627306090abaB3A6e1400e9345bC60c78a8BEf57
2020-05-25T09:11:27.120Z [MessagingInterface] Connected message pattern "*.lock.acquire.>" to function 
2020-05-25T09:11:27.122Z [CF-Node] Node signer address: 0x627306090abaB3A6e1400e9345bC60c78a8BEf57
2020-05-25T09:11:27.122Z [MessagingInterface] Connected message pattern "*.lock.release.>" to function 
2020-05-25T09:11:27.124Z [SwapRateMessaging] Connected message pattern "*.swap-rate.>" to function bound getLatestSwapRate
2020-05-25T09:11:27.124Z [Main] LockModule dependencies initialized
2020-05-25T09:11:27.124Z [Main] SwapRateModule dependencies initialized
2020-05-25T09:11:27.502Z [CFCoreProvider] Balance of signer address 0x627306090abaB3A6e1400e9345bC60c78a8BEf57 on rinkeby (chainId 4): 173511000000000
2020-05-25T09:11:27.502Z [CFCoreProvider] CFCore created
2020-05-25T09:11:27.503Z [Main] CFCoreModule dependencies initialized
2020-05-25T09:11:27.504Z [Main] WithdrawModule dependencies initialized
2020-05-25T09:11:27.504Z [Main] DepositModule dependencies initialized
2020-05-25T09:11:27.506Z [MessagingInterface] Connected message pattern "*.channel.get" to function 
2020-05-25T09:11:27.507Z [MessagingInterface] Connected message pattern "*.channel.create" to function 
2020-05-25T09:11:27.507Z [MessagingInterface] Connected message pattern "*.channel.request-collateral" to function 
2020-05-25T09:11:27.511Z [MessagingInterface] Connected message pattern "*.channel.get-profile" to function 
2020-05-25T09:11:27.511Z [MessagingInterface] Connected message pattern "admin.get-no-free-balance" to function bound getNoFreeBalance
2020-05-25T09:11:27.511Z [LinkedTransferMessaging] Connected message pattern "*.transfer.get-linked" to function 
2020-05-25T09:11:27.511Z [LinkedTransferMessaging] Connected message pattern "*.transfer.get-hashlock" to function 
2020-05-25T09:11:27.511Z [LinkedTransferMessaging] Connected message pattern "*.transfer.install-signed" to function 
2020-05-25T09:11:27.512Z [MessagingInterface] Connected message pattern "*.channel.restore" to function 
2020-05-25T09:11:27.512Z [TransferMessaging] Connected message pattern "*.transfer.get-history" to function 
2020-05-25T09:11:27.512Z [MessagingInterface] Connected message pattern "admin.get-state-channel-by-address" to function bound getStateChannelByUserPublicIdentifier
2020-05-25T09:11:27.512Z [LinkedTransferMessaging] Connected message pattern "*.transfer.install-linked" to function 
2020-05-25T09:11:27.512Z [LinkedTransferMessaging] Connected message pattern "*.transfer.get-signed" to function 
2020-05-25T09:11:27.512Z [MessagingInterface] Connected message pattern "*.channel.latestWithdrawal" to function 
2020-05-25T09:11:27.512Z [TransferMessaging] Connected message pattern "*.client.check-in" to function 
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-state-channel-by-multisig" to function bound getStateChannelByMultisig
2020-05-25T09:11:27.513Z [LinkedTransferMessaging] Connected message pattern "*.transfer.get-pending" to function 
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-all-channels" to function bound getAllChannels
2020-05-25T09:11:27.513Z [Main] HashLockTransferModule dependencies initialized
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-all-linked-transfers" to function bound getAllLinkedTransfers
2020-05-25T09:11:27.513Z [Main] SignedTransferModule dependencies initialized
2020-05-25T09:11:27.513Z [Main] ChannelModule dependencies initialized
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-linked-transfer-by-payment-id" to function bound getLinkedTransferByPaymentId
2020-05-25T09:11:27.513Z [Main] TransferModule dependencies initialized
2020-05-25T09:11:27.514Z [Main] LinkedTransferModule dependencies initialized
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.get-channels-for-merging" to function bound getChannelsForMerging
2020-05-25T09:11:27.514Z [Main] ListenerModule dependencies initialized
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.repair-critical-addresses" to function bound repairCriticalStateChannelAddresses
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.*.channel.add-profile" to function bound addRebalanceProfile
2020-05-25T09:11:27.514Z [Main] AppRegistryModule dependencies initialized
2020-05-25T09:11:27.514Z [Main] AdminModule dependencies initialized
2020-05-25T09:11:27.519Z [Main] AuthController {/auth}:
2020-05-25T09:11:27.521Z [Main] Mapped {/auth/:userIdentifier, GET} route
2020-05-25T09:11:27.521Z [Main] Mapped {/auth, OPTIONS} route
2020-05-25T09:11:27.522Z [Main] Mapped {/auth, POST} route
2020-05-25T09:11:27.522Z [Main] ConfigController {/config}:
2020-05-25T09:11:27.522Z [Main] Mapped {/config, GET} route
2020-05-25T09:11:27.522Z [Main] AppRegistryController {/app-registry}:
2020-05-25T09:11:27.522Z [Main] Mapped {/app-registry, GET} route
2020-05-25T09:11:27.522Z [Main] CollateralController {/collateral}:
2020-05-25T09:11:27.523Z [Main] Mapped {/collateral/anonymized, GET} route
2020-05-25T09:11:27.523Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_CREATED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_UNLOCKED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CREATE_CHANNEL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SETUP_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_CONFIRMED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_STARTED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event INSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROPOSE_INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROPOSE_INSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROTOCOL_MESSAGE_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event REJECT_INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SYNC
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SYNC_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UNINSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UNINSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UPDATE_STATE_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UPDATE_STATE_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_CONFIRMED_EVENT
2020-05-25T09:11:27.525Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_STARTED_EVENT
2020-05-25T09:11:27.525Z [CFCoreService] Registering cfCore callback for event chan_uninstall
2020-05-25T09:11:27.534Z [AppRegistryService] Creating SimpleLinkedTransferApp app on chain 4: 0x59123E806a3D12bC8aD972ABc6C7b88d1b6C086A
2020-05-25T09:11:27.547Z [AppRegistryService] Creating SimpleSignedTransferApp app on chain 4: 0xfbc3E3Ff1e4431AD1a6B172e746fB5263e384Ee9
2020-05-25T09:11:27.551Z [AppRegistryService] Creating SimpleTwoPartySwapApp app on chain 4: 0x8cCcCfdca46E3fAe8f7eD286C568F03A9958480C
2020-05-25T09:11:27.558Z [AppRegistryService] Creating WithdrawApp app on chain 4: 0x432cA8Cda1CB543959EDA76e12428F3ff8B31856
2020-05-25T09:11:27.562Z [AppRegistryService] Creating HashLockTransferApp app on chain 4: 0x5f95A37cf313e98Df727e489B7301582a146680F
2020-05-25T09:11:27.566Z [AppRegistryService] Creating DepositApp app on chain 4: 0x7333856f179170fdb5456B48028f78d93D293A87
2020-05-25T09:11:27.577Z [AppRegistryService] Injecting CF Core middleware
2020-05-25T09:11:27.578Z [AppRegistryService] Injected CF Core middleware
2020-05-25T09:11:27.581Z [Main] Nest application successfully started
2020-05-25T09:11:27.927Z [SwapRateService] Got swap rate from Uniswap at block 10134052: 0.01
2020-05-25T09:11:27.928Z [SwapRateService] Got swap rate from Uniswap at block 10134052: 100.0
^C
ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86" make start
INDRA_UI=daicard bash ops/start-dev.sh
Fetching chainId from https://rinkeby.infura.io/v3/e7813e36e2ea466f89a0f56fcc340a86
Got chainId 4, using token 0x16655FAf612D714039F92c408407D46c5A394a6C
Updating service indra_proxy (id: 26lxb0lktzdwqgiwyehlc1986)
image indra_proxy:latest could not be accessed on a registry to record
its digest. Each node will access indra_proxy:latest independently,
possibly leading to different nodes running different
versions of the image.

Updating service indra_webserver (id: nu0padvg0a7bk4isndlfekyuc)
image indra_builder:latest could not be accessed on a registry to record
its digest. Each node will access indra_builder:latest independently,
possibly leading to different nodes running different
versions of the image.

Updating service indra_node (id: rle4nrcjuq8ygqo08axzmcm1c)
image indra_builder:latest could not be accessed on a registry to record
its digest. Each node will access indra_builder:latest independently,
possibly leading to different nodes running different
versions of the image.

Updating service indra_ethprovider (id: awjqobo4p50u9llvem60b4o4c)
image indra_builder:latest could not be accessed on a registry to record
its digest. Each node will access indra_builder:latest independently,
possibly leading to different nodes running different
versions of the image.

Updating service indra_database (id: kgraouz1f99erzh84j6sawxsn)
image indra_database:latest could not be accessed on a registry to record
its digest. Each node will access indra_database:latest independently,
possibly leading to different nodes running different
versions of the image.

Updating service indra_nats (id: kk317qiugfzz2r81sjcvsvgnj)
Updating service indra_redis (id: iykeure2qtps3w1h4reypxdsx)
Waiting for the indra stack to wake up. Good Morning!
ubuntu@nathan:~/indra$ bash ops/logs.sh node
ID                          NAME                IMAGE                  NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
hyqg8a1vik88tx7cskyh6uk1s   indra_node.1        indra_builder:latest   nathan              Running             Running 8 minutes ago                       
2020-05-25T09:11:27.512Z [MessagingInterface] Connected message pattern "*.channel.latestWithdrawal" to function 
2020-05-25T09:11:27.512Z [TransferMessaging] Connected message pattern "*.client.check-in" to function 
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-state-channel-by-multisig" to function bound getStateChannelByMultisig
2020-05-25T09:11:27.513Z [LinkedTransferMessaging] Connected message pattern "*.transfer.get-pending" to function 
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-all-channels" to function bound getAllChannels
2020-05-25T09:11:27.513Z [Main] HashLockTransferModule dependencies initialized
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-all-linked-transfers" to function bound getAllLinkedTransfers
2020-05-25T09:11:27.513Z [Main] SignedTransferModule dependencies initialized
2020-05-25T09:11:27.513Z [Main] ChannelModule dependencies initialized
2020-05-25T09:11:27.513Z [MessagingInterface] Connected message pattern "admin.get-linked-transfer-by-payment-id" to function bound getLinkedTransferByPaymentId
2020-05-25T09:11:27.513Z [Main] TransferModule dependencies initialized
2020-05-25T09:11:27.514Z [Main] LinkedTransferModule dependencies initialized
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.get-channels-for-merging" to function bound getChannelsForMerging
2020-05-25T09:11:27.514Z [Main] ListenerModule dependencies initialized
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.repair-critical-addresses" to function bound repairCriticalStateChannelAddresses
2020-05-25T09:11:27.514Z [MessagingInterface] Connected message pattern "admin.*.channel.add-profile" to function bound addRebalanceProfile
2020-05-25T09:11:27.514Z [Main] AppRegistryModule dependencies initialized
2020-05-25T09:11:27.514Z [Main] AdminModule dependencies initialized
2020-05-25T09:11:27.519Z [Main] AuthController {/auth}:
2020-05-25T09:11:27.521Z [Main] Mapped {/auth/:userIdentifier, GET} route
2020-05-25T09:11:27.521Z [Main] Mapped {/auth, OPTIONS} route
2020-05-25T09:11:27.522Z [Main] Mapped {/auth, POST} route
2020-05-25T09:11:27.522Z [Main] ConfigController {/config}:
2020-05-25T09:11:27.522Z [Main] Mapped {/config, GET} route
2020-05-25T09:11:27.522Z [Main] AppRegistryController {/app-registry}:
2020-05-25T09:11:27.522Z [Main] Mapped {/app-registry, GET} route
2020-05-25T09:11:27.522Z [Main] CollateralController {/collateral}:
2020-05-25T09:11:27.523Z [Main] Mapped {/collateral/anonymized, GET} route
2020-05-25T09:11:27.523Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_CREATED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_UNLOCKED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CONDITIONAL_TRANSFER_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event CREATE_CHANNEL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SETUP_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_CONFIRMED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event DEPOSIT_STARTED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event INSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROPOSE_INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROPOSE_INSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event PROTOCOL_MESSAGE_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event REJECT_INSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SYNC
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event SYNC_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UNINSTALL_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UNINSTALL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UPDATE_STATE_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event UPDATE_STATE_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_FAILED_EVENT
2020-05-25T09:11:27.524Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_CONFIRMED_EVENT
2020-05-25T09:11:27.525Z [CFCoreService] Registering cfCore callback for event WITHDRAWAL_STARTED_EVENT
2020-05-25T09:11:27.525Z [CFCoreService] Registering cfCore callback for event chan_uninstall
2020-05-25T09:11:27.534Z [AppRegistryService] Creating SimpleLinkedTransferApp app on chain 4: 0x59123E806a3D12bC8aD972ABc6C7b88d1b6C086A
2020-05-25T09:11:27.547Z [AppRegistryService] Creating SimpleSignedTransferApp app on chain 4: 0xfbc3E3Ff1e4431AD1a6B172e746fB5263e384Ee9
2020-05-25T09:11:27.551Z [AppRegistryService] Creating SimpleTwoPartySwapApp app on chain 4: 0x8cCcCfdca46E3fAe8f7eD286C568F03A9958480C
2020-05-25T09:11:27.558Z [AppRegistryService] Creating WithdrawApp app on chain 4: 0x432cA8Cda1CB543959EDA76e12428F3ff8B31856
2020-05-25T09:11:27.562Z [AppRegistryService] Creating HashLockTransferApp app on chain 4: 0x5f95A37cf313e98Df727e489B7301582a146680F
2020-05-25T09:11:27.566Z [AppRegistryService] Creating DepositApp app on chain 4: 0x7333856f179170fdb5456B48028f78d93D293A87
2020-05-25T09:11:27.577Z [AppRegistryService] Injecting CF Core middleware
2020-05-25T09:11:27.578Z [AppRegistryService] Injected CF Core middleware
2020-05-25T09:11:27.581Z [Main] Nest application successfully started
2020-05-25T09:11:27.927Z [SwapRateService] Got swap rate from Uniswap at block 10134052: 0.01
2020-05-25T09:11:27.928Z [SwapRateService] Got swap rate from Uniswap at block 10134052: 100.0
Signed 760-byte bearer authorization token for subject: indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c
2020-05-25T09:16:25.947Z [MessagingInterface] Responded to indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c.channel.get in 29 ms
2020-05-25T09:16:26.117Z [ChannelService] create indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c started
2020-05-25T09:16:26.132Z [LockService] Acquiring lock for chan_create:indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR at 1590398186132
2020-05-25T09:16:26.138Z [LockService] Acquired lock for chan_create:indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR with secret 7c70d419e9295c5010ebe590306b4b19
2020-05-25T09:16:26.929Z [CF-SetupProtocol] [fb753efc-46be-4967-b229-8a888333c90d] Initiation started
2020-05-25T09:16:27.796Z [CF-SetupProtocol] [fb753efc-46be-4967-b229-8a888333c90d] Received responder's sigs in 837 ms
2020-05-25T09:16:27.862Z [CF-SetupProtocol] [fb753efc-46be-4967-b229-8a888333c90d] Initiation finished in 933 ms
2020-05-25T09:16:27.863Z [ChannelService] makeAvailable {"from":"indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR","type":"CREATE_CHANNEL_EVENT","data":{"multisigAddress":"0x9041ec35D8df1A2d8A14b47522e3DaEE42e9C9a4","owners":["0x627306090abaB3A6e1400e9345bC60c78a8BEf57","0x5ADf8eF9c2BB8cf12563E9A8A27009e5Dbd0122F"],"counterpartyIdentifier":"indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c"}} start
2020-05-25T09:16:27.864Z [LockService] Releasing lock for chan_create:indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR at 1590398187864 with secret 7c70d419e9295c5010ebe590306b4b19
2020-05-25T09:16:27.866Z [LockService] Released lock for chan_create:indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR
2020-05-25T09:16:27.866Z [CF-RpcRouter] Processed chan_create method in 1735 ms
2020-05-25T09:16:27.867Z [ChannelService] create indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c finished: {"multisigAddress":"0x9041ec35D8df1A2d8A14b47522e3DaEE42e9C9a4"}
2020-05-25T09:16:27.867Z [MessagingInterface] Responded to indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c.channel.create in 1752 ms
2020-05-25T09:16:27.893Z [ChannelService] makeAvailable {"from":"indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR","type":"CREATE_CHANNEL_EVENT","data":{"multisigAddress":"0x9041ec35D8df1A2d8A14b47522e3DaEE42e9C9a4","owners":["0x627306090abaB3A6e1400e9345bC60c78a8BEf57","0x5ADf8eF9c2BB8cf12563E9A8A27009e5Dbd0122F"],"counterpartyIdentifier":"indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c"}} complete
2020-05-25T09:16:28.415Z [LinkedTransferService] getLinkedTransfersForReceiverUnlock for indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c started
2020-05-25T09:16:28.431Z [LinkedTransferService] getLinkedTransfersForReceiverUnlock for indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c complete: []
2020-05-25T09:16:28.589Z [LinkedTransferService] unlockLinkedTransfersFromUser for indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c started
2020-05-25T09:16:28.600Z [LinkedTransferService] unlockLinkedTransfersFromUser for indra52h8V6GBcEZzvdfRuLqWHrDCAnebdyjUAP4PBpEqcvHnLNEe4c complete: []
Signed 760-byte bearer authorization token for subject: indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ
2020-05-25T09:17:26.652Z [ChannelService] create indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ started
2020-05-25T09:17:26.662Z [LockService] Acquiring lock for chan_create:indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR at 1590398246662
2020-05-25T09:17:26.664Z [LockService] Acquired lock for chan_create:indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR with secret 81111864b1e2c5e7fe47ad51291f67be
2020-05-25T09:17:27.464Z [CF-SetupProtocol] [56d989ea-6d0b-4727-998e-ecfe6917aa91] Initiation started
2020-05-25T09:17:28.621Z [CF-SetupProtocol] [56d989ea-6d0b-4727-998e-ecfe6917aa91] Received responder's sigs in 1152 ms
2020-05-25T09:17:28.670Z [CF-SetupProtocol] [56d989ea-6d0b-4727-998e-ecfe6917aa91] Initiation finished in 1206 ms
2020-05-25T09:17:28.671Z [ChannelService] makeAvailable {"from":"indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR","type":"CREATE_CHANNEL_EVENT","data":{"multisigAddress":"0x4305FD095230C9C15Bb3b1eaBbCdBf685fD75C54","owners":["0x627306090abaB3A6e1400e9345bC60c78a8BEf57","0x9D971F720942131829546f6Be2F190D400cB63AC"],"counterpartyIdentifier":"indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ"}} start
2020-05-25T09:17:28.672Z [LockService] Releasing lock for chan_create:indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR at 1590398248672 with secret 81111864b1e2c5e7fe47ad51291f67be
2020-05-25T09:17:28.673Z [LockService] Released lock for chan_create:indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ,indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR
2020-05-25T09:17:28.673Z [CF-RpcRouter] Processed chan_create method in 2011 ms
2020-05-25T09:17:28.673Z [ChannelService] create indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ finished: {"multisigAddress":"0x4305FD095230C9C15Bb3b1eaBbCdBf685fD75C54"}
2020-05-25T09:17:28.673Z [MessagingInterface] Responded to indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ.channel.create in 2022 ms
2020-05-25T09:17:28.701Z [ChannelService] makeAvailable {"from":"indra8AXWmo3dFpK1drnjeWPyi9KTy9Fy3SkCydWx8waQrxhnW4KPmR","type":"CREATE_CHANNEL_EVENT","data":{"multisigAddress":"0x4305FD095230C9C15Bb3b1eaBbCdBf685fD75C54","owners":["0x627306090abaB3A6e1400e9345bC60c78a8BEf57","0x9D971F720942131829546f6Be2F190D400cB63AC"],"counterpartyIdentifier":"indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ"}} complete
2020-05-25T09:17:29.212Z [LinkedTransferService] getLinkedTransfersForReceiverUnlock for indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ started
2020-05-25T09:17:29.223Z [LinkedTransferService] getLinkedTransfersForReceiverUnlock for indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ complete: []
2020-05-25T09:17:29.522Z [LinkedTransferService] unlockLinkedTransfersFromUser for indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ started
2020-05-25T09:17:29.540Z [LinkedTransferService] unlockLinkedTransfersFromUser for indra5bAMYK7js1jhzxSzLq3bF2japK9DjYhmfhcbvBqBycP8WhPScZ complete: []
^C
ubuntu@nathan:~/indra$ bash ops/logs.sh node

```

```
chrishobcroftToday at 11:56 AM
Hi Connext folks, I hope you're all fine.

I'm looking for some advice on using Connext for mobile-to-mobile payments.

The use case is livestreaming, with one mobile device paying-to-play per second of content consumed, and the other device livestreaming-to-earn, publishing live video and watching their balance grow.

Specific question: is it possible to run Connext on both mobile devices, without needing a hub in the middle?
bohendoToday at 12:34 PM
Hi @chrishobcroft :wave:

    Specific question: is it possible to run Connext on both mobile devices, without needing a hub in the middle?

In theory: yes. These two mobiles will need to send signed messages back and forth though. This could be done in a truely p2p way via something like bluetooth but, for more practical applications, you'll probably want some kind of central messaging service that both clients can both connect to to share messages (or maybe we could use something like Whisper once it's ready?).

In practice: we haven't begun to support this yet so you'd be looking at a few months of full-time dev-work to create a custom bluetooth/shh messaging service & modifying our platform to support it.

My recommendation: Focus first on building a micro-payment-powered-streaming-service that uses either our hosted Indra node or one that you host yourself. This will be drastically simpler & once this is running smoothly, you'll have a better understanding of the platform & will be well prepared to start upgrading to a more pure p2p version w the same features.
chrishobcroftToday at 12:42 PM
The Consumer and Publisher would typically not be within Bluetooth range for each other.

Perhaps I should clarify - I want the Consumer app to pay the Publisher app over a network.

Both devices will be online, otherwise they would not be able to livestream to each other.

The Publisher is publishing live video (using this https://github.com/videoDAC/android/issues/31) and earning payments.

The Consumer is consuming live video (using this https://github.com/videoDAC/android/blob/master/README.mdhttps://github.com/videoDAC/android/blob/master/README.md) and sending payments.

If either goes offline from a video viewpoint, then it's also right that the money stops being sent.
GitHub
videoDAC/android
Repository for storing code, configuration and information about the applications developed by videoDAC for Android. - videoDAC/android
bohendoToday at 12:51 PM
How does a consumer find the publisher they're looking for? Seems like a central server would be required and, if that's the case, then I see no problem w hosting an intermediate payment node on that same server.
chrishobcroftToday at 12:57 PM
There is a central server processing video content.

I want to avoid running anything except video server software on that
I suppose it is possible to run a payment hub on that video server, but if it's avoidable, I would like to avoid it.
bohendoToday at 1:04 PM
It's very avoidable, you can just use the payment node that we're hosting ;)
Remember: the consumer & publisher need to be able to exchange signed messages. If you don't want to use any intermediate node at all, then you'll want to host your own messaging service regardless (eg adding a NATS service to your video server)
arjunToday at 1:05 PM
@chrishobcroft I would definitely recommend using our hosted node to bootstrap
We'll have the full network set up and running by the time you'd need to move off that anyway
and so then you can just make the switch fairly easily
chrishobcroftToday at 1:07 PM
OK, can someone share details of quick-start setup for this. Ideally with pre-requisites.
(and hi @arjun nice to connect, hope you're well!)
bohendoToday at 1:08 PM
Here's our quick-start guide: https://docs.connext.network/en/latest/userDocumentation/quickStart.html
Let us know if anything's unclear or missing :smiley:
chrishobcroftToday at 1:09 PM

    We'll have the full network set up and running by the time you'd need to move off that anyway

@arjun can you explain what this means? With a "full network", will I be able to pay from mobile to mobile without needing a server in the middle?
The thing is, we're ready now to start implementing mobile-to-mobile payments for livestreaming video.
If it needs a hub in the middle, it starts to weigh us down in terms of running infrastructure.
Perhaps I'm also completely misunderstanding Connext here BTW.
Perhaps it will always require me to run a server in the middle.
I'm woefully ill-educated on this whole thing
arjunToday at 1:12 PM
you can point at our hosted node for now so you dont have to run a node yourself
chrishobcroftToday at 1:12 PM
OK, so, this means I am dependent on you. How is your uptime incentivised?
what's your cut? how does it work? sorry for my ignorance.
arjunToday at 1:13 PM
eventually, other nodes in the network will be able to route payments to each other -- then you can just rely on the network as a whole to route payments (though you may eventually want to run a node regardless just for your own business model/user experience)
ATM, we dont have a ton of volume through the hosted node we're just doing it for free. If we start to see usage, we'll subsidize our cost of operating it using microfees collected from the user's transaction. We're not really looking to make money on the node, our business model is more focused on the network as a whole
chrishobcroftToday at 1:14 PM
OK. And is there any way to run this without needing to get into npm or yarn? Like, good old-fashioned pre-compiled binaries?
arjunToday at 1:15 PM
uhh not at the moment. You have to run a typescript client somewhere
bohendoToday at 1:15 PM
Uhh actually yes there is but our compilation target is a docker image, not a binary
chrishobcroftToday at 1:15 PM
Hmm, this is a barrier-to-entry for me.
docker, yarn, npm - all barriers to entry :/
bohendoToday at 1:16 PM
If you can run docker images, you can run our stuff w/out needing to fiddle w npm
rahulToday at 1:16 PM
is there a way to precompile node.js programs?
i dont think so right
bohendoToday at 1:16 PM
maybe w wasm eventually
But that still needs it's own runtime env so not exactly the same as a bin
chrishobcroftToday at 1:17 PM
Where are docker instructions?
arjunToday at 1:17 PM
client also compiles to a docker image?
I didnt know that
I was assuming we're only shipping it as a webpacked npm bundle
bohendoToday at 1:19 PM
Well the bot isn't too far from a dockerized client. If anyone needed something like that it'd be trivial to provide it to them (but not a very common use case so prob won't be needed)
@chrishobcroft If you want to run your own payment node on some server, see this guide: https://github.com/connext/indra#deploy-an-indra-node-to-production
& to set up clients that will connect to some other node (eg on your mobile devices), see: https://docs.connext.network/en/latest/userDocumentation/quickStart.html
arjunToday at 1:24 PM
Simplest way to get started is the second link -- you should be able to do something like:

const channel = await connext.connect("rinkeby")
await channel.deposit({...})
await channel.transfer({...})

chrishobcroftToday at 1:41 PM
I don't understand. I'm just looking for a simple way to spin up a Connext node on a clean Ubuntu server.

Seems like the minimum entry requirement for this would be to learn docker, which sounds like it's not broadly supported across the ecosystem. So then if I want to follow the default path, I need to understand npm also. Maybe npm is a lower barrier to entry than docker. Hmm.
arjunToday at 1:43 PM
Connext node == hub
if you want to spin up your own node, then you definitely need docker (this is pretty well supported everywhere)
if you want to connect to existing nodes, you need a client which can come in either npm or docker variants
bohendoToday at 1:45 PM
TL;DR: install make and docker and git on the clean Ubuntu server (we provide a helper script at indra/ops/setup-ubuntu.sh to do this for you) then:

git clone git@github.com:connext/indra.git
cd indra
make start

^ This will run a Connext node against a local eth testnet. If you want to run against mainnet then you'll want to modify a .env file w the eth provider endpoint.
As a user, there's not much you need to learn about docker other than how to install it. It's used to abstract away the ops/env complexity so that you never need to worry about stuff like which version of node is installed or where dependencies need to be.
A Connext node needs docker to run (and, internally, it uses npm to manage deps but, thanks to docker you don't need to know/care about that).
A Connext client generally does not need docker at all (but it could if helpful), it's primarily used as a JS library that can be plugged into any JS project (including react native for mobile apps)
chrishobcroftToday at 2:02 PM
And which ports do I need to open to the outside world so that clients can connect?
bohendoToday at 2:04 PM
80, 443, 4221, 4222
however, docker actually takes care of quite a bit of firewall management too so you shouldn't need to worry about which ports to expose unless you're using AWS which includes firewall controls that are not accessible from w/in the server itself
chrishobcroftToday at 2:07 PM

ubuntu@cibulka:~$ git clone git@github.com:connext/indra.git
Cloning into 'indra'...
The authenticity of host 'github.com (140.82.118.4)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.118.4' (RSA) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
ubuntu@cibulka:~$ git clone git@github.com:connext/indra.git
Cloning into 'indra'...
Warning: Permanently added the RSA host key for IP address '140.82.118.3' to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
ubuntu@cibulka:~$ git clone git@github.com:connext/indra.git
Cloning into 'indra'...
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

So, I tried that... but github is throwing some complaint
rahulToday at 2:09 PM
try git clone https://github.com/connext/indra.git
chrishobcroftToday at 2:10 PM
oh right. Yes.
OK, so, this is looking like it might end up being remarkably easy.
bohendoToday at 2:17 PM
Did docker installation go well? check w docker --version
chrishobcroftToday at 2:18 PM
I see that sudo apt install docker doesn't work, it should be sudo apt install docker.io or sudo snap install docker
trying

ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e36e2ea4....0a86" make start
=============
[Makefile] => Start building builder
docker build --file ops/builder/Dockerfile --build-arg SOLC_VERSION=0.5.11   --tag indra_builder ops/builder
ERRO[0000] failed to dial gRPC: cannot connect to the Docker daemon. Is 'docker daemon' running on this host?: dial unix /var/run/docker.sock: connect: permission denied 
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/build?buildargs=%7B%22SOLC_VERSION%22%3A%220.5.11%22%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=uinr8yzao77xr2si1hhrlj1mu&shmsize=0&t=indra_builder&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
make: *** [Makefile:216: builder] Error 1

bohendoToday at 2:20 PM
Check out how our setup script does it: https://github.com/connext/indra/blob/staging/ops/setup-ubuntu.sh#L129
GitHub
connext/indra
Monorepo containing everything related to the core Connext protocols and network. - connext/indra
^ this line might be the one you're missing systemctl enable docker
chrishobcroftToday at 2:21 PM
you guys are getting me to run docker I love you
arjunToday at 2:21 PM
:heart_eyes:
we love docker here, it really simplifies everything a lot once it's set up
chrishobcroftToday at 2:22 PM
OK, next roadblock:

ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e..........cc340a86" make start
=============
[Makefile] => Start building builder
docker build --file ops/builder/Dockerfile --build-arg SOLC_VERSION=0.5.11   --tag indra_builder ops/builder
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/build?buildargs=%7B%22SOLC_VERSION%22%3A%220.5.11%22%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=oypomnoexyqrbrwok6320c475&shmsize=0&t=indra_builder&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
make: *** [Makefile:216: builder] Error 1

bohendoToday at 2:23 PM
I bet this line is the one missing: https://github.com/connext/indra/blob/staging/ops/setup-ubuntu.sh#L128
usermod -aG docker $user
GitHub
connext/indra
Monorepo containing everything related to the core Connext protocols and network. - connext/indra
chrishobcroftToday at 2:23 PM
let's try it
this is fun
bohendoToday at 2:24 PM
(sub $user for your username btw)
chrishobcroftToday at 2:24 PM

ubuntu@nathan:~/indra$ usermod -aG docker $ubuntu
Usage: usermod [options] LOGIN

Options:
  -b, --badnames                allow bad names
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the
                                new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remove range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remove range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account

OK, I got it I think
bohendoToday at 2:25 PM
Cool, might need sudo tho :thinking: sudo usermod -aG docker ubuntu
chrishobcroftToday at 2:25 PM
sudo usermod -aG docker ubuntu
Still this:

ubuntu@nathan:~/indra$ INDRA_ETH_PROVIDER="https://rinkeby.infura.io/v3/e7813e..........cc340a86" make start
=============
[Makefile] => Start building builder
docker build --file ops/builder/Dockerfile --build-arg SOLC_VERSION=0.5.11   --tag indra_builder ops/builder
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/build?buildargs=%7B%22SOLC_VERSION%22%3A%220.5.11%22%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&session=oypomnoexyqrbrwok6320c475&shmsize=0&t=indra_builder&target=&ulimits=null&version=1: dial unix /var/run/docker.sock: connect: permission denied
make: *** [Makefile:216: builder] Error 1

aha, a server reboot helped a lott
Did you guys just get me running indra in docker?

[Makefile] => Start building node-modules
docker run --name=indra_builder --interactive --tty --rm --volume=/home/ubuntu/indra:/root indra_builder 1000:1000 "lerna bootstrap --hoist --no-progress"
Running command as 0:0 (target user: 1000:1000)
lerna notice cli v3.20.2
lerna info Bootstrapping 15 packages
lerna WARN EHOIST_PKG_VERSION "bots" package depends on ts-node@8.10.1, which differs from the hoisted ts-node@8.9.0.
lerna WARN EHOIST_PKG_VERSION "@connext/cf-core" package depends on typescript-memoize@1.0.0-alpha.3, which differs from the hoisted typescript-memoize@^1.0.0-alpha.3.
lerna info Installing external dependencies
lerna info hoist Installing hoisted dependencies into root
lerna info hoist Pruning hoisted dependencies
lerna info hoist Finished pruning hoisted dependencies

Does someone want to help me script this thing up, from a clean Ubuntu 20.04 to a node running indra on docker?
bohendoToday at 2:30 PM
Not just running. Building from source and then running :sunglasses:
chrishobcroftToday at 2:31 PM
That's the beauty
bohendoToday at 2:31 PM

    Does someone want to help me script this thing up, from a clean Ubuntu 20.04 to a node running indra on docker?

99% of this script is already written: https://github.com/connext/indra/blob/staging/ops/setup-ubuntu.sh
Only thing missing is running make start (or configuring env vars then running make start-prod)
GitHub
connext/indra
Monorepo containing everything related to the core Connext protocols and network. - connext/indra
chrishobcroftToday at 2:31 PM
Not just building from source and then running, but downloading master from github, then building from source and then running
Looks like that script assumes all the source has been pulled already?
Would be nice if it did a git clone ... or a refresh, or whatever - then it's a single file script to download and run, and come back later
bohendoToday at 2:34 PM
Yea, this script is made to be run on local machine so you'd still need to clone first:

git clone https://github.com/connext/indra.git
cd indra
bash ops/setup-ubuntu.sh $SERVER_IP
ssh ubuntu@$SERVER_IP bash -c "cd indra && make start"

Almost a one-liner :laughing:
chrishobcroftToday at 2:36 PM
I understand. If it could be a one-liner, I could install it with systemd to start on boot.
Which could update from master, build from source, then run...
Like I do with prysm.sh from here https://prylabs.net/participate
Prysmatic Labs
Ethereum 2.0 - Phase 0 Testnet
The Topaz Testnet is a public network created by Prysmatic Labs. It implements the Ethereum 2.0 Phase 0 protocol for a proof-of-stake blockchain enabling anyone holding Goerli test ETH to join!
OK, hit a new blocker:

INDRA_UI=daicard bash ops/start-dev.sh
Swarm initialized: current node (lcyave24ryn800vtzlzd4wn9q) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-1wb7k0k3ixbjfqtb15wrzenv3xh7p3iuyjero3iojdnbh55ypy-f2ialjh480ozsq2uox1ouda40 89.145.161.66:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

Fetching chainId from https://rinkeby.infura.io/v3/e7813e36e2e......40a86
ops/start-dev.sh: line 72: jq: command not found
(23) Failed writing body
ops/start-dev.sh: line 75: jq: command not found
Failed to fetch chainId from provider https://rinkeby.infura.io/v3/e7813e36e.....6fcc340a86
make: *** [Makefile:56: start-daicard] Error 1

bohendoToday at 2:39 PM
Oops one more dep: sudo apt install jq
chrishobcroftToday at 2:39 PM
That's the story of my life sometimes...
one more dep...
OK, I made it this far:

Waiting for the indra stack to wake up... Good Morning!

What now?
(1 line... :cool_but_depressed_but_cool: )
bohendoToday at 2:42 PM
It's alive! make dls to get a health-check
Each service should have 1/1 replicas, bash ops/logs.sh node to investigate the node, for example, if something's wrong w it
Otherwise, browse to it's $SERVER_IP:3000 to check out the example UI
chrishobcroftToday at 2:44 PM
OK, so, the Android app that's able to send payments to this... which port is it connecting to?
Also, I configured it with Rinkeby, but it's retrieving Uniswap data from mainnet:

Got swap rate from Uniswap at block 10134052

bohendoToday at 2:47 PM
Btw make start starts up a version optimized for development so it exposes stuff on port 3000 instead of 80/433. You can try out make start-prod for production (this won't build, rather it will pull docker images based on the current git commit/tag and just run them)

    Also, I configured it with Rinkeby, but it's retrieving Uniswap data from mainnet:

Yea, swap rate isn't really a meaningful concept for rinkeby-eth so we just use the mainnet number for everything
chrishobcroftToday at 2:48 PM
I understand - is it polling a Uniswap API for this though? Or is there a hardcoded mainnet URL being used somewhere?
bohendoToday at 2:50 PM
I believe it's using ethers.getDefaultProvider("homestead") :thinking:
rahulToday at 2:50 PM
swap rate is polling Uniswap API
chrishobcroftToday at 2:51 PM
OK, phew.
rahulToday at 2:52 PM
this is configurable to be turned off if you dont need swaps
```
