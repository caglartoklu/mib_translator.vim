# mib_translator.vim

Translates SNMP OIDs within Vim.

Home page:
https://github.com/caglartoklu/mib_translator.vim

Vim.org page:
http://www.vim.org/scripts/script.php?script_id=2881

My motivation was to be able to translate the OIDs when I code something about SNMP.
Instead of using a separate tool for OID translation,
I needed the translation within my text editor, Vim in this case.

That is why mib_translator.vim exists:
You don't have to quit your text editor for a simple translation.
Type your code, and when you encounter an OID or a label,
whether it is a comment in the code, or a string literal
in a constant, you can easily access its SNMP translation within Vim.


## Changelog

The changes from now on will be not be posted in this section anymore,
please follow the [commit log](https://github.com/caglartoklu/mib_translator.vim/commits/master)
for that purpose.

- 0.0.6, 2013-11-04
  - Hosted the code on [Github](https://github.com),
    all the development will be on Github from now on.
    [vim.org](http://www.vim.org) will be updated if a major change happens.
  - The license has been changed from GPL to 2-clause FreeBSD.
  - The file is saved in UNIX file format instead of Windows.
  - Added: If the OID is starts with `1.1`, the leading `1` is
    automatically removed.
  - Added: If there are no arguments for `OidTranslateFromLabel` command,
    current word will be used as the argument.
  - Added: If there are no arguments for `OidTranslateFromOid` command,
    mib_translator will try to extract the OID from cursor location.
  - Added: `OidTranslateInfer` command, to infer if the word under cursor
    is whether an OID number or an OID label, and call the appropriate
    function. This is the easiest way to use the script.
  - Added: If the parameters to launch the command are empty/faulty,
    a warning will be displayed and the command will not be run.
  - Added: option `g:OidTranslatorNetSnmpLogging` to enable/disable
    Net-SNMP logs.
  - Added: Command `:OidTranslateNumberList` to display the list
    of OIDs defined in Net-SNMP in another buffer.
  - Added: Displaying line numbers in the opened buffer, especially
    useful for listing OID number.
  - Added option `g:OidTranslatorExposingCommand`, when used, it will print
    the Net-SNMP command to get the information on top of the created
    translator buffer.
  - Added another option `g:OidTranslatorInferMapping` which will be used
    to map the `:OidTranslateInfer` command. By default, there is no default
    key binding for this purpose to avoid clashes with other plugins.
    The key binding can be mapping as you like in your .vimrc, for example:
    `let g:OidTranslatorInferMapping = '<leader>mb'`
  - Fixed : The plugin could cause other new buffers to be non-modifiable,
    now all the options are applied using setlocal, without side effects.
  - Changed: A little refactoring on the converters.
- 0.0.4, 2010-04-12
  - Fixed: If the opened buffer is deleted by hand, that raised an error when the buffer is opened again.
- 0.0.3, 2010-04-12
  - Fixed: Opened buffer remained unmodifiable for the second run.
- 0.0.2, 2010-04-12
  - The opened buffer is colored with MIB file syntax, making it more readable.
  - The opened buffer is not modifiable from now on.
  - Made it more compatible with other plugins, the functions starts with 's:'.
- 0.0.1, 2009-12-04
  - First version with forward and reverse OID translation.


## Installation

For [Vundle](https://github.com/gmarik/vundle) users:

    Bundle 'caglartoklu/mib_translator.vim'

For [Pathogen](https://github.com/tpope/vim-pathogen) users:

    cd ~/.vim/bundle
    git clone git://github.com/caglartoklu/mib_translator.vim

For all other users, simply drop the `mib_translator.vim` file to your
`plugin` directory.


## Requirements

- Vim (no `+Python` required)
- [Net-SNMP](http://www.net-snmp.org) must be
  [downloaded](http://www.net-snmp.org/download.html)
  and installed, since its
  [snmptranslate](http://www.net-snmp.org/docs/man/snmptranslate.html)
  command is used for translation.


## Usage

### :OidTranslateFromLabel
Displays the OID information if the OID name is given. This is the reverse translation.

    :OidReverseTranslate ipv6MIB

![Reverse Inference](https://raw.github.com/caglartoklu/mib_translator.vim/media/sshots/infer_reverse.png)

### :OidTranslateFromOid
Displays the OID information if the OID number is given. This is the forward translation.

    :OidTranslate .1.3.6.1.2.1.55

![Forward Inference](https://raw.github.com/caglartoklu/mib_translator.vim/media/sshots/infer_forward.png)

### :OidTranslateInfer or :OidTranslateInfer
Infers whether to call `OidTranslateFromOid` or `OidTranslateFromLabel`
according to the data the cursor is on.

These are both correct, and they will call the proper translation command:

    :OidTranslateInfer .1.3.6.1.2.1.55

    :OidTranslateInfer ipv6MIB

### :OidTranslateNumberList
Prints the list of all OIDs known by the system.

![Forward Inference](https://raw.github.com/caglartoklu/mib_translator.vim/media/sshots/oidtranslate_list.png)


## Configuration

### g:OidTranslatorBufferName
The name of the buffer to be used to display the
result of the translation process.
Translated values will be displayed in this window.
There is no need to change this value unless there is
a clash with another plugin.
The default is:

    let g:OidTranslatorBufferName = 'OIDTranslator'

### g:OidTranslatorBufferSize
The visible line count of the translation buffer.
If it is not convenient for you, change it
from within VIMRC by copying the following line.
The default is:

    let g:OidTranslatorBufferSize = 10

### g:OidTranslatorSnmpTranslatePath
The path to the `snmptranslate` executable of Net-SNMP.
If the `bin` folder is in your path, no need to change this setting.
If it is not, set this option from your VIMRC.
The value must the path to the executable, not the directory including the executable.
The default value is `snmptranslate`,
so that it can directly run without modification if `snmptranslate` is included in a directory on path.
Note that if Net-SNMP is not on the path, even though you define this option correct,
snmptranslate itself can have difficulties to find the MIB files,
so it is recommended to keep it on the path and not changing this option.
The default is:

    let g:OidTranslatorSnmpTranslatePath = 'snmptranslate'

Full path to the executable can also be used:

    let g:OidTranslatorSnmpTranslatePath = 'C:\\usr\\bin\\snmptranslate.exe'

### g:OidTranslatorNetSnmpLogging
Whether the logs from Net-SNMP to be displayed in
translation window or not.
The default is:

        let g:OidTranslatorNetSnmpLogging = 0

### g:OidTranslatorExposingCommand
Prints the command sent to Net-SNMP
as first line in the translation buffer.
The default is:

        let g:OidTranslatorExposingCommand = 1

### g:OidTranslatorInferMapping
Defines a customizable key binding to inference.
Example for VIMRC:

        let g:OidTranslatorInferMapping = '<leader>mb'

### Path Configuration
On Linux systems, *Net-SNMP* package can easily be installed using the
package manager such as `apt-get`.
On Windows, it is recommended that `C:\usr\bin` is in the path variable.
That is where `snmptranslate.exe` lives.

![Path Configuration](https://raw.github.com/caglartoklu/mib_translator.vim/media/sshots/snmptranslate_path.png)


## License

Licensed with
[2-clause license](https://en.wikipedia.org/wiki/BSD_licenses#2-clause_license_.28.22Simplified_BSD_License.22_or_.22FreeBSD_License.22.29)
("Simplified BSD License" or "FreeBSD License").
See the
[LICENSE](https://github.com/caglartoklu/mib_translator.vim/blob/master/LICENSE) file.


## Legal

All trademarks and registered trademarks are the property of their respective owners.

[Net-SNMP](http://www.net-snmp.org/)
is not part of this software, please see its license file
[here](http://www.net-snmp.org/about/license.html).



## Contact Info
You can find me on
[Google+](https://plus.google.com/108566243864924912767/posts)

Feel free to send bug reports, or ask questions.
