# helpers

[history-18v.pptx](https://disk.yandex.ru/i/Zou_NPs4mFSXoA)
[history-18.pdf](https://disk.yandex.ru/i/ShnYZ_LmbZh_gw)

[VDF home page  for plugins IDEA](https://plugins.jetbrains.com/plugin/22668-valve-data-format)

[ITU-R M.1371-5](https://www.itu.int/dms_pubrec/itu-r/rec/m/R-REC-M.1371-5-201402-I!!PDF-R.pdf)


```c++

/*
 * after GetInf info contains CRC
 * this function divides info and CRC
 */
uint8_t AisDemodulator::GetCRC(uint16_t length)
{
	if (length < 16)
		return 0;
	else
	{
		for (int i = 0; i < 16; ++i)
		{
			crc[i] = *info.Ptr(length - 16 + i);
			*info.Ptr(length - 16 + i) = 0;
		}
		return 1;
	}
}
//////////////////

/*
 * checks CRC with VTV algorithm
 */
uint8_t AisDemodulator::CRCCheck(uint16_t length)
{
	uint8_t reg[16], out;
	memset(reg, 0, 16);

	if (length < 16)
		return 0;

	for (int i = 0; i < 16; ++i) {
	    out = reg[15];
	    reg[11] = reg[11] ^ out;
	    reg[4] = reg[4] ^ out;
	    for (int n = 0; n < 15; ++n)
	    	reg[15 - n] = reg[14 - n];
	    out = out ^ 1;
	    reg[0] = (*info.Ptr(i)) ^ out;
	}

	for (int i = 16; i < length - 16; ++i) {
	    out = reg[15];
	    reg[11] = reg[11] ^ out;
	    reg[4] = reg[4] ^ out;
	    for (int n = 0; n < 15; ++n)
	    	reg[15 - n] = reg[14 - n];
	    reg[0] = (*info.Ptr(i)) ^ out;
	}

	for (int i = 0; i < 16; ++i) {
	    out = reg[15];
	    reg[11] = reg[11] ^ out;
	    reg[4] = reg[4] ^ out;
	    for (int n = 0; n < 15; ++n)
	    	reg[15 - n] = reg[14 - n];
	    out = out ^ 1;
	    reg[0] = crc[i] ^ out;
	}

	for (int i = 0; i < 16; i++)
	    if (reg[i])
	        return 0;
	return 1;
}


```

Царик Дмитрий Владимирович
