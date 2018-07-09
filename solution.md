
# Verifying Transactions

This is a review of previous concepts.

### Test Driven Exercise


```python
from tx import Tx
from io import BytesIO
from ecc import S256Point, Signature

class Tx(Tx):

    
    def verify_input(self, input_index):
        '''Returns whether the input has a valid signature'''
        # get the relevant input
        tx_in = self.tx_ins[input_index]
        # parse the point from the sec format (tx_in.sec_pubkey())
        point = S256Point.parse(tx_in.sec_pubkey())
        # parse the signature from the der format (tx_in.der_signature())
        signature = Signature.parse(tx_in.der_signature())
        # get the hash type from the input (tx_in.hash_type())
        hash_type = tx_in.hash_type()
        # get the sig_hash (z)
        z = self.sig_hash(input_index, hash_type)
        # use point.verify on the z and signature
        return point.verify(z, signature)
```
