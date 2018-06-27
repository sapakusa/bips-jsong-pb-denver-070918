
# Signalling BIP Readiness

Signalling simply means the miner of a block has set a bit in the version field to say that they support something. BIP 8 and 9 discuss this, it allows the miners to let the network know they are ready for the change or not. The version field of a block is 32 bits long, and if the top 3 bits are set to '001', 29 bits are free to be used for signalling.

After a certain start time defined for a particular proposal, miners can set the bit specified to signal their support. It is during this time that if at least a certain percentage of blocks are mined with the bit set in one difficulty retarget period (2016 blocks), specified as the threshold needed, that proposal becomes LOCKED_IN. Once it is locked in, it is ready to become active in the network. Locked in just means that it gained enough support to proceed.

This can all be found in [BIP 9](https://github.com/bitcoin/bips/blob/master/bip-0009.mediawiki). 

[[Source](https://bitcoin.stackexchange.com/a/56928)]


```python
from io import BytesIO
from block import Block

class Block(Block):

    def bip9(self):
        '''Returns whether this block is signaling readiness for BIP9'''
        # BIP9 is signalled if the top 3 bits are 001
        # remember version is 32 bytes so right shift 29 (>> 29) and see if
        # that is 001
        pass

    def bip91(self):
        '''Returns whether this block is signaling readiness for BIP91'''
        # BIP91 is signalled if the 5th bit from the right is 1
        # shift 4 bits to the right and see if the last bit is 1
        pass

    def bip141(self):
        '''Returns whether this block is signaling readiness for BIP141'''
        # BIP91 is signalled if the 2nd bit from the right is 1
        # shift 1 bit to the right and see if the last bit is 1
        pass
```
