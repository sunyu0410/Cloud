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

<h1>Haralick's Textural Featrues</h1>
<p>To calculate the Haralick's textural features of an image <code>img</code> in EBImage, we need to specify another mask to indicate which pixels are considered as a block. The mask should have the same size as <code>img</code>. An example of a legal mask is as follows.</p>
```r
0 0 0 0 1 1 2 2 3 3 4 4
0 0 0 0 1 1 2 2 3 3 4 4
```
<p>In this case, 1) voxels with non-zeros will be considered for feature calculation and 2) the voxels with the same number will be regarded as a block. Haralick's features will be calculated on each block. Thus in this case, 4 sets of Haralick's features will be calculated.</p>
<p>Following the previous example, to calculate the Haralick's features for each cell, we can first create an mask which delineate the cells, then label the mask with a sequential integers and calculate the Haralick's features.</p>
```r
# Create the mask by thresholding the img
# thresh(x, w=5, h=5, offset=0.01)
#   x:        An Image object or an array.
#   w, h:     Width and height of the moving rectangular window.
#   offset:   Thresholding offset from the averaged value.
mask = thresh(img, 10, 10, 0.05)
# Slightly manipulate the mask mophologically
mask = opening(mask, makeBrush(5, shape='disc'))
# Label the mask (this is an important step)
mask = bwlabel(mask)
# Calculate the Haralick's features
hf = computeFeatures.haralick(mask, img)

# Calculating and visualising correlations
library(gplots)         # for heatmap.2()
library(RColorBrewer)   # for brewer.pal() to create color palettes

# Calculating the correlation matrix
correlationMatrix = cor(hf)

# Plot the heatmap without dendrogram
heatmap.2(correlationMatrix,
          dendrogram="none", 
          Rowv=FALSE, 
          symm=TRUE,
          main="Texture Features",
          scale="none", 
          col=brewer.pal(11,"RdBu"), 
          margins=c(7,7), 
          trace="none")
          
# Plot the heatmap with dendrogram
heatmap.2(correlationMatrix,
          key=FALSE,
          main="Haralick Texture Features",
          scale="none", 
          col=brewer.pal(11,"RdBu"), 
          margins=c(7,7), 
          trace="none")
```

<h1>Reference</h1>
<ol>
  <li>Haralick's features: http://earlglynn.github.io/RNotes/package/EBImage/Features-Haralick.html</li>
</ol>
