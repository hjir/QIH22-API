# OpenSSL QKD integration

Generally, if one wants to extend the `OpenSSL` lib, there are two possibilities:
1. create an `OpenSSL` engine;
2. modify the `OpenSSL` state machine.

## OpenSSL engine

The `OpenSSL` engine mechanism is the proper way of extending the library. An engine is a dynamic library created in a special way that can be loaded during `OpenSSL` initialization. Such a library can be used to customize the existing cryptography protocols. The vast majority of popular software, which relies on `OpenSSL`, supports this mechanism.

Engines for QKD protocols are not currently supported in `OpenSSL`. Nevertheless, for the purposes of the hackathon we can simply "abuse" an existing classical protocol. In our case, it makes sense to use the `Diffie-Hellman API` (`DH`). If you prefer to go this way, there is no need to implement the whole state machine behind engine: `OpenSSL` does it for you. The following instructions provide some tips on the `DH OpenSSL` engine.

To overload the Diffie-Hellman protocol it is necessary to set the proper callbacks in the [corresponding structure](https://www.openssl.org/docs/man1.0.2/man3/DH_set_method.html) (`DH_METHOD`):
- `generate_key`: this one is called to perform the first step of a Diffie-Hellman key exchange.
- `compute_key`: this one is called to combine values obtained in the previous step with the other party's public value.
- `bn_mod_exp`: this one computes `r = a ^ p mod m`, which is not relevant for QKD.

Here are some useful links:

- [OpenSSL](https://www.openssl.org/) library.
- Detailed examples of `OpenSSL` engines can be found on a reference implementation of the Russian GOST crypto algorithms [repository](https://github.com/gost-engine/engine). In this repository, you can also find examples of overloading other methods (e.g., `RSA_METHOD`).
- Related code snippets you can find on the [official wiki](https://wiki.openssl.org/index.php/Creating_an_OpenSSL_Engine_to_use_indigenous_ECDH_ECDSA_and_HASH_Algorithms).
- Additional information which could be vital for the implementation process can be found [here](https://linux.die.net/man/3/dh_compute_key).
- You can also consult with the default `OpenSSL DH` [implementation](https://github.com/openssl/openssl/blob/master/crypto/dh/dh_key.c).

## OpenSSL state machine

Considering our desire to run a "quantum" HTTPS session, it makes sense to concentrate on TLS state machine modification (in this particular manual, we implicitly mean `TLS v1.3`). The corresponding code can be found [here](https://github.com/openssl/openssl/tree/master/ssl/statem). The following places are the intervention points:
- `ssl/statem/extensions_clnt.c:add_key_share()` method is responsible for adding the temporary key to the [key share extension](https://tools.ietf.org/html/rfc8446#section-4.2.8). At this point, some information can be sent from a client to a server.
- `ssl/statem/extensions_srvr.c:tls_parse_ctos_key_share()` method is responsible for processing a `key_share extension` received in the `ClientHello`. At this point, information received from the client can be processed. A response can be generated.
- `ssl/statem/extensions_srvr.c:tls_construct_stoc_key_share()` method is responsible for responding to client. At this point, information received from the client can be processed. A response can be generated.
- `ssl/statem/extensions_clnt.c:tls_parse_stoc_key_share()` method is responsible for processing server response. At this point, we are done.

In case you want to register implemented mechanism as a new algorithm there are some tips:
- It is necessary to register algorithm id in `ssl/t1_lib.c:eccurves_default`.
- `ssl/t1_lib.c:tls1_group_id_lookup(), tls1_nid2group_id(), nid_cb()` need to be modified in order to support new id.
- `ssl/t1_trce.c:ssl_groups_tbl` needs to be modified if you want to get human-readable algorithm name in trace outputs.
- `apps/s_cb.c:ssl_print_tmp_key()` needs to be modified if you want proper debug output for you algorithm.
- `ssl/ssl_locl.h:ssl3_state_st` needs to be modified in case you want to share state between methods.


For a more detailed explanation, you can consult the [OpenSSL architecture document](https://www.openssl.org/docs/OpenSSLStrategicArchitecture.html). Although it also contains future architecture plans, it is still a good reference point. Also, you may be interested in the Pre-Shared Key TLS 1.3 protocol feature.
