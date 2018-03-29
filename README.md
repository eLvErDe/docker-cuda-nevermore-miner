# nvidia-docker image for brian112358 ccminer fork aka nevermore-miner

These images use [brian112358 nevermore-miner] from my own [Debian/Ubuntu mining packages repository].
It requires a CUDA compatible docker implementation so you should probably go
for [nvidia-docker].
It has also been tested successfully on [Mesos] 1.4.

It has been built with **0% dev fees**.

## x16r (Raven) and x16s (Pigeon) variants

Default variant handle X16R algorithm, the other supports X16S.
You may also build a container with CUDA 8 support by replacing "cuda9" by "main" in Dockerfiles.
Packages are also available for a wide range of Debian/Ubuntu version and their provided CUDA version so you can easily build an image based on a different distribution.

## -api variants

A new variant has been introduced:
It provides the PHP files connecting the TCP api and embed Apache2+PHP as well as a Python wrapper I made myself to start both Apache2 and CCMiner.

```
nvidia-docker run -dt --restart=unless-stopped -p 4068:4068 -p 4069:4069 --name cuda-rvn-nevermore-miner acecile/cuda-nevermore-miner-cuda9-api /root/multi-process-launcher/multi-process-launcher.py --cmd '/root/start-apache2.sh 4068 4069' --cmd '/usr/bin/ccminer -a x16r -o stratum+tcp://xvg-lyra.suprnova.cc:6667 -u greenie.catch-all -p x --api-bind=0.0.0.0:4068 --api-allow=0/0'
```

You can now access status page at http://you.host:4069 and live hashrate graphs at http://you.host:4069/websocket.htm (yes, .htm it's not a typo).

## Build images

```
git clone https://github.com/eLvErDe/docker-cuda-nevermore-miner
cd docker-cuda-nevermore-miner
docker build . -t cuda-nevermore-miner-cuda9 -f Dockerfile.cuda9
docker build . -t cuda-nevermore-miner-cuda9-api -f Dockerfile.cuda9.api
docker build . -t cuda-nevermore-miner-cuda9-x16s -f Dockerfile.cuda9.x16s
docker build . -t cuda-nevermore-miner-cuda9-x16s-api -f Dockerfile.cuda9.x16s.api

```

## Publish it somewhere

```
docker tag cuda-nevermore-miner-cuda9 docker.domain.com/mining/cuda-nevermore-miner-cuda9
docker push docker.domain.com/mining/cuda-nevermore-miner-cuda9

```

## Test it (using dockerhub published image)

```
nvidia-docker pull acecile/cuda-nevermore-miner-cuda9:latest
nvidia-docker pull acecile/cuda-nevermore-miner-x16s-cuda9:latest
nvidia-docker run -it --rm acecile/cuda-nevermore-miner-cuda9 /usr/bin/ccminer --help
nvidia-docker run -it --rm acecile/cuda-nevermore-miner-x16s-cuda9 /usr/bin/ccminer --help
```

An example command line to mine Raven on SuprNova (ccminer supports nearly all algorythm so check its documentation and picks what you want):
```
nvidia-docker run -it --rm -p 4068:4068 --name cuda-rvn-nevermore-miner acecile/cuda-nevermore-miner-cuda9 /usr/bin/ccminer -a x16r -o stratum+tcp://xvg-lyra.suprnova.cc:6667 -u greenie.catch-all -p x --api-bind=0.0.0.0:4068 --api-allow=0/0
```

To mine Pigeon on BlockCruncher
```
nvidia-docker run -it --rm -p 4068:4068 --name cuda-rvn-nevermore-miner-x16s acecile/cuda-nevermore-miner-cuda9-x16s /usr/bin/ccminer -a x16s -o stratum+tcp://blockcruncher.com:3366 -u PDr9BSz1o43gZyJAS749Yfd3WortNS7eDM -p c=PGN --api-bind=0.0.0.0:4068 --api-allow=0/0
```

