# QKD API

The mockup provided in this repository is written for [ETSI QKD 004 v1.1.1](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/01.01.01_60/gs_QKD004v010101p.pdf), but there is a more recent version [ETSI QKD 004 v2.1.1](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/02.01.01_60/gs_QKD004v020101p.pdf) which you are encouraged to use in your project. You can see for yourself what are the differences or check Annex E of [v2.1.1](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/02.01.01_60/gs_QKD004v020101p.pdf) for changelog.

(There is also an API defined for REST-based key delivery, see [ETSI QKD 014 v1.1.1](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/014/01.01.01_60/gs_QKD014v010101p.pdf) - this is a more constrained API which uses HTTPS protocols and JSON data format. See Annex A of [ETSI QKD 014](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/014/01.01.01_60/gs_QKD014v010101p.pdf) or Annex C of [ETSI QKD 004](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/02.01.01_60/gs_QKD004v020101p.pdf) for more details on the relationship between these two interfaces.)

The following are methods defined in the [ETSI QKD 004 v1.1.1](https://www.etsi.org/deliver/etsi_gs/QKD/001_099/004/01.01.01_60/gs_QKD004v010101p.pdf):


```c
uint32_t QKD_OPEN(ip_address_t destination, qos_t qos, key_handle_t key_handle);
```
Establishes a set of parameters that define the expected levels of key service.
- :param `destination`: address of peer application
- :param `qos`: a structure describing the characteristics of the requested key source
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :return: status of operation

---

```c
uint32_t QKD_CONNECT_NONBLOCK(key_handle_t key_handle, uint32_t timeout);
uint32_t QKD_CONNECT_BLOCKING(key_handle_t key_handle, uint32_t timeout);
```
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :param `timeout`:
- :return: status of operation

---

```c
uint32_t QKD_GET_KEY(key_handle_t key_handle, char *key_buffer);
```
Obtains the required amount of key material requested for this `key_handle`. Each call returns the fixed amount of requested key stored in `key_buffer`.
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :param `key_buffer`: buffer containing the current stream of keys
- :return: status of operation

---

```c
uint32_t QKD_CLOSE(key_handle_t key_handle);
```
Terminates the association established for this `key_handle`.
- :param `key_handle`: a unique handle to identify the group of synchronized bits provided by the QKD key manager to the application
- :return: status of operation

---
