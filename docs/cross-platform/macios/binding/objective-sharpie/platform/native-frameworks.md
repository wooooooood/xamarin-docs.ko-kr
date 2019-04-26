---
title: 네이티브 프레임워크 바인딩
description: 이 문서에서는 목표 Sharpie를 사용 하는 방법을 설명의-프레임 워크로 배포 프레임 워크 라이브러리에 대 한 바인딩을 만들 수 있습니다.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199130"
---
# <a name="binding-native-frameworks"></a>네이티브 프레임워크 바인딩

네이티브 라이브러리는 배포 되는 경우에 따라 한 [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)합니다. 프레임 워크를 통해 정의 된 바인딩을 올바르게 목표 Sharpie 주는 편리한 기능을 제공 합니다 `-framework` 옵션입니다.

예를 들어, 바인딩를 [Adobe Creative SDK 프레임 워크](https://creativesdk.adobe.com/downloads.html) iOS는 간단 합니다.

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

일부 경우에서는 프레임 워크를 지정 합니다는 **Info.plist** 프레임 워크는 SDK에 대해 나타내는 컴파일해야 합니다. 이 정보가 존재 하는 경우 및 더 명시적 `-sdk` 옵션은 전달 된, 목표 Sharpie 프레임 워크의에서 유추 됩니다 **Info.plist** (중 하나는 `DTSDKName` 키나 조합을 `DTPlatformName` 및 `DTPlatformVersion`키).

`-framework` 옵션 전달할 명시적 헤더 파일을 허용 하지 않습니다. 포괄적인 헤더 파일은 프레임 워크 이름을 기반으로 하는 규칙에 따라 선택 됩니다. 포괄적인 헤더를 찾을 수 없습니다, 목표 Sharpie는 프레임 워크를 바인딩합니다 하려고 하지 않습니다 하 고, 바인딩을 clang에 대 한 모든 프레임 워크 인수와 함께 텍스트를 구문 분석에 올바른 포괄적인 헤더 파일을 제공 하 여 수동으로 수행 해야 합니다 (같은 합니다 `-F`프레임 워크 검색 경로 옵션).

내부적으로 지정 `-framework` 바로 가기 인 합니다. 다음 바인딩 인수와 동일 합니다 `-framework` 위의 줄임.
특별 한 중요도입니다는 `-F .` clang를 제공 하는 프레임 워크 검색 경로 (명령의 일부로 필요한 공간 및 시간, 참고).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>관련 링크

- [Xamarin University 과정: Objective-c 바인딩 라이브러리를 빌드](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University 과정: 목표 Sharpie로는 Objective-c 바인딩 라이브러리 빌드](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

