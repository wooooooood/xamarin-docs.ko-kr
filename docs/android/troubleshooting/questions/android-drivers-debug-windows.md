---
title: Windows에서 Android를 디버그 하는 USB 드라이버 필요 합니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8b996a1cc89acedc47c7169ec579dfb99dae788f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114537"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows에서 Android를 디버그 하는 USB 드라이버 필요 합니까?

## <a name="finding-usb-drivers"></a>USB 드라이버 찾기

Windows;에서 개발할 때 Android 장치에서 디버그 하려면 호환 USB 드라이버를 설치 해야 합니다. 여기에 설명 된 대로 Nexus 장치에 대 한 지원을 추가 하는 기본적으로 "Google USB 드라이버"를 포함 하는 Android SDK Manager: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

다른 장치에는 특히 장치 제조업체에서 게시 하는 USB 드라이버 필요 합니다. 가장 일반적인 제조업체에 대 한 일부 링크는이 가이드에 포함 됩니다. [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>대안

제조사에 따라 필요한 정확한 USB 드라이버를 추적 하는 것이 어려울 수 있습니다. Android 에뮬레이터를 사용 하 여 외부 테스트 서비스를 사용 하거나 포함 하 여 Windows에서 Android 앱을 테스트 하는 것에 대 한 몇 가지 대안 개발. 여기에는 다음과 같은 항목이 포함됩니다.

- [App Center 테스트](https://docs.microsoft.com/appcenter/test-cloud/) -수백 대의 실제 Android 장치에서 실행 되는 서비스를 테스트 하는 클라우드입니다.

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)

