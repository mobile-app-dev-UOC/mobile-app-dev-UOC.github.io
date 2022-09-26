---
layout: default
title: 5.1. Measurement units
parent: 5. User interfaces
nav_order: 1
---

# 5.1. Measurement units 

Android user interfaces can be designed using different measurement units. The major difference between one unit and another is device independence. Achieving device independence means that the interface must be viewed in a very similar way regardless of device features. 

Device features are determined by two basic metrics:
- The size of the screen (width and height, measured in **pixels**).
- The pixel density of the device, measured in **pixels per inch (ppi)**.

To better understand this concept let us consider an example. 
- First, we have a low-end Android tablet that has 1200 x 1920 pixels, with a density of 224 ppi. The size of the screen is 26 x 16 cm.
- Now let’s consider a mid-range smartphone and which has 1080 x 2340 pixels, with a density of 398 ppi. Its actual size is 7.6 x 16.5 cm. 

The conclusion is that the tablet has a surface area of 416 cm2, much larger than that of the smartphone (125.4 cm2). However, the smartphone has many more pixels (2,527,200) than the tablet (2,304,000). This happens because the smartphone has a density of 398 ppi, higher than the 224 ppi tablet.

In other words, a pixel of our tablet occupies more square millimeters than a pixel of our smartphone. Given the above, let us consider the different types of units we can use when designing an application that shows a square. Let’s look at the following goal: we want the square to look the same (same size in millimeters, all sides the same) on any device.

## 5.1.1. Device-dependent units

**Pixel (px):** These are the pixels on the screen. Your actual size will depend on the pixel density of the device. If we paint a square in pixels and because of the different pixel densities, the square’s height and width will measure a different amount of millimeters on each device.

> ![Two squares measuring the same amount of pixels.](/images/05/square-pixels.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Two squares measuring the same amount of pixels.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


For example, if we define the size of a small button using pixels, it may not be visible on a device with a very high pixel density.

## 5.1.2. Device-independent units

These are the units commonly used in application development.

**Density-independent pixels (dp or dip):** 1 dp is 1 pixel on a screen with a density of 160 ppi. With this ratio, the number of pixels corresponding to a dp will change depending on the screen density of each device.

**Scaleable Pixel (sp):** These are very similar to dps and they are often used when font sizes are defined. This unit is used because it preserves the accessibility settings that the users have selected on their devices. That is, if the user has indicated that the font size should be twice larger, the size is first converted to dp and then multiplied by 2.

These device-independent units allow us to design interfaces that are proportionally rescaled based on the density of the different screens. This makes interfaces display correctly on devices with different densities.

> ![Two squares measuring the same amount of device-independent pixels.](/images/05/square-device-independent.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Two squares measuring the same amount of device-independent pixels.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## 5.1.3. Physical units

Alternatively, physical units can also be used:
- **Inch (in):** The quality of the UI depends on the physical size of the screen.
- **Millimeter (mm):**  The quality of the UI depends on the physical size of the screen.
- **Points (pt):** 1pt = 1/72 inches. As a result, this unit also depends on the physical format of the screen

These units allow elements to occupy the same space across all devices regardless of their pixel density. It is the same across all devices, but the number of pixels will be different: it will depend on the pixel density of each device.

Physical units are not used in practice because they do not allow the designer to work homogeneously across multiple devices. For example, the designer prepares background images for different pixel densities. If we define screens in centimeters, then the layout needs to use two types of units (centimeters and pixels). Instead, using dp allows everything to be measured in the same type of units.


