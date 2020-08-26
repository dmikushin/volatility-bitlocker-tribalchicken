# Volatility Framework: bitlocker

This plugin finds and extracts Full Volume Encryption Key (FVEK) from memory dumps and/or hibernation files using the following methods to locate FVEK:

* **Windows 7**: searching for the FVEc pool tag
* **Windows 8/8.1 and 10**: analysing memory after finding the Cngb pool tag (experimental)

This allows rapid unlocking of systems that had BitLocker encrypted volumes mounted at the time of acquisition. Works on Windows 7 through to Windows 10. The full article is availabe [here](https://tribalchicken.net/recovering-bitlocker-keys-on-windows-8-1-and-10/)

**Update 2016-04-06:** Applied a hacky fix for 32-bit windows. I've realised that I need a more robust solution to handle slight differences in Windows 8 and 32-bit Windows... That will happen soon and will include full Windows 8 support. Until then, Win8 is not currently supported ( 8.1 is though). Contact me if you need more info.

## Preparation

```
python3 -m venv ./venv
source ./venv/bin/activate
pip3 install git+https://github.com/volatilityfoundation/volatility3.git
```

## Usage
bitlocker.py is a plugin for the Volatility Framework. You can either place the plugin in the plugins directory at `volatility/plugins`, or  alternatively, you can place the plugin in a separate directory and point volatility to it with `--plugins`

For example, using a directory called "Plugins":

```
ls plugins
bitlocker.py
volatility --plugins=plugins/ --profile=Win81U1x64 -f WIN81X64-20160916-061911.raw bitlocker
```

## Common Problems

### Volatility tells you it needs something to do

Volatility doesn't know about the plugin. Check the location of the plugin, and run `volatility --info` to determine if it is detected

### The plugin doesn't find anything

There could be many causes.

- The drive is not bitlocker encrypted
- The memory image does not contain the key (Image captured after key is evicted from memory, overwritten during acquisition, etc)
- The key exists but the plugin doesn't find it.

If you suspect the plugin isn't working for you then I would love to know.

