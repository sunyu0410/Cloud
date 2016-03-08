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
colorMode(img)                  # RGB / grayscale
storage.mode(img)               # Integer / float
dim(img)                        # Dimension

# Adjust pixels values as an array
img2 = img + 0.1                # Add brightness
img3 = img^2                    # Increase contrast
img4 = img[1:10,]               # Crop
img5 = rotate(img,45)           # Rotate
img6 = translate(img, c(50,50)) # Translate

# Filters
# filter2(): Fast 2D FFT convolution
# Create the kernel
# 'shape' can be box (default), disc, diamond, Gaussian or line
# 'step' specifies whether the kernel is binary
f = makeBrush(21, shape='disc', step=FALSE)
# Normalise the kernel
f = f/sum(f)
# Apply the filter
img_filtered = filter2(img, f)
```
