# TestDrive License Manager (TestDrive_LM)
This is license manager for source **<span style="color:red">encryption</span>**/**<span style="color:blue">decryption</span>**.

```bash
> TestDrive_LM
License Manager for TestDrive Profiling Master. - (build : May 29 2024)

Usage : TestDrive_LM [options] OP ...
        [options]
            -f flag_file    : specify custom flag file.
            -k key_file     : specify custom private_key list file.
            -t tag_string   : specify custom tag string.
        OP
            create [public_key]
                : Create encrypted public_key file('.TestDrive.public_key').
            install [public_key]
                : Install private_key from public_key on this system.
            register private_key
                : Register new private_key on this system.
            private [public_key] [private_hash]
                : Get private_key.
            status
                : Get private_key status.
            encrypt input_file [output_file]
                : Encrypt file.
            decrypt input_file [output_file]
                : Decrypt file.
            discard
                : Discard all private keys on your system.
```
...(under construction...)

### [:fa-arrow-left: Back](?top.md)