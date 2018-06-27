
# Verifying Transaction Inputs [not working]

This is a review of previous concepts.

### Test Driven Exercise


```python
from tx import Tx
from io import BytesIO

class Tx(Tx):
    def verify_input(self, input_index):
        '''Returns whether the input has a valid signature'''
        # get the relevant input
        # parse the point from the sec format (tx_in.sec_pubkey())
        # parse the signature from the der format (tx_in.der_signature())
        # get the hash type from the input (tx_in.hash_type())
        # get the sig_hash (z)
        # use point.verify on the z and signature
        pass
```


```python
# Transaction Construction Example

from ecc import PrivateKey
from helper import decode_base58, p2pkh_script, SIGHASH_ALL
from script import Script
from tx import TxIn, TxOut, Tx

# Step 1
tx_ins = []
prev_tx = bytes.fromhex('0025bc3c0fa8b7eb55b9437fdbd016870d18e0df0ace7bc9864efc38414147c8')
tx_ins.append(TxIn(
            prev_tx=prev_tx,
            prev_index=0,
            script_sig=b'',
            sequence=0xffffffff,
        ))

# Step 2
tx_outs = []
h160 = decode_base58('mzx5YhAH9kNHtcN481u6WkjeHjYtVeKVh2')
tx_outs.append(TxOut(
    amount=int(0.99*100000000),
    script_pubkey=p2pkh_script(h160),
))
h160 = decode_base58('mnrVtF8DWjMu839VW3rBfgYaAfKk8983Xf')
tx_outs.append(TxOut(
    amount=int(0.1*100000000),
    script_pubkey=p2pkh_script(h160),
))
tx_obj = Tx(version=1, tx_ins=tx_ins, tx_outs=tx_outs, locktime=0, testnet=True)

# Step 3
hash_type = SIGHASH_ALL
z = tx_obj.sig_hash(0, hash_type)
pk = PrivateKey(secret=8675309)
der = pk.sign(z).der()
sig = der + bytes([hash_type])
sec = pk.point.sec()
script_sig = bytes([len(sig)]) + sig + bytes([len(sec)]) + sec
script_sig = bytes([len(script_sig)]) + script_sig
tx_obj.tx_ins[0].script_sig = Script.parse(script_sig)
print(tx_obj.serialize().hex())
```
