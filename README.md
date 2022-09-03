# GPG
---

This repository demonstrates the use of GPG.

We'll simulate 2 parties: bob and alice.
Every time you perform actions as one of them, you need to set the `GNUPGHOME` enviroment
variable to their directory.

## Bob and Alice Simulation
We'll do a simulation in which we have 2 persons: Bob and Alice.
In this simulation, Alice sends an encrypted message to Bob.

### Generate a key pair

We'll start working with bob.

```
$ export GNUPGHOME=$PWD/bob
$ gpg --full-generate-key
```

Answer the questions. For email, write `bob@example.com`
After this, you'll see that the `bob` directory is filled with some files.
This is bob's private key, plus some other stuff.

### Export Bob's public key

Now we want to export Bob's public key so that Alice can import it.
```
$ gpg --output bob.key --export bob@example.com
```

### Import Bob's public key
---

Now, as Alice, let's import Bob's public key.
```
$ export GNUPGHOME=$PWD/alice
$ gpg --import bob.key
```

### Encrypt a message

Now, as Alice, let's encrypt a message with Bob's public key:
```
$ gpg --encrypt -r bob@example.com message.txt
```

This will generate a `message.txt.gpg` file, which is the encrypted message.
The `-r` stands for `receipient`, and it tells gpg with what public key to encrypt the message.

### Decrypt

Now, as Bob, let's decrypt the message:
```
$ gpg -d message.txt.gpg
```

### Sign a message

Now let's do this again, this time, in addition to encrypting the message, 
we want to sign it as Alice, so that Bob can be sure that Alice is the one that sent
this message.

First, as Alice, generate a key-pair, as we've done with Bob.
Then, import that public key with Bob, as we've done with Alice.

Now, encrypt the message as such:
```
$ gpg --encrypt --sign -r bob@example.com message.txt
```

Decryption is done the same.
