Binary file Manipulation
========================

Two's complement
################


    To get 2's complement of binary number is 1's complement of given number plus 1 to the least significant bit (LSB). For example 2's complement of binary number 10010 is (01101) + 1 = 01110


Example to get the representation of -5 on 5 bits:

1. Get the positive binary representation: 0 0101
2. Apply to it the two's complement: 1 1011


Basic Writing/Reading example
#############################

Let's consider the following binary file structure:

+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  Field Name  |  Description                              |  Size    |  Format   |  Byte order     +  Number of byte(s) |
+==============+===========================================+==========+===========+=================+====================+
|  txt0        |  | Text in ASCII with 12 characters       |  12      |  ASCII    |  _              |   12               |
|              |  | Date format -> "YYYY-MM-DD"            |          |           |                 |                    |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  num0        |  1 unsigned int 32 bits                   |  1       |  uint32   |  Little Endian  |   4                |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  num1        |  1 int 8 bits                             |  1       |  int8     |  _              |   1                |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  array0      |  3 int 16 bits                            |  3       |  int16    |  Little Endian  |   6                |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  _           |  3 unused bits                            |  _       |  _        |  Little Endian  |   1                |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  flag0       |  1 bit                                    |  1       |  bit      |                 |                    |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  flag1       |  1 bit                                    |  1       |  bit      |                 |                    |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  flag2       |  1 bit                                    |  1       |  bit      |                 |                    |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  flag3       |  1 bit                                    |  1       |  bit      |                 |                    |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  flag4       |  1 bit                                    |  1       |  bit      |                 |                    |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  array1      |  100 int 32 bits                          |  100     |  int32    |  Big Endian     |  400               |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  _           |  6 unused bytes                           |  6       |  _        |  Little Endian  |  6                 |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  num2        |  1 unsigned int 32 bits                   |  1       |  int32    |  Big Endian     |  4                 |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  _           |  3 unused bits                            |  _       |           |  Little Endian  |  2                 |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  num3        |  1 unsigned int 13 bits                   |  1       |  uint13   |                 |                    |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|  _           |  2 unused bits                            |  _       |           |  Little Endian  |  1                 |
+--------------+-------------------------------------------+----------+-----------+                 |                    +
|  num4        |  1 signed int 6 bits (Two's complement)   |  1       |  int6     |                 |                    |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+
|                                                                                                   |  TOTAL = 437       |
+--------------+-------------------------------------------+----------+-----------+-----------------+--------------------+


.. code-block:: python

    import datetime as dt
    import numpy as np
    import os

    # Field values
    txt0 = dt.datetime.now().strftime("%Y-%m-%d")
    num0 = 3932881021
    num1 = -45
    array0 = np.array([-31324, 25324, 672], dtype=np.int16)
    flag0 = True
    flag1 = False
    flag2 = False
    flag3 = True
    flag4 = True
    array1 = np.linspace(-2 ** 31, 2 ** 31 - 1, 100, dtype=np.int32)
    num2 = 99
    num3 = 2**13 - 5
    num4 = -17

    # txt0: 10 ASCII characters
    byte_array = txt0.encode("ascii")
    # num0: uint32 - little endian
    byte_array += struct.pack("<I", num0)
    # num1: int8
    byte_array += struct.pack('b', num1)
    # array0: 3 * int16 - little endian
    byte_array += struct.pack("<hhh", *array0)
    # Flags
    flag_byte = int(flag0) * 2 ** 4
    flag_byte += int(flag1) * 2 ** 3
    flag_byte += int(flag2) * 2 ** 2
    flag_byte += int(flag3) * 2 ** 1
    flag_byte += int(flag4)
    byte_array += struct.pack('B', flag_byte) # B = unsigned byte
    # array1: 100 * int32 ; big endian
    byte_array += struct.pack('>' + 100 * 'i', *array1) 
    # 6 empty bytes
    byte_array += b'\x00\x00\x00\x00\x00\x00'
    # num2: uint32 - big endian
    byte_array += struct.pack(">I", num2)
    # num3: uint13 - little endian
    # we don't care about the value of the 3 unused bits
    # Just write it as an uint16
    byte_array += struct.pack("<H", num3)
    # num4: int6 (two's complements)
    # num4 being already encoded as two's complement by python type 'int'
    # I just have to apply bitwise mask to set the 2 unused MSB to 0
    byte_array += struct.pack("B", num4 & 0x3F)

    filename = "binary_file"

    with open(filename, "wb") as f:
        f.write(byte_array)

    # Check file size corresponds to the description
    nb_bytes = os.stat(filename).st_size


    # Decode the 2's complement of int value
    def decode_twos_comp(val, bits):
        if (val & (1 << (bits - 1))) != 0: # if sign bit is set e.g., 8bit: 128-255
            val = val - (1 << bits)        # compute negative value
        return val                         # return positive value as is


    with open(filename, "rb") as f:
        txt0_bytes = f.read(10)
        num0_bytes = f.read(4)
        num1_bytes = f.read(1)
        array0_bytes = f.read(6)
        flags_bytes = f.read(1)
        array1_bytes = f.read(400)
        _ = f.read(6)
        num2_bytes = f.read(4)
        num3_bytes = f.read(2)
        num4_bytes = f.read(1)


    txt0_recovered = txt0_bytes.decode("ascii")
    num0_recovered = struct.unpack("I", num0_bytes)[0]
    num1_recovered = struct.unpack('b', num1_bytes)[0]
    array0_recovered = np.array(struct.unpack("<hhh", array0_bytes))
    flags_int = struct.unpack('B', flags_bytes)[0]
    flag0_recovered = (flags_int & 0x10) > 0 
    flag1_recovered = (flags_int & 0x08) > 0
    flag2_recovered = (flags_int & 0x04) > 0
    flag3_recovered = (flags_int & 0x02) > 0
    flag4_recovered = (flags_int & 0x01) > 0
    array1_recovered = np.array(struct.unpack('>' + 100 * 'i', array1_bytes))
    num2_recovered = struct.unpack(">I", num2_bytes)
    num3_recovered = struct.unpack("<H", num3_bytes)[0] & 0x1FFF
    num4_recovered = decode_twos_comp(struct.unpack("B", num4_bytes)[0], 6)


------------------------------------------------------------

**Sources**:

- Two's complement implementation: https://stackoverflow.com/questions/1604464/twos-complement-in-python
- Two's complement explained: https://www.tutorialspoint.com/two-s-complement
- Two's complement online: https://www.omnicalculator.com/math/twos-complement
- Struct documentation: https://docs.python.org/3/library/struct.html
