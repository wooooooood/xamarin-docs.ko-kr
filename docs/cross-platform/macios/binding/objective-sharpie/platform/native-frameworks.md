---
title: 네이티브 프레임워크 바인딩
description: 이 문서에서는 목적 Sharpie의 프레임 워크 옵션을 사용 하 여 프레임 워크로 배포 된 라이브러리에 대 한 바인딩을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 0733e2b406f5032a2e2717df66c96dcb28301f09
ms.sourcegitcommit: 24883be72e485e5311dd0eb91f9a22f78eeec11a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/17/2020
ms.locfileid: "77374130"
---
# <a name="binding-native-frameworks"></a>네이티브 프레임워크 바인딩

네이티브 라이브러리가 [프레임 워크로](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)배포 되는 경우도 있습니다. 목표 Sharpie는 `-framework` 옵션을 통해 올바르게 정의 된 프레임 워크를 바인딩하는 편리한 기능을 제공 합니다.

예를 들어 iOS 용 [Adobe CREATIVE SDK 프레임 워크](https://creativesdk.adobe.com/downloads.html) 를 바인딩하는 것은 간단 합니다.

```
$ sharpie bind \
    -framework ./AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

경우에 따라 프레임 워크는 info.plist을 지정 합니다 **.** 이는 프레임 워크를 컴파일해야 하는 SDK를 나타냅니다. 이 정보가 존재 하 고 명시적 `-sdk` 옵션이 전달 되지 않은 경우 목표 Sharpie는 프레임 워크의 info.plist (`DTSDKName` 키 또는 `DTPlatformName`와 `DTPlatformVersion` 키의 조합)에서이 정보를 유추 합니다 **.**

`-framework` 옵션은 명시적 헤더 파일을 전달 하는 것을 허용 하지 않습니다. 파라솔 헤더 파일은 프레임 워크 이름을 기반으로 하는 규칙에 의해 선택 됩니다. 파라솔 헤더를 찾을 수 없는 경우 목표 Sharpie는 프레임 워크를 바인딩하지 않으며, clang에 대 한 프레임 워크 인수 (예: `-F` framework 검색 경로 옵션)와 함께 구문 분석할 올바른 파라솔 헤더 파일을 제공 하 여 바인딩을 수동으로 수행 해야 합니다.

내부적으로 `-framework` 지정 하는 것은 바로 가기입니다. 다음 바인딩 인수는 위의 `-framework` 약어와 동일 합니다.
특별 한 중요도는 clang에 제공 되는 `-F .` framework 검색 경로입니다 (명령의 일부로 필요한 공백과 마침표를 적어 둡니다.).

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    ./AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
