<h1>Environment setup for cloud computing</h1>
<p>Environment setup to read and process Aperio SVS files in Ubuntu 14.04 (Trusty) amd64.</p>

<h2>Python</h2>

```bat
:: Install python developer tools (to compile other packages)
:: and setuptools
apt-get install python-dev python-setuptools
```

<h2>OpenSlide</h2>
```bat
:: Install OpenSlide
apt-get install openslide-tools
```

<h2>PIL</h2>

```bat
:: Install PIL (dependency of openslide-python)
:: Make sure to install the old version, 
:: because openslide-python calls the old functions in PIL
pip install Pillow==2.0
```

<h2>openslide-python (the Python interface for OpenSlide)</h2>

```
:: Install openslide-python
pip install openslide-python==1.0.0

:: If every component is right, Python is able to read the SVS files.
```

<h2>Test</h2>
<p>Here's a script to test the installation. Download the sample SVS file from <a href="http://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1-Small-Region.svs">here</a>.</p>

```python
import openslide as op
slide = op.OpenSlide('CUM-1-Small-Region.svs')
r = slide.read_region((0,0), 0, (100,100))
print r
```

<h2>R</h2>

```bat
:: To install R
apt-get update
apt-get install r-base

:: Run non-interactively in background
R CMD BATCH SVM_for_Cloud.r SVM_for_Cloud_out.txt &

:: Monitor progress
tail SVM_for_Cloud_out.txt
:: or
top
```

<h2>Trasnfering files between local and cloud</h2>

```bat
:: To upload files to the server
scp -i mykeypair.pem mrhist0* ubuntu@130.56.254.253:~/

:: To download files from the server
scp -i mykeypair.pen ubuntu@130.56.254.253:~/myfile <local_path>
```

<h2>Download from a url</h2>

```bat
curl -o myfile.txt <url>
```

<h2>Unzip</h2>

```bat
:: tar
tar -vxf myfile.tar

:: unzip
unzip myfile.zip
```
