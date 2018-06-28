---
title: Windows에서 Android 디버그에 어떤 USB 드라이버 필요 합니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: ee3f2b1e1ff6a3ac1bec2d73d4af6e740979aa04
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066873"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows에서 Android 디버그에 어떤 USB 드라이버 필요 합니까?

## <a name="finding-usb-drivers"></a>USB 드라이버를 찾는

Windows;에서 개발할 때 Android 장치에서 디버깅 하려면 호환 되는 USB 드라이버를 설치 해야 합니다. 여기에 설명 된 대로 Nexus 장치에 대 한 지원을 추가 하는 기본적으로 "Google USB 드라이버"를 포함 하는 Android SDK Manager: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

다른 장치에는 장치 제조업체에서 구체적으로 게시 하는 USB 드라이버가 필요 합니다. 가장 일반적인 제조업체에 대 한 일부 링크는이 가이드에 포함 됩니다. [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>대체 방법

제조사에 따라 필요한 정확한 USB 드라이버를 추적 하기 어려울 수 있습니다. Android 에뮬레이터를 사용 하 여 외부 테스트 서비스를 사용 하거나 포함 하 여 Windows에 Android 앱을 테스트 하기 위한 몇 가지 대안 개발 했습니다. 여기에는 다음과 같은 항목이 포함됩니다.

- [앱 센터 테스트](https://docs.microsoft.com/appcenter/test-cloud/) -클라우드 서비스를 수백 대의 실제 Android 장치에서 실행을 테스트 합니다.

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android 에뮬레이터에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)

