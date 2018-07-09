
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
        # parse the point from the sec format (tx_in.sec_pubkey())
        # parse the signature from the der format (tx_in.der_signature())
        # get the hash type from the input (tx_in.hash_type())
        # get the sig_hash (z)
        # use point.verify on the z and signature
        pass
```
