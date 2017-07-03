# Homestead Troubleshooting


## bash.init Error

When running the `bash init.sh` script to initialize Homestead, you _may_ run into a cryptic bash error similar to the one below:

```shell
$ bash
  dyld: Library not loaded: /usr/local/opt/readline/lib/libreadline.6.dylib
  Referenced from: /usr/local/bin/bash
  Reason: image not found
Trace/BPT trap: 5
```

If so, you need to upgrade your bash via Homebrew using the following commands:

```shell
$ brew update

$ brew doctor

$ brew upgrade bas
```
