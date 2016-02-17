<h1>Environment setup for cloud computing</h1>
<p>Environment setup to read and process Aperio SVS files in Ubuntu 14.04 (Trusty) amd64.</p>
<ol>
  <li>Python</li>
  <li>PIL</li>
  <li>OpenSlide</li>
  <li>openslide-python (the Python interface for OpenSlide)</li>
</ol>


```bat
:: Install python developer tools (to compile other packages)
:: and setuptools
apt-get install python-dev python-setuptools

:: Install OpenSlide
apt-get install openslide-tools

:: Install PIL (dependency of openslide-python)
:: Make sure to install the old version, 
:: because openslide-python calls the old functions in PIL
pip install Pillow==2.0

:: Install openslide-python
pip install openslide-python==1.0.0

:: If every component is right, Python is able to read the SVS files.
```

<p>Here's a script to test the installation. Download the sample SVS file from <a href="http://openslide.cs.cmu.edu/download/openslide-testdata/Aperio/CMU-1-Small-Region.svs">here</a>.</p>

```python
import openslide as op
slide = op.OpenSlide('CUM-1-Small-Region.svs')
r = slide.read_region((0,0), 0, (100,100))
print r
```
