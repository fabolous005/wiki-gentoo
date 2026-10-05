<!-- source: https://wiki.gentoo.org/wiki/Ultimate_Vocal_Remover_(UVR) | group: Gentoo Wiki (Main) | wiki-title: Ultimate Vocal Remover (UVR) -->
---
title: Ultimate Vocal Remover (UVR)
url: https://wiki.gentoo.org/wiki/Ultimate_Vocal_Remover_(UVR)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-23"
fingerprint: "60093e7f85f5c413"
license: CC BY-SA 4.0
---

# Ultimate Vocal Remover (UVR)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ultimate Vocal Remover** (UVR) is software that separates vocals from instrumentals in music audio files using deep neural networks and separation models.

## Installation

### Emerge

At this time, there is no known ebuild for this software. It can be installed manually as follows.

### Manual installation

Reference: [https://github.com/Anjok07/ultimatevocalremovergui/?tab=readme-ov-file#linux-installation](https://github.com/Anjok07/ultimatevocalremovergui/?tab=readme-ov-file#linux-installation)

#### Portage packages

The [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python) package needs to have the [tk](https://packages.gentoo.org/useflags/tk) [USE flag.](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/package.use/package.use`**

**package.use example**

If it does not, re-emerge it:

`root #``emerge --ask --oneshot dev-lang/python`
If there are multiple versions of [dev-lang/python](https://packages.gentoo.org/packages/dev-lang/python) installed, be sure that the Python version that will be re-emerged corresponds to the current Python version.

`user $``python --version`
If it isn't, specify the version:

`root #``emerge --ask --oneshot =dev-lang/python-3.11.8_p1`
#### Get source code

Obtain the UVR source code. Either with [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git), or downloading a zip archive. Do this as a non-root user.

With git:

or download an archive:

and unpack the zip file (emerge [app-arch/zip](https://packages.gentoo.org/packages/app-arch/zip) if necessary).

#### Build from source

Enter the source directory, which is either the path to the `ultimatevocalremovergui` repository that was cloned, or the directory where the zip file was unzipped.

`user $``cd ultimatevocalremovergui`
Modify the `requirements.txt` file to remove an unused dependency which causes installation problems:

`user $``${EDITOR} requirements.txt`
**`./requirements.txt`**

**Python dependency modifications**

It may be necessary to modify numpy to a higher version number:

**`./requirements.txt`**

**Python dependency modifications**

Create a [Python virtual environment](https://docs.python.org/3/library/venv.html):

`user $``python -m venv venv`
Activate the virtual environment:

`user $``. venv/bin/activate`
Update the `wheel` package (safe to do if no upgrade is needed):

`user $``pip install --upgrade wheel`
Make the Python dependency installation script runnable:

`user $``chmod +x install_packages.sh`
and run it:

`user $``./install_packages.sh`
Make sure there are no errors (and not just at the end of the output).

## Usage

### Invocation

`user $``python UVR.py`
To exit the Python virtual environment when done:

`user $``deactivate`
## Configuration

Configuration is done within the GUI. There are many ways to use UVR, but here is one way to configure it to do basic processing.

- Choose an input audio file.
- Choose an output directory.
- Under "CHOOSE PROCESS METHOD", select MDX-Net.
- Leave SEGMENT SIZE and OVERLAP as is (256, and Default).
- Under CHOOSE MDX-NET MODEL, select Download More Models. A settings window will appear.
- Download the "UVR-MDX-NET Inst Main" model.
- Close the settings window.
- Ensure the "UVR-MDX-NET Inst Main" model is selected in the dropdown.

Click the "Start Processing" button.
