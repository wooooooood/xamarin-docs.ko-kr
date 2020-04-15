---
title: Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 21fd8eff64d374e52e64194524a8c096cdf4d90e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027034"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?

## <a name="finding-usb-drivers"></a>USB 드라이버 찾기

Windows에서 개발할 때 Android 디바이스에서 디버그하려면 호환되는 USB 드라이버를 설치해야 합니다. Android SDK 관리자에는 [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)에서 설명하는 것처럼 기본적으로 Nexus 디바이스에 대한 지원을 추가하는 “Google USB 드라이버”가 포함됩니다.

다른 디바이스에는 해당 디바이스 제조업체에서 제공하는 USB 드라이버가 필요합니다. [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html) 가이드에는 자주 사용되는 제조업체 링크가 포함되어 있습니다.

## <a name="alternatives"></a>대안

제조업체에 따라, 필요한 정확한 USB 드라이버를 찾기가 어려울 수 있습니다. Windows에서 개발한 Android 앱을 테스트하기 위한 대안에는 Android 에뮬레이터를 사용하거나 외부 테스트 서비스를 사용하는 방법이 있습니다. 여기에는 다음과 같은 항목이 포함됩니다.

- [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) - 수백 가지의 실제 Android 디바이스에서 실행되는 클라우드 테스트 서비스입니다.

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)
