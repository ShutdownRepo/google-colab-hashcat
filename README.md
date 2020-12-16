# Google colab hash cracking

<p align="center">
  <a href="https://colab.research.google.com/github/ShutdownRepo/google-colab-hashcat/blob/main/google_colab_hashcat.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>
  <a href="https://twitter.com/intent/follow?screen_name=_nwodtuhs" title="Follow"><img src="https://img.shields.io/twitter/follow/_nwodtuhs?label=Shutdown&style=social"></a>
</p>

## Workflow example 1 (simple wordlist)

This Google colab can be used for hash cracking with wordlists and rules.
Here is an example of that can be followed to crack NT hashes.

1. run the preparation script below
2. upload your hashes list on the colab `!wget http://yourip:yourport/yourfile`
3. run a hashcat command like this to start cracking `!hashcat --status --hash-type 1000 --attack-mode 0 --username DOMAIN.LOCAL.ntds wordlists/rockyou.txt`

## Workflow example 2 (wordlist + rules)

This is an example that is especially useful for internal engagements where users often use a transformation of the corp name as password (i.e. Corp2016!).

1. create a wordlist based on some names that are currently used in the company
```
company
cpmny
corp
management
admin
```
2. upload your hashes list on the colab `!wget http://yourip:yourport/yourfile`
3. run the hashcat command `!hashcat --status --hash-type 1000 --attack-mode 0 --username --rules-file rules/d3adhob0.rule DOMAIN.LOCAL.ntds company.lst`

## Workflow example 3 (OPSEC: crack anonymized hashes)

1. run the preparation script below
2. on your local machine, run [hashonymize](https://github.com/ShutdownRepo/hashonymize) to anonymize your hash lists
3. upload your anon hashes list on the colab `!wget http://yourip:yourport/yourfile`
4. run a hashcat command like this to start cracking `!hashcat --status --hash-type 1000 --attack-mode 0 --username DOMAIN.LOCAL.ntds wordlists/rockyou.txt`
5. recover the .pot file from the Google Colab `!curl --upload-file ~/.hashcat/hashcat.potfile http://yourip:yourport/`
6. on your local machine, run the following hashcat command with the recovered potfile to match real usernames with cracked password `hashcat --potfile-path hashcat.potfile --hash-type 1000 --username DOMAIN.LOCAL.ntds wordlists/rockyou.txt`

hashcat -m 1000 --potfile-path ntds.cracked ntds.tocrack --show --username

## Short hashcat manual

Here are some useful options
```
--status            Enable automatic update of the status screen
--attack-mode       Attack-mode, see references below
--hash-type         Hash-type, see references below
--username          Enable ignoring of usernames in hashfile 
--rules-file        Multiple rules applied to each word from wordlists
--potfile-path      Specific path to potfile
```

Here are some of the most used attack modes for the `--attack-mode` option
```
0     Wordlist (with or without rules)
3     Pure bruteforce
```

Here are some of the most used hash types for the `--hash-type` option
```
1000     NTLM (actually it's for NT hashes)
3000     LM
5500     Net-NTLMv1 (actually, it should be called NTLMv1)
5600     Net-NTLMv2 (actually, it should be called NTLMv2)
13100    Kerberoast
18200    ASREProast
22000    WPA-PBKDF2-PMKID+EAPOL
16800    WPA-PMKID-PBKDF2
0        md5
100      sha1
1400     sha2-256
1700     sha2-512
```

# Credits
Credits go to mxrch for his original project called [penglab](https://github.com/mxrch/penglab)
