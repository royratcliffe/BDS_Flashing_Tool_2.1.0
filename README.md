# BDS Flashing Tool

A Windows utility for preparing an Intel HEX application image and
launching the bundled BDS flashing tool. The repository includes the
Python launcher, its configuration files, the flashing executable, and
the supporting PCAN/BODAS libraries.

## Before you start

- Use this tool only with an ECU and firmware image you are authorised
  to flash.
- Confirm that the image is intended for the target ECU and memory
  block.
- Keep the ECU and programming interface powered and connected
  throughout flashing. An interrupted flash may leave the ECU unusable.
- Back up the original HEX file before running the Python launcher: it
  sorts and rewrites the configured application HEX file **in place**.
- Install/configure any required PCAN interface drivers and connect the
  supported hardware.

## Configuration

Edit `config.ini` before running the launcher. Set the file paths to
files available on your machine:

| Setting | Purpose |
|------------------------------------|------------------------------------|
| `BDS_COM_BAUD_RATE` | Communication baud rate |
| `BDS_ECU_SERVER_ID` | ECU server identifier |
| `BDS_TOOL_CLIENT_ID` | Flashing tool client identifier |
| `BDS_SELECT_MEM_BLOCK_NAME` | Memory block to select |
| `BDS_MEM_CB_PATH` | Control/boot image path |
| `BDS_MEM_ASW_DS_PATH` | Application image path; this HEX file is preprocessed in place |
| `BDS_MEM_CRA_PATH` | CRA/key file path |
| `BDS_FLASH_DEBUG_LOG_PATH` | Debug log output path |
| `BDS_SELECT_TOOL_SERVICE_MODE` | Tool service-mode selection |
| `BDS_FLASH_PASSWORD` | Flashing password |

The checked-in paths are examples for a particular local setup. Replace
them with valid paths on your workstation. Keep passwords and other
credentials out of source control; if the configured password is
sensitive, provide it locally and do not share the edited configuration.

`FlashingUtilities\python_script_config.ini` points the Python launcher
to `bds_flashing_tool_script.bat`. Change `BDS_FLASH_BAT_PATH` there
only if you move or rename the batch file.

## Run

The Python launcher uses Python 2's `ConfigParser` module; note the
title case, a version-2 Python module, renamed to lowercase in Python 3.
From the repository directory, run it with a Python 2 interpreter:

``` bat
py -2 bds_flash_script.py
```

The launcher reads `BDS_MEM_ASW_DS_PATH`, sorts the HEX records by
extended linear address, writes the result back to that same file, and
then starts the configured batch file. The batch file launches
`FlashingUtilities\bds_flashing_tool.exe` with `config.ini`.

You can also run `bds_flashing_tool_script.bat` directly to launch the
bundled executable without the Python HEX-preprocessing step.

## Files

- `bds_flash_script.py` — HEX preprocessing and batch-file launcher.
- `config.ini` — flashing parameters and firmware/key/log paths.
- `FlashingUtilities\python_script_config.ini` — batch-file location for
  the Python launcher.
- `bds_flashing_tool_script.bat` — starts the bundled flashing
  executable.
- `FlashingUtilities\` — executable and runtime/interface libraries.
- `Release Notes - BDS Flashing Tool.pdf` — release notes.
