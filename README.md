# useful_scripts

This repository groups useful scripts for the general NCCR Catalysis community.
You can keep up to date on addition to this repo with the "Watch" functionality on GitHub.

If you are familiar with Python, you can use the functions in these scripts as you wish.
If you are not familiar with Python, just make sure to have a Python3 installation and use the scripts with the command line interface (CLI) as explained in this documentation.

For any feedback, to report a bug, or to request a feature, contact: [admin@nccr-catalysis.ch](mailto:admin@nccr-catalysis.ch)

# Installation

```
# 1. Clone your repo
git clone https://github.com/YourUsername/useful_scripts.git
cd useful_scripts

# 2. Install the package in "editable" mode for development (best practice)
pip install -e .
```
# Importing
To use the functions in your own scripts, import as follows:

```from nccr_cat_scripts import [module name]```

where `[module name]` is any of the modules documented below (zip_utils, excel_utils).

# Command Line Interface (CLI)
Every module can be called in the command line. Generally this is just the module name but where "_" has been replaced with "-".
Then a command needs to be given to specify what function to run. You can see which commands are available by running 

```module-name --help```

or by reading the section of this documentation that concerns that module.
If you do not know how to use a command you can run

`module-name [command] --help`

or check this documentation.

# zip_utils

Functionalities concerning zip files particularly with regards to Zenodo datasets.

## Recursive extraction

The function `extract_recursively(path)` takes a filepath and extracts everything that can be extracted, recursively. It ignores system files such as "\_\_MACOSX" and ".DS_Store".

If the filepath is a folder, it will look for zip files anywhere in that folder or subfolders. If the zip files contains zip files, those will be extracted too.

If the filepath is a .zip file (e.g. a Zenodo dataset downloaded), it will extract its content and look for zip files within it.

To use the CLI:

```
zip-utils extract --path /path/to/extract [--remove-zips]
```

where `/path/to/extract` is the filepath to extract and the optional flag `--remove-zips` removes the zip files after successful extraction.

## Zipping

Use of this function is recommended to upload to Zenodo: just run it on your dataset folder, open the output, and drag everything onto Zenodo. This avoids system files and redundant folder levels.
The function `zip_appropriately(input_path, output_path)` copies files from `input_path` to `output_path` and zips subfolders and places them in `output_path`.

To use the CLI:

```
zip-utils zip --source_dir /path/to/source/dir --target_dir /path/to/target/dir
```

## Cleaning

Generally speaking, if using the function `zip_appropriately` to zip, there should be no need to clean up archives, but otherwise the `main_cleaner` function can be used to remove any redundant level and any system file.

To use the function:
use either `main_cleaner(input_filepath, output_filepath=output_filepath)` to obtain the cleaned zip file at `output_filepath` or `main_cleaner(input_filepath, in_place=in_place)` to overwrite the input file.

To use the CLI:

`zip-utils clean /path/to/file --output-filepath /path/to/output` or `zip-utils clean /path/to/file --in-place`

# Excel utils
Functionalities to clean up specific recurring issues with .xlsx files. 

## Unpad
Tables padded with empty space around them reduce machine readability. You can process a single .xlsx or all the .xlsx inside a folder and its subfolders recursively and remove any padding. This preserves the cell color and borders, text formatting (e.g. text color, bold, italic) and particularly **formulas** even if across sheets.
To use the CLI:

```
excel-utils unpad --source /path/to/source/dir --target /path/to/target/dir
```

## Strip text
Cells with text with trailing spaces or spaces at the beginning can reduce machine readability. You can process a single .xlsx or all the .xlsx inside a folder and its subfolders recursively and remove any space at the beginning and end of text in a cell.
To use the CLI:
```
excel-utils strip-text --source /path/to/source/dir --target /path/to/target/dir
```

## Clean
This just consists in unpadding and stripping the text at the same time. You can run this function on your data before uploading to Zenodo.

To use the CLI:
```
excel-utils clean --source /path/to/source/dir --target /path/to/target/dir
```

## Unpad, strip, or both in a script
The functions process_folder and process_excel_file handle the unpadding, stripping, or both.
Use:

```process_folder(source_fol: str, dest_fol: str, unpad: bool, strip_text: bool)```

or 

```process_excel_file(filename: str, outname: str, unpad: bool, strip_text: bool)```

where the booleans `unpad` and `strip_text` control which operation(s) to perform.

## Check
You can use the function  check_folder_recursively to know if any file in it has issues of either padding or text that needs stripping.
Use:
``` check_folder_recursively(folder_path: str, check_padding: bool, check_strip: bool)```


To use the CLI:
```
excel-utils unpad folder  [--padding] [--strip]
```

where the two optional flags allow to check only for that type of issue.

