Wednesday, December 31, 2008 at 11:57:56 PM


# General notes #
  * PDF417 barcode with BASE64 encoded binary payload
  * Always 0x7A bytes long == 122 bytes == 976 bits
  * Endianness appears to be Little Endian (Intel)
  * [0x00, 0x39] Always differs from bill to bill
    * _Hypothesis:_ this area contains signature, hash, checksum, etc...
  * [0x40, 0x43] Venue specific _constant_ binary data
    * _Notes:_ left-aligned binary, zero padded (0x00)
  * [0x40, 0x43] Venue specific _constant_ binary data
    * _Notes:_ left-aligned binary, zero padded (0x00)
  * [0x44, 0x47] Monotonically increasing number
    * _Hypothesis:_ ???
	* _Notes:_ left-aligned binary, zero padded (0x00)
  * [0x49, 0x4B]  Venue specific _constant_ binary data
    * _Hypothesis:_ ???
  * [0x4C, 0x55] Unique bill/transaction number
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x56, 0x59] Time related
    * _Hypothesis:_ Could be datetime (inverted?)
    * _Sample:_ 2011-11-22T12:43:06 --> 5A:91:6F:05
    * _Sample:_ 2011-11-25T13:18:?? --> 16:8E:6F:05
    * _Sample:_ 2011-11-25T13:20:?? --> 95:8E:6F:05
  * [0x5A, 0x63] Employee name
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x64, 0x6B] Vendor field, typically table number, takeout, etc.
    * _Notes:_ right-aligned ASCII text, whitespace padded (0x20)
  * [0x6C, 0x6E] ??? (price related)
    * _Notes:_ left-aligned binary, zero padded (0x00)
    * _Hypothesis:_ number field containing price, TPS or TVQ
  * [0x6F, 0x71] ??? (price related)
    * _Notes:_ left-aligned binary, zero padded (0x00)
    * _Hypothesis:_ number field containing price, TPS or TVQ
  * [0x72, 0x77] ??? (price related)
    * _Notes:_ left-aligned binary, zero padded (0x00)
    * _Hypothesis:_ number field containing price, TPS or TVQ
  * [0x78, 0x7A] Constant data (0C:43:0E)

# Interesting sample datasets #

## Same venue, same datetime (source) ##
  *  facture_2011-11-22_001.data
  *  facture_2011-11-22_002.data

## Same venue, same price ##
  *  facture_2011-11-25_001.data
  *  facture_2011-11-25_002.data
