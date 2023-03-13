---
title: "Yocto: Add a package to Native SDK (eng)"
categories:
  - yocto
tags:
  - nativesdk
  - yocto
  - eng
comments: true
toc: true
toc_sticky: true
---


To add a package to the Yocto Project Native SDK, you can follow these general steps:

## 1. Create a new recipe for your package
Just like when adding a package to the Yocto Project SDK, you need to create a recipe for your package. However, in this case, you need to create a recipe that is specific to the Native SDK. You can create a new recipe file for your package and add it to the **`meta-nativesdk`** layer in your Yocto Project environment.

## 2. Build the package for the Native SDK
Once you have created a recipe for your package, you need to build it for the Native SDK. You can do this by using the **`nativesdk`** prefix when running **`bitbake`**. For example:

```
$ bitbake nativesdk-my-package
```

This will build your package specifically for the Native SDK, generating the necessary files and metadata.

## 3. Add the package to the Native SDK
  After the package has been built, you need to add it to the Native SDK. You can do this by adding the name of your package to the **`TOOLCHAIN_HOST_TASK`** variable in the appropriate configuration file. The configuration file may vary depending on which version of the Yocto Project you are using, but it is typically located in the **`conf/`** directory of your build directory.

```
TOOLCHAIN_HOST_TASK += "nativesdk-my-package"

```

## 4. Build the Native SDK
 Finally, you can build the Native SDK using the Yocto Project build system. This will include your package along with any other packages that are specified in the **`TOOLCHAIN_HOST_TASK`** variable.

```
$ bitbake nativesdk

```

This will build the Native SDK, including your **`nativesdk-my-package`** package.


Note that the specific steps may vary depending on the version of the Yocto Project you are using, and there may be additional configuration required to ensure that your package is properly included in the Native SDK. Additionally, you may need to consider any dependencies or compatibility issues with other packages in the Native SDK.
