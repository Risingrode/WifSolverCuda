# WifSolverCuda v3.0
This is a modified version of WifSolverCuda v0.5.0 by [PawGo](https://github.com/PawelGorny) </br>
This is a soft for recovering unknown (lost) characters in the middle and beginning of a (Wallet Import Format) private key. </br>
If you have a lost WIF private key end (on the right), use soft [**Wif key Recovery**](https://github.com/phrutis/Wif-key-Recovery)

Help page: ```WifSolverCuda.exe -h```
```
C:\Users\User>WifSolverCuda.exe -h

 WifSolverCuda v3.0 (phrutis modification 10.04.2022)

-wif         START WIF key 5.... (51 characters) or L..., K...., (52 characters)
-wif2        END WIF key 5.... (51 characters) or L..., K...., (52 characters)
-a           Bitcoin address 1.... or 3.....
-n           Letter number from left to right from 9 to 51
-n2          Spin additional letters -n2 from 9 to 51 (every sec +1)
-turbo       Quick mode (skip 3 identical letters in a row) -turbo 3 (default: OFF)
-part1       First part of the key starting with K, L or 5 (for random mode)
-part2       The second part of the key with a checksum (for random mode)
-fresult     The name of the output file about the find (default: FOUND.txt)
-fname       The name of the checkpoint save file to continue (default: GPUid + Continue.txt)
-ftime       Save checkpoint to continue every sec (default 60 sec)
-d           DeviceId. Number GPU (default 0)
-list        Shows available devices
-h           Shows help page
 ```   

## How to use it

The Compressed WIF key must span K... or L... contain 52 characters.</br>
The Uncompressed WIF key must span 5... contain 51 characters.</br>

Example WIF key: KyBLV6rrV9hsbsU96VwmEtMnACavqnKnEi7_________J9tM5JQQSo</br>
Replace unknown (missing) characters in a row with a capital ```X``` (min. 4, max. 15 X)</br>
We collect the key: KyBLV6rrV9hsbsU96VwmEtMnACavqnKnEi7XXXXXX```X```J9tM5JQQSo (52)</br>
We need to twist the first unknown letter ```X```, this symbol is number 11</br>
Minimum position -n 9 (-n 51 max, 1-8 this is the checksum it can't be rotated)</br>

Run: ```WifSolverCuda.exe -wif KyBLV6rrV9hsbsU96VwmEtMnACavqnKnEi7XXXXXXXJ9tM5JQQSo -n 11 -a 1EpMLcfjKsQCYUAaVj9mk981qkmT5bxvor```

![private key WIF](https://user-images.githubusercontent.com/82582647/162636370-bbdbd196-209e-4546-a4e4-87f7ace2a4b4.png)

## Performance

| card          | compressed with collision | all other cases |
|---------------|---------------------------|-----------------|
| RTX 3090      | 29 Gkey/s                 | 4.0 Gkey/s      |
| RTX 3080 Ti   | 29 Gkey/s                 | 4.0 Gkey/s      |
| RTX 3060 eGPU | 10 Gkey/s                 | 1.3 Gkey/s      |
| RTX 2070      | 12 Gkey/s                 | 1.4 Gkey/s      |
| GTX 1080TI    | 6 Gkey/s                  | 700 Mkey/s      |

If the speed is several times higher than in the table. Your WIF key is not correct.</br>

If you need help recovering your private key, contact [telegram](https://t.me/+mAY1x5YYuL8yNjQy) </br>

## Сontinuation
Сontinuation of the last checkpoint from the file Сontinuation.txt</br>
Run: ```WifSolverCuda.exe -wif KyBLV6rrV9hsbsU96VwmEtMnACavqnKnAaZvihMARQJ9tM5JQQSo -wif2 L5oLkpV3aqBjhki6LmvChTCV6odsp4SXM6FfU2Gppt5kFLaHLuZ9 -a 1XXXLcfjKsQCYUAaVj9mk981qkmT5bxvor -n 11 -turbo 3 -d 0```

![Сontinuation recovery bitcoin](https://user-images.githubusercontent.com/82582647/162636418-9c46211a-a266-44d3-abf4-fef6d8b64c92.png)

## TURBO MODE
 - [How Turbo mode works?](https://github.com/phrutis/WifSolverCuda/blob/main/Other/turbo.md#how-turbo-mode-works) </br>
 
 Run: ```WifSolverCuda.exe -wif KyBLV6rrV9hsbsU96VwmEtMnACavqnKnAaZb5xcbaaJ9tM5JQQSo -n 11 -a 1XXXLcfjKsQCYUAaVj9mk981qkmT5bxvor -turbo 3```
 
![recovery part private key](https://user-images.githubusercontent.com/82582647/162636457-25a10c34-0f7c-4554-ae96-3633e47ff796.png)

## Special mode
### Search for additional letter -n2
- [How is the extra letter rotated](https://github.com/phrutis/WifSolverCuda/blob/main/Other/turbo.md#how-is-the-extra-letter-rotated) </br>

Run: ```WifSolverCuda.exe -wif KyBLV6rrV9hsbsU961wmEtMnACavqnKnEi7eY11111J9tM5JQQSo -n 11 -n2 35 -a 1EpMLcfjKsQCYUAaVj9mk981qkmT5bxvor```

![paper wallet missing](https://user-images.githubusercontent.com/82582647/162636581-4aa135ec-d84a-4630-811c-32e1fe9c9d19.png)

## Random mode
### Random + sequential search
- [How random works](https://github.com/phrutis/WifSolverCuda/blob/main/Other/turbo.md#how-random-works)

Run: ```WifSolverCuda.exe -part1 KyBLV6rrV9hsbsU96VwmEtMnACavqnKnEi7 -part2 J9tM5JQQSo -a 1EpMLcfjKsQCYUAaVj9mk981qkmT5bxvor```

![missing characters private key](https://user-images.githubusercontent.com/82582647/162636844-84366745-19ff-49ec-8052-c8e678b96170.png)


## Build
### Windows:

#### Microsoft Visual Studio Community 2019
RTX 30xx - CUDA version [**11.6**](https://developer.nvidia.com/cuda-11-6-0-download-archive) compute_cap=86 use the prepared file WifSolverCuda.vcxproj from the Other folder.</br>
For others GPUs - CUDA version [**10.2**](https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exenetwork) compute_cap=54 </br>

### Linux:
Go to linux/ subfolder and execute _make all_. If your device does not support compute capability=86 (error "No kernel image is available for execution on the device"), do the change in _Makefile_ (for example 1080Ti requires COMPUTE_CAP=61).

### Description of the diary

Teddy came up to me (American)</br>
The story is this:

He worked for an Indian e-currency exchange company.

He attached links to the site of an Indian crypto exchange company and others.

They had a miner client, he often cashed btc through them.</br>
Once he turned to them with a request to restore the key in a burnt diary.

At that time it was 2017-2018. The btc rate was low, the search for the key was unprofitable.</br>
There were no programs for recovering a key on a GPU yet.</br>
The GPU cards were weaker than the GTX 1080.</br>
Perhaps there was a program on the processor, but it would take millions of years to find the key on it.

The photo in the diary changed hands and came to me in the spring of 2022 from Taddi.</br> 
For this task, the WifSolverCuda program was made. A [challenge](https://github.com/jonlloner/wif500) was made on its modification.

About the [challenge](https://github.com/jonlloner/wif500):</br>
The large range has been split into 3364 ranges (58*58)</br>
The challenge began, the passage of the ranges was recorded by screenshots and [recorded in the table manually](https://github.com/jonlloner/wif500/blob/main/x64/Release/table.md).</br> 
Since % of the pool was relied on for participation. Hunters began to fake screenshots of the passage.</br>
They were hoping someone else would find it and they would get a big % of the pool.</br>
I protected the fake with a hex id. It didn't work, because with the big hash step they faked the walkthrough again.</br>
As a result, some hunters spent their money, others forged. As a result, 2000 ranges passed, of which half are fake.</br> 
Several people confessed to falsifying the results. The challenge has been stopped.</br>

Now the search happens in the pool, which has many advantages:</br>
The search speed is several times higher.</br>
There is support for multigpu.</br>
Accurate statistics</br>
There are no fakes, how much passed, so much is your% of the pool.

Addition:
One of the hunters contacted the owner of the diary to find out more. information from the owner. The owner is from India.</br>
According to him, the diary is real. As evidence, he made inscriptions on the inside of the diary and a new photograph of the key.</br>
See photos [**here**](https://user-images.githubusercontent.com/125174641/218313586-287f080e-9bc8-4f06-8b7d-0a2fbceb8743.jpg) and [**here**](https://user-images.githubusercontent.com/125174641/218313489-b1d8ab4e-8d3a-4d6a-b584-d15fcdf7127e.jpg)</br>
The hunter wanted to clarify the symbols of the key and got a better picture of the desired part:</br>
![333](https://user-images.githubusercontent.com/125174641/218313615-e0a09020-6a43-44d8-8511-6624e636f6cb.jpg)</br>

According to the hunter, the owner stopped making contact. phrutis has no owner contact</br>

Don't waste your time, join the search
