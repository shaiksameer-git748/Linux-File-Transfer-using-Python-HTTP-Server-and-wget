# Linux-File-Transfer-using-Python-HTTP-Server-and-wget
# Linux File Transfer using Python HTTP Server and wget

## Goal

Transfer a file from Machine A to Machine B.

---

## Machine A (Sender)

Navigate to the directory containing the file:

```bash
cd ~/picoctfs
```

Check the file:

```bash
cat hash.txt
```

Start an HTTP server:

```bash
python3 -m http.server 80
```

This shares all files in the current directory over HTTP.

---

## Machine B (Receiver)

Move to the location where the file should be saved:

```bash
cd /tmp
```

Download the file:

```bash
wget http://192.168.90.104/hash.txt
```

OR

```bash
wget http://192.168.90.104/hash.txt -O hash.txt
```

Verify the contents:

```bash
cat hash.txt
```

---

## Common Mistake

Wrong:

```bash
wget http://192.168.90.104/
```

Result:

* Downloads the directory listing page.
* Saves HTML instead of the actual file.

Example output:

```html
<!DOCTYPE HTML>
<html>
<title>Directory listing</title>
...
</html>
```

Reason:

* The URL points to the web server root, not a specific file.

---

## Another Mistake

If wget shows:

```text
301 Moved Permanently
Location: /sample.txt/
```

This means:

```text
sample.txt/
```

is a directory, not a file.

wget downloads the directory listing HTML page.

To download the actual file, specify its full path:

```bash
wget http://IP/folder/file.txt
```

Example:

```bash
wget http://192.168.90.104/sample.txt/sample.txt
```

---

## What Happens During Transfer?

Before:

Machine A:

```text
hash.txt
└── c20fa16907343eef642d10f0bdb81bf629e6aaf6c906f26eabda079ca9e5ab67
```

Machine B:

```text
(No file)
```

After wget:

Machine A:

```text
hash.txt
```

Machine B:

```text
hash.txt
```

Both files contain the same data.

The original file remains on Machine A.

A copy is created on Machine B.

---

## Key Concept

python3 -m http.server
= Shares files

wget
= Downloads files

Correct flow:

Machine A
(hash.txt)
↓
python3 -m http.server
↓ HTTP
wget
↓
Machine B
(hash.txt)
