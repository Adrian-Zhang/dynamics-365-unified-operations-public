---
# required metadata

title: Improve performance by optimizing images
description: This topic explains how to improve website performance by optimizing image use in Microsoft Dynamics 365 Commerce.
author: mssle
ms.date: 09/17/2021
ms.topic: article
audience: Developer, IT Pro
ms.reviewer: josaw
ms.search.region: Global
ms.author: sheaton
ms.search.validFrom: 2021-09-20
---

# Improve performance by optimizing images

[!include[banner](../includes/banner.md)]

This topic explains how to improve website performance by optimizing image use in Microsoft Dynamics 365 Commerce during the upgrade or go-live process. 

## Configuration
This topic applies to the following configurations: 

- **Version**: Commerce 10.0.16 or later
- **Component**: B2C or B2B 
- **Feature area**: Commerce Website Performance

## Prerequisites
Install the Dynamics 365 Commerce online software development kit (SDK). For more information, see [Install the online SDK](../dev-itpro/ecommerce-platform-sdk.md).

## Optimize images

One of the biggest performance hits to a web page can be the downloading of images. You can reduce the size of your images and improve actual and perceived performance using the instructions below.

1. Use CSS to generate images for items such as buttons whenever possible.
2. Upload high quality/resolution marketing or product images to the Commerce site builder [Media Library](../dam-overview.md), where the image resizer will be used automatically during rendering.
3. Include width and height properties for each image
    1. For each module that uses images, open the **theme.settings.json** file in the **/src/settings** directory in the SDK install location
    2. Locate the module you wish to update. 
    3. Ensure the image properties include width and height parameters. Learn more: [Configure theme settings](../e-commerce-extensibility/configure-theme-settings.md)
4. Disable lazy loading for images. 
    1. Open Commerce Site Builder
    2. Navigate to the module with an image that should not be lazy loaded
    3. Select the checkbox next to "Disable Lazy Load"
    4. Save, preview, and publish your content.
  
> [!NOTE]
> For a **Product collection** module, ensure lazy loading is enabled by selecting the **Enable module lazy load** check box.

## Validate 

Use one or more of the following options to validate that the module was successfully excluded.

- **Description or Purpose**: Verify page performance
- **Steps to Run**:  Run performance tests before and after optimizing your images
- **Passing Result**: Performance is improved after optimizing your images



  
