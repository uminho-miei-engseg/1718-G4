```
# general
(gen) banner: SSH-2.0-OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.3
(gen) software: OpenSSH 6.6.1p1
(gen) compatibility: OpenSSH 6.5-6.6, Dropbear SSH 2013.62+ (some functionality from 0.52)
(gen) compression: enabled (zlib@openssh.com)

# key exchange algorithms
(kex) curve25519-sha256@libssh.org          -- [info] available since OpenSSH 6.5, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp256                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp384                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) ecdh-sha2-nistp521                    -- [fail] using weak elliptic curves
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(kex) diffie-hellman-group-exchange-sha256  -- [warn] using custom size modulus (possibly weak)
                                            `- [info] available since OpenSSH 4.4
(kex) diffie-hellman-group-exchange-sha1    -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.3.0
(kex) diffie-hellman-group14-sha1           -- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 3.9, Dropbear SSH 0.53
(kex) diffie-hellman-group1-sha1            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [fail] disabled (in client) since OpenSSH 7.0, logjam attack
                                            `- [warn] using small 1024-bit modulus
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.3.0, Dropbear SSH 0.28

# host-key algorithms
(key) ssh-rsa                               -- [info] available since OpenSSH 2.5.0, Dropbear SSH 0.28
(key) ssh-dss                               -- [fail] removed (in server) and disabled (in client) since OpenSSH 7.0, weak algorithm
                                            `- [warn] using small 1024-bit modulus
                                            `- [warn] using weak random number generator could reveal the key
                                            `- [info] available since OpenSSH 2.1.0, Dropbear SSH 0.28
(key) ecdsa-sha2-nistp256                   -- [fail] using weak elliptic curves
                                            `- [warn] using weak random number generator could reveal the key
                                            `- [info] available since OpenSSH 5.7, Dropbear SSH 2013.62
(key) ssh-ed25519                           -- [info] available since OpenSSH 6.5

# encryption algorithms (ciphers)
(enc) aes128-ctr                            -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52
(enc) aes192-ctr                            -- [info] available since OpenSSH 3.7
(enc) aes256-ctr                            -- [info] available since OpenSSH 3.7, Dropbear SSH 0.52
(enc) arcfour256                            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher
                                            `- [info] available since OpenSSH 4.2
(enc) arcfour128                            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher
                                            `- [info] available since OpenSSH 4.2
(enc) aes128-gcm@openssh.com                -- [info] available since OpenSSH 6.2
(enc) aes256-gcm@openssh.com                -- [info] available since OpenSSH 6.2
(enc) chacha20-poly1305@openssh.com         -- [info] available since OpenSSH 6.5
                                            `- [info] default cipher since OpenSSH 6.9.
(enc) aes128-cbc                            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak cipher mode
                                            `- [info] available since OpenSSH 2.3.0, Dropbear SSH 0.28
(enc) 3des-cbc                              -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak cipher
                                            `- [warn] using weak cipher mode
                                            `- [warn] using small 64-bit block size
                                            `- [info] available since OpenSSH 1.2.2, Dropbear SSH 0.28
(enc) blowfish-cbc                          -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [fail] disabled since Dropbear SSH 0.53
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher mode
                                            `- [warn] using small 64-bit block size
                                            `- [info] available since OpenSSH 1.2.2, Dropbear SSH 0.28
(enc) cast128-cbc                           -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher mode
                                            `- [warn] using small 64-bit block size
                                            `- [info] available since OpenSSH 2.1.0
(enc) aes192-cbc                            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak cipher mode
                                            `- [info] available since OpenSSH 2.3.0
(enc) aes256-cbc                            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak cipher mode
                                            `- [info] available since OpenSSH 2.3.0, Dropbear SSH 0.47
(enc) arcfour                               -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher
                                            `- [info] available since OpenSSH 2.1.0
(enc) rijndael-cbc@lysator.liu.se           -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak cipher mode
                                            `- [info] available since OpenSSH 2.3.0

# message authentication code algorithms
(mac) hmac-md5-etm@openssh.com              -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 6.2
(mac) hmac-sha1-etm@openssh.com             -- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 6.2
(mac) umac-64-etm@openssh.com               -- [warn] using small 64-bit tag size
                                            `- [info] available since OpenSSH 6.2
