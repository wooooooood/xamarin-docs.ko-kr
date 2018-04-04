---
title: 바인딩 네이티브 프레임 워크
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 52b845d9e062eea6292528c5a40a74aa67d8e1b7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-native-frameworks"></a>바인딩 네이티브 프레임 워크

으로 네이티브 라이브러리는 배포 하는 경우에 따라 한 [프레임 워크](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html)합니다. 프레임 워크를 통해 정의 된 바인딩을 올바르게 목표 Sharpie 편리한 기능을 제공 된 `-framework` 옵션입니다.

예를 들어 바인딩는 [Adobe 창조적인 SDK 프레임 워크](https://creativesdk.adobe.com/downloads.html) iOS는 간단 하 게:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

일부 경우에는 프레임 워크를 지정 합니다는 **Info.plist** 프레임 워크는 SDK에 대 한 나타냅니다 컴파일해야 합니다. 이 정보가 있을 경우 / 더 명시적 `-sdk` 옵션 전달, 목표 Sharpie 프레임 워크의에서 유추 됩니다 **Info.plist** (중 하나는 `DTSDKName` 키나의 조합을 `DTPlatformName` 및 `DTPlatformVersion`키)입니다.

`-framework` 옵션 전달 되도록 명시적인 헤더 파일을 사용 하지 못하도록 합니다. 포괄적인 헤더 파일은 프레임 워크 이름을 기반으로 하는 규칙에 따라 선택 됩니다. 포괄적인 헤더를 찾을 수 없습니다, 프레임 워크를 바인딩할 목표 Sharpie 시도 하지 것입니다 clang 모든 프레임 워크 인수와 함께 텍스트 구문 분석을 올바른 포괄적인 헤더 파일을 제공 하 여 바인딩을 수동으로 수행 해야 경우 (같은 `-F`프레임 워크 검색 경로 옵션).

내부적으로 지정 `-framework` 는 바로 가기입니다. 다음 바인딩 인수는 동일는 `-framework` 위의 줄임 표기입니다.
특별 한 중요도 `-F .` clang에 제공 되는 프레임 워크 검색 경로 (명령의 일부로 필요한 공간 및 기간을 참고).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

