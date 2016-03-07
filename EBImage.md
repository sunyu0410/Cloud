<H1>Installation in R</h1>
```r
source("http://bioconductor.org/biocLite.R")
biocLite()
biocLite('EBImage')
```

<h1>Read and manipulate images</h1>
```r
# Read a tiff image included in the EBImage package
# [,,1] means the first layer of the tiff, not the color channel
img = readImage(system.file('images', 'nuclei.tif', package='EBImage'))[,,1]

# Show in R
image(img)

# Display image in a browser
display(img, title="Cell nucleus")

# Show image information
print(img)

# Get information of the image
# RGb/Grayscale
colorMode(img)      # RGB / grayscale
storage.mode(img)   # Integer / float
dim(img)            # Dimension
