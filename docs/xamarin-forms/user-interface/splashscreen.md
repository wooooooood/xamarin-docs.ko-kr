---
title: Xamarin 양식 시작 화면
description: 이 문서에서는 Xamarin.ios 응용 프로그램에 대 한 시작 화면을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetId: 338C8F60-90F2-4B3D-BB47-7F846AE8DC6C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/1/2019
ms.openlocfilehash: 2624c3f35a9cfac122a0b36ea8c1684d30f57824
ms.sourcegitcommit: 5110d1279809a2af58d3d66cd14c78113bb51436
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035775"
---
# <a name="xamarinforms-splash-screen"></a>Xamarin 양식 시작 화면

응용 프로그램은 초기화 프로세스를 완료 하는 동안 시작 지연이 발생 하는 경우가 많습니다. 개발자는 응용 프로그램을 시작 하는 동안 일반적으로 시작 화면 이라고 하는 브랜드 환경을 제공 하려고 할 수 있습니다. 이 문서에서는 Xamarin.ios 응용 프로그램에 대 한 시작 화면을 만드는 방법을 설명 합니다.

네이티브 시작 시퀀스가 완료 된 후에는 각 플랫폼에서 xamarin.ios가 초기화 됩니다. Xamarin.ios가 초기화 되었습니다.

- Android에서 `MainActivity` 클래스의 `OnCreate` 메서드
- IOS에서 `AppDelegate` 클래스의 `FinishedLaunching` 메서드
- UWP에서 `App` 클래스의 `OnLaunched` 메서드

시작 화면은 응용 프로그램이 시작 될 때 가능한 한 빨리 표시 되어야 하지만, Xamarin.ios는 시작 시퀀스에서 런타임에까지 초기화 되지 않습니다. 즉, 시작 화면은 각 플랫폼에서 Xamarin.ios 외부에서 구현 되어야 합니다. 다음 섹션에서는 각 플랫폼에서 시작 화면을 만드는 방법을 설명 합니다.

## <a name="xamarinforms-android-splash-screen"></a>Xamarin 양식 Android 시작 화면

Android에서 시작 화면을 만들려면 특수 테마를 사용 하 여 `MainLauncher`로 시작 `Activity`을 만들어야 합니다. 시작 `Activity`이 시작 되는 즉시 일반 응용 프로그램 테마를 사용 하 여 기본 `Activity`이 시작 됩니다.

Xamarin.ios의 시작 화면에 대 한 자세한 내용은 [Xamarin android 시작 화면](~/android/user-interface/splash-screen.md)을 참조 하세요.

## <a name="xamarinforms-ios-splash-screen"></a>Xamarin 양식 iOS 시작 화면

IOS의 시작 화면은 시작 화면 이라고 합니다. IOS에서 시작 화면을 만들려면 시작 화면의 UI를 정의 하는 Storyboard를 만든 다음 **info.plist**의 시작 화면으로 storyboard를 설정 해야 합니다.

Xamarin.ios에서 화면을 시작 하는 방법에 대 한 자세한 내용은 [Xamarin.ios 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)을 참조 하세요.

## <a name="xamarinforms-uwp-splash-screen"></a>Xamarin.ios UWP 시작 화면

UWP에서 appxmanifest.xml에는 **시작 화면** 하위 메뉴가 있는 **시각적 자산** 탭이 포함 되어 있습니다 **.** 시작 화면 그래픽은 다음 메뉴에서 지정할 수 있습니다.

[![ UWP에서 시작 화면 설정](splashscreen-images/uwp-splashscreen-cropped.png)](splashscreen-images/uwp-splashscreen.png#lightbox)

## <a name="related-links"></a>관련 링크

- [Xamarin Android 시작 화면](~/android/user-interface/splash-screen.md)
- [Xamarin.ios 시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)
