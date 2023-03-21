# Tone Mapping



One of the more simple tone mapping algorithms is Reinhard tone mapping 

## ToneMapping do auto exposure

1. Calculate image brightness: The first step is to calculate the brightness of the image, which can be achieved by calculating the average, maximum or other ways of the RGB color channel.

2. Calculate the average brightness: Take the average value of the brightness of the image, which represents the overall brightness of the image.

3. Calculate exposure compensation value: Calculate the exposure compensation value based on the average brightness, which is usually calculated using the following formula:

   exposureCompensation = log(0.5) / log(averageLuminance)

   where averageLuminance is the average brightness value.

4. Apply exposure compensation value: Apply the exposure compensation value to the RGB values of each pixel to adjust the brightness of the image.

5. Optional post-processing: Some post-processing can be applied to the adjusted image, such as tone mapping, sharpening, etc., to optimize the image quality.

## calculate image brightness

luminance = 0.2126 * red + 0.7152 * green + 0.0722 * blue

averageBrightness = (1 / n) * sum(luminance)

## Calculate exposure compensation value

exposureCompensation = log(0.5) / log(averageLuminance)

## Apply exposure compensation value

adjustedPixelValue = originalPixelValue * 2^(exposureCompensation)



##  automatic exposure hdr to ldr algorithm

1. Calculate the average brightness of the HDR image.
2. Calculate the exposure compensation value based on the average brightness of the image. This value determines the amount of brightness adjustment required to bring the HDR image to an appropriate LDR range.
3. Apply the exposure compensation value to each pixel of the HDR image.
4. Tone map the adjusted HDR image to create an LDR image. Tone mapping is a technique used to compress the dynamic range of the HDR image into a narrower range suitable for display on LDR devices.
5. Optionally, perform color grading and other post-processing to further enhance the appearance of the LDR image.

### Tone map the adjusted HDR image to create an LDR image formula

LDR pixel value = HDR pixel value * (1 + (HDR pixel value / white^2)) / (1 + HDR pixel value)

