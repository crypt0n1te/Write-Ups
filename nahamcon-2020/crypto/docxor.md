# Docxor
We used [this tool](https://wiremask.eu/tools/xor-cracker/) to guess the key, setting the keylength to 4.

We then used cyberchef to XOR the file with the key `5a 41 99 bb`

It says it determines a `.zip` file, but when unzipping you realise it's a `.docx` file so change the extension to get:

**flag{xor_is_not_for_security}**