(mac) umac-128-etm@openssh.com              -- [info] available since OpenSSH 6.2
(mac) hmac-sha2-256-etm@openssh.com         -- [info] available since OpenSSH 6.2
(mac) hmac-sha2-512-etm@openssh.com         -- [info] available since OpenSSH 6.2
(mac) hmac-ripemd160-etm@openssh.com        -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [info] available since OpenSSH 6.2
(mac) hmac-sha1-96-etm@openssh.com          -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 6.2
(mac) hmac-md5-96-etm@openssh.com           -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 6.2
(mac) hmac-md5                              -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using encrypt-and-MAC mode
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.1.0, Dropbear SSH 0.28
(mac) hmac-sha1                             -- [warn] using encrypt-and-MAC mode
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.1.0, Dropbear SSH 0.28
(mac) umac-64@openssh.com                   -- [warn] using encrypt-and-MAC mode
                                            `- [warn] using small 64-bit tag size
                                            `- [info] available since OpenSSH 4.7
(mac) umac-128@openssh.com                  -- [warn] using encrypt-and-MAC mode
                                            `- [info] available since OpenSSH 6.2
(mac) hmac-sha2-256                         -- [warn] using encrypt-and-MAC mode
                                            `- [info] available since OpenSSH 5.9, Dropbear SSH 2013.56
(mac) hmac-sha2-512                         -- [warn] using encrypt-and-MAC mode
                                            `- [info] available since OpenSSH 5.9, Dropbear SSH 2013.56
(mac) hmac-ripemd160                        -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using encrypt-and-MAC mode
                                            `- [info] available since OpenSSH 2.5.0
(mac) hmac-ripemd160@openssh.com            -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using encrypt-and-MAC mode
                                            `- [info] available since OpenSSH 2.1.0
(mac) hmac-sha1-96                          -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using encrypt-and-MAC mode
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.5.0, Dropbear SSH 0.47
(mac) hmac-md5-96                           -- [fail] removed (in server) since OpenSSH 6.7, unsafe algorithm
                                            `- [warn] disabled (in client) since OpenSSH 7.2, legacy algorithm
                                            `- [warn] using encrypt-and-MAC mode
                                            `- [warn] using weak hashing algorithm
                                            `- [info] available since OpenSSH 2.5.0

# algorithm recommendations (for OpenSSH 6.6.1)
(rec) -diffie-hellman-group14-sha1          -- kex algorithm to remove 
(rec) -diffie-hellman-group-exchange-sha1   -- kex algorithm to remove 
(rec) -diffie-hellman-group1-sha1           -- kex algorithm to remove 
(rec) -ecdh-sha2-nistp256                   -- kex algorithm to remove 
(rec) -ecdh-sha2-nistp521                   -- kex algorithm to remove 
(rec) -ecdh-sha2-nistp384                   -- kex algorithm to remove 
(rec) -ecdsa-sha2-nistp256                  -- key algorithm to remove 
(rec) -ssh-dss                              -- key algorithm to remove 
(rec) -arcfour                              -- enc algorithm to remove 
(rec) -rijndael-cbc@lysator.liu.se          -- enc algorithm to remove 
(rec) -blowfish-cbc                         -- enc algorithm to remove 
(rec) -3des-cbc                             -- enc algorithm to remove 
(rec) -aes256-cbc                           -- enc algorithm to remove 
(rec) -arcfour256                           -- enc algorithm to remove 
(rec) -cast128-cbc                          -- enc algorithm to remove 
(rec) -aes192-cbc                           -- enc algorithm to remove 
(rec) -arcfour128                           -- enc algorithm to remove 
(rec) -aes128-cbc                           -- enc algorithm to remove 
(rec) -hmac-sha2-512                        -- mac algorithm to remove 
(rec) -hmac-md5-96                          -- mac algorithm to remove 
(rec) -hmac-md5-etm@openssh.com             -- mac algorithm to remove 
(rec) -hmac-sha1-96-etm@openssh.com         -- mac algorithm to remove 
(rec) -hmac-ripemd160-etm@openssh.com       -- mac algorithm to remove 
(rec) -hmac-md5-96-etm@openssh.com          -- mac algorithm to remove 
(rec) -hmac-sha2-256                        -- mac algorithm to remove 
(rec) -hmac-ripemd160                       -- mac algorithm to remove 
(rec) -umac-128@openssh.com                 -- mac algorithm to remove 
(rec) -hmac-sha1-96                         -- mac algorithm to remove 
(rec) -umac-64@openssh.com                  -- mac algorithm to remove 
(rec) -hmac-md5                             -- mac algorithm to remove 
(rec) -hmac-ripemd160@openssh.com           -- mac algorithm to remove 
(rec) -hmac-sha1                            -- mac algorithm to remove 
(rec) -hmac-sha1-etm@openssh.com            -- mac algorithm to remove 
(rec) -umac-64-etm@openssh.com              -- mac algorithm to remove 

```
