Wednesday, December 31, 2008 at 11:57:56 PM


# General notes #
  * PDF417 barcode with BASE64 encoded binary payload
  * Always 0x7A bytes long == 122 bytes == 976 bits
  * Endianness appears to be Little Endian (Intel)
  * [0x00, 0x39] Always differs from bill to bill
    * _Hypothesis:_ this area contains signature, hash, checksum, etc...
  * [0x40, 0x43] MEV serial number
    * _Notes:_ left-aligned binary 32-bits little endian, zero padded (0x00)
  * [0x44, 0x47] MEV transaction counter (monotonically increasing)
	* _Notes:_ left-aligned binary 32-bits little endian, zero padded (0x00)
  * [0x48, 0x4B]  Unknown data 
    * _Hypothesis:_ Potential 32-bit number ???
  * [0x4C, 0x55] Unique bill/transaction number
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x56, 0x59] Bill datetime
    * _Note:_ Number of seconds from MRQ Epoch (2009-01-01)
    * _Example:_ python -c "import struct; import sys; import datetime; print datetime.datetime(2009,1,1) + datetime.timedelta(seconds=struct.unpack('<L','\x5a\x91\x6f\x05')[0])"
    * _Sample:_ 2011-11-22T12:43:06 --> 5A:91:6F:05
    * _Sample:_ 2011-11-25T13:18:?? --> 16:8E:6F:05
    * _Sample:_ 2011-11-25T13:20:?? --> 95:8E:6F:05
  * [0x5A, 0x63] Employee name
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x64, 0x6B] Vendor field, typically table number, takeout, etc.
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x6C, 0x6E] TPS value in cents 
    * _Notes:_ left-aligned binary 24-bits, little-endian, zero padded (0x00)
    * _Sample:_ 0B:00:00 == 11 cents == 0.11$
  * [0x6F, 0x71] TVQ value in cents
    * _Notes:_ left-aligned binary 24-bits, little-endian, zero padded (0x00)
    * _Sample:_ 13:00:00 == 19 cents == 0.19$
  * [0x72, 0x77] Total price (including TPS+TVQ) in cents
    * _Notes:_ left-aligned binary 24-bits, little-endian, zero padded (0x00)
    * _Sample:_ 13:00:00 == 246 cents == 2.46$
  * [0x78, 0x7A] Constant data (0C:43:0E)

# Interesting sample datasets #

## Same venue, same datetime (source) ##
  *  facture_2011-11-22_001.data
  *  facture_2011-11-22_002.data

## Same venue, same price ##
  *  facture_2011-11-25_001.data
  *  facture_2011-11-25_002.data
