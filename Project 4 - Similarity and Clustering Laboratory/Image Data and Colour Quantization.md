# Image Data and Colour Quantization

## Overview

A digital colour image can be represented as a three-dimensional NumPy array:

```text
(height, width, channels)
```

For an RGB image, the number of channels is three:

```text
[R, G, B]
```

Each pixel is therefore a point in a three-dimensional colour space.

## RGB values

The loaded images use the `uint8` data type. Each channel is stored using an integer between 0 and 255.

Examples:

```text
[255, 0, 0]     → red
[0, 255, 0]     → green
[0, 0, 255]     → blue
[255, 255, 255] → white
```

Converting all supported images to RGB gives the rest of the project a consistent representation, even when the original file uses another image mode.

## From image to pixel matrix

K-means, KNN, and PCA expect rows of samples rather than a height-by-width image.

The project reshapes:

```text
(height, width, 3)
```

into:

```text
(number_of_pixels, 3)
```

where:

```text
number_of_pixels = height × width
```

For example, a `100 × 200` image becomes:

```text
(20,000, 3)
```

Each row is one RGB pixel. The original height and width are retained so that the pixel matrix can later be reconstructed into an image.

## Loading and validating images

The image-input pipeline:

1. Checks that the path exists.
2. Accepts PNG and JPEG files.
3. Opens the image once to verify that it can be decoded.
4. Reopens it because Pillow’s `verify()` operation leaves the first image object unsuitable for normal loading.
5. Resizes the image if it exceeds the configured dimensions.
6. Converts it to RGB.
7. Returns a NumPy array.

The validation and resizing steps protect the application from unsupported, corrupted, empty, or excessively large inputs.

## JPEG is lossy

Opening and decoding a JPEG does not necessarily reproduce the exact RGB values that existed before the file was compressed.

JPEG encoding may slightly change pixel values to reduce file size. PNG normally preserves exact pixel values.

This matters when testing image round trips: the project verifies that its array transformations preserve the decoded image, not that a JPEG contains its original pre-compression pixels.

## Colour quantization

Colour quantization reduces the number of distinct colours used to represent an image.

The project treats every RGB pixel as a point and replaces it with one representative palette colour:

```text
Original RGB pixels
        ↓
Group similar colours
        ↓
Learn representative colours
        ↓
Replace each pixel with its representative
        ↓
Reduced-colour image
```

With `k = 8`, the goal is to represent the image with at most eight learned colours.

## Naive baseline

The baseline selects `k` existing image pixels as a fixed random palette and assigns every pixel to its nearest selected colour.

It does not improve the palette after selection:

```text
Random palette → one assignment step → reconstructed image
```

K-means provides a stronger method because it repeatedly updates each representative colour using the pixels assigned to it.

## Compression claim

Reducing the number of unique colours does not automatically guarantee a proportional reduction in the saved file size.

Actual file size also depends on:

- PNG or JPEG encoding;
- compression settings;
- image dimensions;
- repeated pixel patterns;
- file metadata.

The project therefore makes a direct palette-reduction claim and reports reconstruction quality separately.

## Key takeaway

An image can become a normal machine-learning dataset by reshaping it into rows of RGB features. The mathematical algorithms operate on the pixel matrix, while the original image shape is needed only for reconstruction and display.

## Related notes

- [Distance Metrics, Broadcasting, and KNN](Distance%20Metrics,%20Broadcasting,%20and%20KNN.md)
- [K-Means from Scratch and Initialization](K-Means%20from%20Scratch%20and%20Initialization.md)
- [Compression Evaluation and k Trade-offs](Compression%20Evaluation%20and%20k%20Trade-offs.md)