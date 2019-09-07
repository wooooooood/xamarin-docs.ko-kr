---
title: Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8be3f1b8803aa7e052ebc89af51dad3b659f95f5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757326"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Windows에서 Android를 디버그해야 하는 USB 드라이버는 무엇인가요?

## <a name="finding-usb-drivers"></a>USB 드라이버 찾기

Windows에서 개발할 때 Android 장치에서 디버깅 하려면 호환 되는 USB 드라이버를 설치 해야 합니다. Android SDK 관리자는 기본적으로 "Google USB 드라이버"를 포함 합니다. 여기에는 여기에 설명 된 대로 Nexus 장치에 대 한 지원이 추가 됩니다.[https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

다른 장치에는 장치 제조업체에서 특별히 게시 한 USB 드라이버가 필요 합니다. 이 가이드에는 가장 일반적인 제조업체를 위한 링크가 포함 되어 있습니다.[https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>대체 형식

Manfacturer에 따라 필요한 정확한 USB 드라이버를 추적 하기 어려울 수 있습니다. Android 에뮬레이터를 사용 하거나 외부 테스트 서비스를 사용 하는 등 Windows에서 개발한 Android 앱 테스트를 위한 몇 가지 대안입니다. 여기에는 다음과 같은 항목이 포함됩니다.

- 수백 개의 실제 Android 장치에서 실행 되는 테스트 클라우드 테스트 서비스를 [App Center](https://docs.microsoft.com/appcenter/test-cloud/) 합니다.

- [Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Android Emulator에서 디버깅](~/android/deploy-test/debugging/debug-on-emulator.md)
