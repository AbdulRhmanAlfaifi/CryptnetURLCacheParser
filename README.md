# CryptnetURLCacheParser

CryptnetURLCacheParser is a tool to parse CryptAPI cache files located on following paths:

```
C:\Windows\System32\config\systemprofile\AppData\LocalLow\Microsoft\CryptnetUrlCache
C:\Windows\SysWOW64\config\systemprofile\AppData\LocalLow\Microsoft\CryptnetUrlCache
C:\Users\<USERNAME>\AppData\LocalLow\Microsoft\CryptnetUrlCache
```

The `metadata` folder contains metadata about the downloaded files. Each file contain the following data:

1. Timestamp : This is the last time the file was downloaded.
2. URL : The URL form where the file was downloaded.
3. FileSize : The downloaded file size in bytes.
4. MetadataHash : The hash for the downloaded file.  The following is some of the hashing algorithms absorved:
   * SHA1
   * SHA256
   * MD5
5. FullPath : The full path for the parsed file.
6. MD5 (Optional) : The calculated MD5 hash for the actual file in the `content` folder. This field is only available if you used the `--useConent` option.



## Installation

### From source

1. clone the repository:

```
git clone https://github.com/AbdulRhmanAlfaifi/CryptnetURLCacheParser
```

That is it. 

### Precompiled

You can use the latest compiled windows executable from the release section.

## How to use

The following is the command line tool help message:

```
usage: CryptnetUrlCacheParser.py [-h] [-d DIRS [DIRS ...]] [-o OUTPUT]
                                 [--outputFormat {csv,json,jsonl}]
                                 [--useContent]

CryptnetUrlCache Metadata Parser - Developded by AbdulRhman Alfaifi

optional arguments:
  -h, --help            show this help message and exit
  -d DIRS [DIRS ...], --dirs DIRS [DIRS ...]
                        A list of dirs that contain certutil cache files
                        (default: all certutil cache paths)
  -o OUTPUT, --output OUTPUT
                        The file path to write the output to (default: stdout)
  --outputFormat {csv,json,jsonl}
                        The output formate (default: csv)
  --useContent          Try finding the cached file and calculate the MD5 hash
                        for it
```

* **-d** or **--dirs** : a list of directories that contains `CryptnetUrlCache` metadata files. the default paths are :
  * C:\Windows\System32\config\systemprofile\AppData\LocalLow\Microsoft\CryptnetUrlCache
  * C:\Windows\SysWOW64\config\systemprofile\AppData\LocalLow\Microsoft\CryptnetUrlCache
  * C:\Users\<USERNAME>\AppData\LocalLow\Microsoft\CryptnetUrlCache

* **-o** or **--output** : the output file path. default to `stdout`.
* **--outputFormat** : the results output format. you can choose from the following:
  * csv (default)
  * json
  * jsonl
* **--useContent** : try to find the actual file related to the metadata file and calculate it's MD5 hash. The following are the steps taken to accomplish this task:
  * Save the metadata file name (ex. `00000000000000000000000000000000`)
  * Go to parent directory.
  * Go inside `Conent` directory.
  * Check if the metadata file name saved earlier is present. (ex. `00000000000000000000000000000000`)
  * If preset calculate file's MD5 hash, otherwise return `00000000000000000000000000000000`