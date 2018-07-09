
# Block Version

Version in normal software refers to a particular state. That is, a particular version reflects a particular set of features. For a block, this is similar, in the sense that the version field reflects what capabilities the software that produced the block is ready for. In the past this was used as a way to indicate a single feature that was ready. Version 2 meant that the software was ready for BIP0016, or pay-to-script-hash as described previously.

Unfortunately, this means that only one feature may be signaled on the network at a time. To alleviate this, the developers came up with BIP9, which allows up to 29 different features to be signaled at the same time. The way this works is discussed later.

### Try it


```python
# Version Signaling Example

from io import BytesIO
from block import Block

hex_block = '020000208ec39428b17323fa0ddec8e887b4a7c53b8c0a0a220cfd0000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c3157f961db38fd8b25be1e77a759e93c0118a4ffd71d'

# bytes.fromhex to get the binary block
bin_block = bytes.fromhex(hex_block)
# create a stream using BytesIO
stream = BytesIO(bin_block)
# parse the block
b = Block.parse(stream)
# get the version
version = b.version
# rightshift 29 (version >> 29) and see if it's equal to 0b001 for BIP9
print('BIP9: {}'.format(version >> 29 == 0b001))
# see if bit 4 (version >> 4) from the right is set ( & 1) for BIP91
print('BIP91: {}'.format(version >> 4 & 1 == 1))
# see if bit 1 (version >> 1) from the right is set ( & 1) for BIP141
print('BIP141: {}'.format(version >> 1 & 1 == 1))
```

### Signalling BIP Readiness

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
        return self.version >> 29 == 0b001

    def bip91(self):
        '''Returns whether this block is signaling readiness for BIP91'''
        # BIP91 is signalled if the 5th bit from the right is 1
        # shift 4 bits to the right and see if the last bit is 1
        return self.version >> 4 & 1 == 1
    
    def bip141(self):
        '''Returns whether this block is signaling readiness for BIP141'''
        # BIP91 is signalled if the 2nd bit from the right is 1
        # shift 1 bit to the right and see if the last bit is 1
        return self.version >> 1 & 1 == 1
```
