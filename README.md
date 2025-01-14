# jupyterlab_nodeeditor

![Github Actions Status](https://github.com/matthewturk/jupyterlab_nodeeditor/workflows/Build/badge.svg)

Edit nodes with Rete and jupyterlab


This extension is composed of a Python package named `jupyterlab_nodeeditor`
for the server extension and a NPM package named `jupyterlab_nodeeditor`
for the frontend extension.


## Requirements

* JupyterLab >= 3.0

## Install

```bash
pip install jupyterlab_nodeeditor
```


## Troubleshoot

If you are seeing the frontend extension, but it is not working, check
that the server extension is enabled:

```bash
jupyter server extension list
```

If the server extension is installed and enabled, but you are not seeing
the frontend extension, check the frontend extension is installed:

```bash
jupyter labextension list
```


## Contributing

### Development install

It is recommended to first install JLNE and all of its dependencies on a fresh environment to ensure it is stable.

1) Install ![jupyterlab](https://jupyter.org/install), ipykernel (conda_env is your current environment name), ![NodeJS](https://nodejs.org/en/download/package-manager/), and pyyaml.
```
conda install jupyterlab
conda install nodejs
pip install pyyaml
python -m ipykernel install --name conda_env --user
```
2) Clone the repo to your local environment and change the working directory to the jupyterlab_nodeeditor directory.
3) Install the package via pip
```
pip install -e .
```
4) Use jlpm to install javascript deps
```
jlpm install
```
7) Link your development version of the extension with JupyterLab
```
jupyter labextension develop . --overwrite
```

The `jlpm` command is JupyterLab's pinned version of
[yarn](https://yarnpkg.com/) that is installed with JupyterLab. You may use
`yarn` or `npm` in lieu of `jlpm` below.

```bash
# Rebuild extension Typescript source after making changes
jlpm run build
```

You can watch the source directory and run JupyterLab at the same time in different terminals to watch for changes in the extension's source and automatically rebuild the extension.

```bash
# Watch the source directory in one terminal, automatically rebuilding when needed
jlpm run watch
# Run JupyterLab in another terminal
jupyter lab
```

With the watch command running, every saved change will immediately be built locally and available in your running JupyterLab. Refresh JupyterLab to load the change in your browser (you may need to wait several seconds for the extension to be rebuilt).

By default, the `jlpm run build` command generates the source maps for this extension to make it easier to debug using the browser dev tools. To also generate source maps for the JupyterLab core extensions, you can run the following command:

```bash
jupyter lab build --minimize=False
```

### Uninstall

```bash
pip uninstall jupyterlab_nodeeditor
```
