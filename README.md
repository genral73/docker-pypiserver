## pypiserver - minimal PyPI server for use with pip/easy_install

> ``pypiserver`` is a minimal PyPI_ compatible server for ``pip`` or ``easy_install``.
> It is based on bottle_ and serves packages from regular directories.
> Wheels, bdists, eggs and accompanying PGP-signatures can be uploaded either with ``pip``, ``setuptools``, ``twine``, ``pypi-uploader``, or simply copiedwith ``scp``.


#### Quickstart: Installation and Usage [ Using the Docker Image ]

1. Copy some packages into your ``~/packages`` folder and then get pull the``pypiserver`` image and running::

2. To serve packages from a directory on the host, e.g. ~/packages
```bash
docker run -p 8080:8080 -v ~/packages:/data/packages pypiserver/pypiserver:latest
```
  - To  authenticate against a local ``.htpasswd`` file::
```bash
  docker run -p 80:8080 -v ~/.htpasswd:/data/.htpasswd pypiserver/pypiserver:latest -P .htpasswd packages
```
  - You can now access your pypiserver at 192.168.100.10:8080 in a web browser.

3. From the client computer, type this:
###### Download and install hosted packages.
```bash
pip3 install --extra-index-url http://192.168.100.10:8080/simple/ --trusted-host 192.168.100.10 babel
```
###### Search hosted packages, ``` Note that pip search does not currently work with the /simple/ endpoint```.
```bash    
pip3 search --index http://192.168.100.10:8080 babel
```

## See also ``Client-side configurations`` for avoiding tedious typing:

> Always specifying the the pypi url on the command line is a bit cumbersome. Since ``pypiserver`` redirects ``pip/easy_install`` to the ``pypi.org`` index if it doesn't have a requested package, it is a good idea to configure them to always use your local pypi index.

#### Configuring ``pip``
For ``pip`` command this can be done by setting the environment variable
``PIP_EXTRA_INDEX_URL and PIP_TRUSTED_HOST`` in your ``.bashrc or .profile or .zshrc``::
```bash   
nano ~/.bashrc
export PIP_EXTRA_INDEX_URL=http://192.168.100.10:8080/simple/
export PIP_TRUSTED_HOST=192.168.100.10
```
or by adding the following lines to ``~/.pip/pip.conf``::
```bash
mkdir ~/.pip
nano ~/.pip/pip.conf
[global]
extra-index-url = http://192.168.100.10:8080/simple/
trusted-host = 192.168.100.10
```
#### Configuring ``easy_install``
For ``easy_install`` command you may set the following configuration in
``~/.pydistutils.cfg``::
```bash
nano ~/.pydistutils.cfg
[easy_install]
index_url = http://192.168.100.10:8080/simple/
```
> For more details [https://github.com/pypiserver/pypiserver](https://github.com/pypiserver/pypiserver).
