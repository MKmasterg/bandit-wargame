## Bandit Level 13 — Unwrapping Layers of Encodings

### Description

In this level, we’re told that the password is stored in the file `data.txt`, but it has been **encoded multiple times** using various compression and archiving formats.
Our task is to **unwrap each layer** to reach the final password.

### Walkthrough

As usual, we start in the home directory. To keep things clean, let’s work inside a temporary directory:

```bash
bandit12@bandit:~$ mktemp -d
/tmp/tmp.7qLgxgUPkV
```

Copy the file and move into the directory:

```bash
bandit12@bandit:~$ cp data.txt /tmp/tmp.7qLgxgUPkV
bandit12@bandit:~$ cd /tmp/tmp.7qLgxgUPkV
```

Inspect the file:

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data.txt
data.txt: ASCII text
```

---

### Layer 0: Hex to Binary

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ mv data.txt data
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ xxd -r data > data.bin
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ ls
data  data.bin
```

---

### Layer 1: Gzip

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data.bin
data.bin: gzip compressed data, was "data2.bin" ...
```

Try decompressing:

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ gzip -d data.bin
gzip: data.bin: unknown suffix -- ignored
```

Fix the name:

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ mv data.bin data1.gz
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ gzip -d data1.gz
```

---

### Layer 2: Bzip2

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data1
data1: bzip2 compressed data
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ bzip2 -d data1
bzip2: Can't guess original name for data1 -- using data1.out
```

---

### Layer 3: Gzip

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data1.out
data1.out: gzip compressed data
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ mv data1.out data4.gz
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ gzip -d data4.gz
```

---

### Layer 4: Tar

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data4
data4: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ tar -xf data4
```

---

### Layer 5: Tar Again

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ tar -xf data5.bin
```

---

### Layer 6: Bzip2

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data6.bin
data6.bin: bzip2 compressed data
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
```

---

### Layer 7: Tar Again

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ tar -xf data6.bin.out
```

---

### Layer 8: Gzip

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data8.bin
data8.bin: gzip compressed data
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ mv data8.bin data8.gz
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ gzip -d data8.gz
```

Final check:

```bash
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ file data8
data8: ASCII text
bandit12@bandit:/tmp/tmp.7qLgxgUPkV$ cat data8
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

## Password

```
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

### Note
This level was all about understanding how different compression and archiving formats work together. By carefully inspecting each layer and using the appropriate tools to decompress and extract the files, we were able to retrieve the final password.
If you noticed `file` was a handy tool throughout this process; also might not be bad to learn more about `xxd` and `magic number`s.