Ouput will looks like:
```
*** nevermore 0.1 for nVidia GPUs by brian112358@github ***
    Built with the nVidia CUDA Toolkit 9.0 64-bits

  Originally based on Christian Buchner and Christian H. project
  Include some kernels from alexis78, djm34, djEzo, tsiv and krnlx.

No dev donation set. Please consider making a one-time donation to the following addresses:
BTC donation address: 1AJdfCpLWPNoAMDfHF1wD5y8VgKSSTHxPo (tpruvot)

BTC donation address: 1FHLroBZaB74QvQW5mBmAxCNVJNXa14mH5 (brianmct)
PGN donation address: RWoSZX6j6WU6SVTVq5hKmdgPmmrYE9be5R (brianmct)

[2018-03-29 17:28:43] Starting on stratum+tcp://blockcruncher.com:3366
[2018-03-29 17:28:43] NVML GPU monitoring enabled.
[2018-03-29 17:28:43] 4 miner threads started, using 'x16s' algorithm.
[2018-03-29 17:28:44] API open in full access mode to 0/0 on port 4068
[2018-03-29 17:29:01] Stratum difficulty set to 16 (0.06250)
[2018-03-29 17:29:01] x16s block 28334, diff 4205.612
[2018-03-29 17:29:01] GPU #2: Intensity set to 20, 1048576 cuda threads
[2018-03-29 17:29:01] GPU #0: Intensity set to 20, 1048576 cuda threads
[2018-03-29 17:29:01] GPU #3: Intensity set to 20, 1048576 cuda threads
[2018-03-29 17:29:01] GPU #1: Intensity set to 20, 1048576 cuda threads
[2018-03-29 17:29:07] hash order 859B37F10E246ACD (5abd225b)
[2018-03-29 17:29:07] GPU #2: Gigabyte GTX 1080 Ti, 164.52 kH/s
[2018-03-29 17:29:07] GPU #0: Gigabyte GTX 1080 Ti, 164.39 kH/s
[2018-03-29 17:29:07] GPU #3: Gigabyte GTX 1080 Ti, 164.35 kH/s
[2018-03-29 17:29:07] GPU #1: GeForce GTX 1080, 164.02 kH/s
[2018-03-29 17:29:23] GPU #1: GeForce GTX 1080, 11.71 MH/s
[2018-03-29 17:29:23] accepted: 1/1 (diff 0.098), 29.16 MH/s yes!
[2018-03-29 17:29:27] x16s block 28335, diff 4205.612
[2018-03-29 17:29:27] hash order C940AE71FD23568B (5abd2277)
[2018-03-29 17:29:32] GPU #3: Gigabyte GTX 1080 Ti, 15.52 MH/s
[2018-03-29 17:29:33] accepted: 2/2 (diff 0.121), 31.93 MH/s yes!
[2018-03-29 17:29:37] GPU #0: Gigabyte GTX 1080 Ti, 15.69 MH/s
[2018-03-29 17:29:37] GPU #0: 1606 MHz 64.58 kH/W 155W 52C FAN 0%
[2018-03-29 17:29:37] accepted: 3/3 (diff 0.063), 34.76 MH/s yes!
```

## Background job running forever

```
nvidia-docker run -dt --restart=unless-stopped -p 4068:4068 --name cuda-rvn-nevermore-miner-x16s acecile/cuda-nevermore-miner-cuda9-x16s /usr/bin/ccminer -a x16s -o stratum+tcp://blockcruncher.com:3366 -u PDr9BSz1o43gZyJAS749Yfd3WortNS7eDM -p c=PGN --api-bind=0.0.0.0:4068 --api-allow=0/0
```

You can check the output using `docker logs cuda-rvn-nevermore-miner-x16s -f` 

## Use it with Mesos/Marathon

Edit `mesos_marathon.json` to replace miner parameter, change application path as well as docker image address (if you dont want to use public docker image provided).
Then simply run (adapt application name here too):

```
curl -X PUT -u marathon\_username:marathon\_password --header 'Content-Type: application/json' "http://marathon.domain.com:8080/v2/apps/mining/cuda-rvn-nevermore-miner?force=true" -d@./mesos\_marathon.json
```

You can check CUDA usage on the mesos slave (executor host) by running `nvidia-smi` there:

```
Thu Mar 29 19:40:13 2018       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 390.25                 Driver Version: 390.25                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    On   | 00000000:03:00.0 Off |                  N/A |
| 52%   79C    P2   183W / 200W |   1357MiB /  8119MiB |     93%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX 108...  On   | 00000000:04:00.0 Off |                  N/A |
| 34%   63C    P2   186W / 200W |   1395MiB / 11178MiB |     97%      Default |
+-------------------------------+----------------------+----------------------+
|   2  GeForce GTX 108...  On   | 00000000:05:00.0 Off |                  N/A |
| 35%   64C    P2   188W / 200W |   1395MiB / 11178MiB |    100%      Default |
+-------------------------------+----------------------+----------------------+
|   3  GeForce GTX 108...  On   | 00000000:06:00.0 Off |                  N/A |
| 32%   62C    P2   163W / 200W |   1395MiB / 11178MiB |     98%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1970      C   /usr/bin/ccminer                            1339MiB |
|    1      2016      C   /usr/bin/ccminer                            1377MiB |
|    2      2002      C   /usr/bin/ccminer                            1377MiB |
|    3      2020      C   /usr/bin/ccminer                            1377MiB |
+-----------------------------------------------------------------------------+
```
