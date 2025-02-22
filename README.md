# Argos Translate
[Docs](https://argos-translate.readthedocs.io) | [Website](https://www.argosopentech.com)

Open source offline translation app written in Python. Uses OpenNMT for translations, SentencePiece for tokenization, Stanza for sentence boundary detection, and PyQt for GUI. Designed to be used either as a GUI application or as a Python library.

Argos Translate supports installing model files which are a zip archive with an ".argosmodel" extension that contains an OpenNMT CTranslate model, a SentencePiece tokenization model, a Stanza tokenizer model for sentence boundary detection, and metadata about the model. Pretrained models can be downloaded [here](https://drive.google.com/drive/folders/11wxM3Ze7NCgOk_tdtRjwet10DmtvFu3i). To install a model click "Manage packages" from the toolbar in the GUI and select your model file. By default models are stored in ~/.argos-translate, this can be changed in settings.py or by setting the value of the `$ARGOS_TRANSLATE_PACKAGES_DIR` environment variable. When running from a snap package models can be pre-packaged with the snap package or installed after installation to `$SNAP_USER_DATA`.

Argos Translate also manages automatically pivoting through intermediate languages to translate between languages that don't have a direct translation between them installed. For example, if you have a es -> en and en -> fr translation installed you are able to translate from es -> fr as if you had that translation installed. This allows for translating between a wide variety of languages at the cost of some loss of translation quality.

![Argos Translate Banner](/img/Banner.png)

## Models
Models available for download [here](https://drive.google.com/drive/folders/11wxM3Ze7NCgOk_tdtRjwet10DmtvFu3i), and are being trained using [this](https://github.com/argosopentech/onmt-models) training script. Models are also available for torrent download [in the /p2p directory](/p2p/all-argos-translate-models-2020-12-20.zip.torrent).

Currently, there are models available to translate between:
- Arabic
- Chinese
- English
- French
- German
- Italian
- Portuguese
- Russian
- Spanish

## Examples
### GUI
![Screenshot](/img/Screenshot.png)
![Screenshot2](/img/Screenshot2.png)


### Python
```
>>> from argostranslate import package, translate
>>> package.install_from_path('en_es.argosmodel')
>>> installed_languages = translate.load_installed_languages()
>>> [str(lang) for lang in installed_languages]
['English', 'Spanish']
>>> translation_en_es = installed_languages[0].get_translation(installed_languages[1])
>>> translation_en_es.translate("Hello World!")
'¡Hola Mundo!'
```

### Command Line ([Beta](https://github.com/argosopentech/argos-translate/issues/3))
```
$ argos-translate-cli --from-lang en --to-lang es "Hello World"
Hola Mundo
```

### [LibreTranslate](https://github.com/uav4geo/LibreTranslate) Web App ([Demo](https://libretranslate.com/))
![Web App Screenshot](img/WebAppScreenshot.png)

### [LibreTranslate](https://github.com/uav4geo/LibreTranslate) API
```
const res = await fetch("https://libretranslate.com/translate", {
	method: "POST",
	body: JSON.stringify({
		q: "Hello!",
		source: "en",
		target: "es"
	}),
	headers: {
		"Content-Type": "application/json"}
	});

console.log(await res.json());

{
    "translatedText": "¡Hola!"
}
```


## Installation
### Install from PyPI
Argos Translate is available from [PyPI](https://pypi.org/project/argostranslate/) and can be installed with pip.
```
python3 -m pip install --upgrade pip
python3 -m pip install argostranslate
```
#### Installation for Windows and MacOS
CTranslate, the inference engine for Argos Translate, [currently only distributes binaries for Linux](https://github.com/OpenNMT/CTranslate2/issues/133) so to install Argos Translate on Windows or MacOS you will need to build CTranslate from source.
### Install from Snap Store
Argos Translate is available from the Snap Store and auto installs a content snap to support translation between Arabic, Chinese, English, French, Russian, and Spanish. Additional languages can be installed from supplementary content snaps.

With [snapd installed](https://snapcraft.io/docs/installing-snapd):
```
sudo snap install argos-translate
``` 
[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-white.svg)](https://snapcraft.io/argos-translate)

Currently content snaps other than argos-translate-base-langs need to be manually connected but this [hopefully won't be necessary in the future](https://forum.snapcraft.io/t/store-request-for-argos-translate-snaps-argos-packages-plug-to-greedily-connect-to-available-slots/21503):
```
sudo snap connect argos-translate:argos-packages argos-translate-en-it:argos-packages
```

### Python source installation
#### Dependencies
Requires Python3, pip (which should come with Python3), and optionally virtualenv to keep Argos Translate's dependencies separate from other Python programs you have installed.

[Python Installation Instructions](https://wiki.python.org/moin/BeginnersGuide/Download)

On Ubuntu:
```
sudo apt-get update
sudo apt-get install -y python3
```
#### Install
1. Download a copy of this repo (this requires either installing git or downloading a zip from GitHub):
```
git clone https://github.com/argosopentech/argos-translate.git
cd argos-translate
```
2. Make a virtual environment to install into (optional):
```
python3 -m pip install --upgrade virtualenv # If virtualenv not already installed
virtualenv env
source env/bin/activate
```
3. Install this package with pip:
```
python3 -m pip install --upgrade pip
python3 -m pip install .
```

### Build and install snap package
1. Install [snapd](https://snapcraft.io/docs/installing-snapd) if it isn't already installed.
2. Using snapd install snapcraft and its dependency multipass:
```
sudo snap install multipass
sudo snap install snapcraft
```
3. Clone this repo:
```
git clone https://github.com/argosopentech/argos-translate.git
cd argos-translate
```
4. From the root directory of this project build the snap package:
```
SNAPCRAFT_BUILD_ENVIRONMENT_MEMORY=4G snapcraft
```
Any *unzipped* package files in package/ will be automatically included in the snap archive (and won't be able to be deleted by users of the snap).

Note, the build won't run with Snapcraft's default build memory of 2GB so you need to set the SNAPCRAFT_BUILD_ENVIRONMENT environment variable. [More on Snapcraft forum](https://forum.snapcraft.io/t/snapcraft-configuration-of-multipass-vm-arguments/9761).

5. Install the snap package:
```
sudo snap install --devmode argos-translate_<version information>.snap
```
### Run Argos Translate!
```
argos-translate
```

When installing with snap a .desktop file should also be installed which will make Argos Translate available from the desktop menu.

## Contributing
Contributions are welcome! 

## License
Dual licensed under either the [MIT License](https://github.com/argosopentech/argos-translate/blob/master/LICENSE) or [CC0](https://creativecommons.org/share-your-work/public-domain/cc0/).

