---
title: Ice Cream Sandwich 기능
description: 이 문서에서는 다양 한 Android 4 api-Ice Cream Sandwich 응용 프로그램 개발자에 게 제공 되는 새 기능 설명 합니다. 몇 가지 새로운 사용자 인터페이스 기술에 설명 하 고 다양 한 Android 4 장치 및 응용 프로그램 간의 데이터 공유를 제공 하는 새 기능을 검사 합니다.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: adb53af9d2e6707cb1fca3c59af63db76e5346d5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102622"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich 기능

_이 문서에서는 다양 한 Android 4 api-Ice Cream Sandwich 응용 프로그램 개발자에 게 제공 되는 새 기능 설명 합니다. 몇 가지 새로운 사용자 인터페이스 기술에 설명 하 고 다양 한 Android 4 장치 및 응용 프로그램 간의 데이터 공유를 제공 하는 새 기능을 검사 합니다._

## <a name="overview"></a>개요

주요 받았던 Android 운영 체제의 나타내며 다양 한 중요 한 변경 내용과 업그레이드를 포함 하 여 포함 하는 android OS 버전 4.0 (API 수준 14):

-   **사용자 인터페이스 업데이트** – 몇 가지 새로운 UI 기능에는 개발자에 게 더 전원 제공 하 고 응용 프로그램 사용자를 만들 때 유연성 인터페이스입니다. 이러한 새 기능에 포함: `GridLayout` , `PopupMenu` 를 `Switch` 위젯 및 `TextureView` 합니다. 
-   **향상 된 하드웨어 가속** – 2D 렌더링 이제 Android 모든 컨트롤에 대 한 GPU에서 이루어집니다. 또한 하드웨어 가속, Android 4.0 용으로 개발 하는 모든 응용 프로그램에서 기본적으로는. 
-   **새 데이터 Api** – 이전에 공식적으로 액세스할 수 없으므로, 일정 데이터 및 장치 소유자의 사용자 프로필과 같은 데이터에 대 한 새 액세스는 합니다. 
-   **앱 데이터 공유** – 응용 프로그램 및 장치 간의 데이터 공유는 이제 기술을 통해 그 어느 때 보다 쉽게 같은 합니다 `ShareActionProvider` , 쉽게 작업 모음에서 공유 작업을 만듭니다 및 *Android 보* 에 대 한 *통신 NFC (근거리)* 는 간단히 서로 가까이 있는 장치에서 데이터를 공유할 수 있습니다. 


이 문서에서는 이러한 기능 및 Android 4.0 API에 적용 된 다른 변경 내용을 소개 하겠습니다 및 Xamarin.Android를 사용 하 여 각 기능을 사용 하는 방법을 설명 합니다.

## <a name="user-interface-features"></a>사용자 인터페이스 기능

다양 한 새로운 사용자 인터페이스 기술 포함 하 여 Android 4를 사용 하 여 사용할 수 있습니다.

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – 컨트롤의 2D 모눈 레이아웃 조정을 지원 합니다. 
-   **[위젯 전환](~/android/user-interface/controls/switch.md)**  – 전환 허용 ON 또는 OFF입니다. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – 비디오 및 OpenGL 콘텐츠 뷰 내에서 사용 하도록 설정 합니다. 
-   **[탐색 모음](~/android/user-interface/controls/navigation-bar.md)**  – 홈, 및 다중 작업 백업용 가상 단추가 포함 되어 있습니다. 


또한 다른 UI 요소 향상 되어 같은 `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`, 이제 쉽게 작업할 수 이며 탭에서 더 세련 된 모양이 있는 합니다.

## <a name="sharing-features"></a>공유 기능

Android 4 장치 및 응용 프로그램에서 데이터를 공유할 수 있도록 하는 몇 가지 새로운 기술을 포함 합니다. 또한 다양 한 유형의 일정 정보 및 장치 소유자의 사용자 프로필과 같은 이전에 불가능 했던 데이터에 대 한 액세스를 제공 합니다. 이 섹션에서는 검토 다양 한 기능에서 제공 하는 Android 4 해당 주소 비롯 한 다양 한이 분야:

-  **[Android 빔](~/android/platform/android-beam.md)**  – 허용 데이터 NFC를 통해 공유 합니다.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  – 개발자가 작업 모음에서 공유 작업을 지정할 수 있도록 공급자를 만듭니다. 
-   **[사용자 프로필](~/android/user-interface/user-profile.md)**  – 장치 소유자의 프로필 데이터에 대 한 액세스를 제공 합니다. 
-   **[API를 달력](~/android/user-interface/controls/calendar.md)**  – 달력 공급자 로부터 액세스 일정 데이터를 제공 합니다. 

## <a name="x86-emulators"></a>x86 에뮬레이터

ICS x86 사용 하 여 개발을 아직 지원 하지 않습니다 에뮬레이터. x86 에뮬레이터 Android 2.3.3, API 수준 10 에서만 지원 됩니다. 참조 [구성 x86 에뮬레이터](~/android/get-started/installation/android-emulator/index.md) 자세한 내용은 합니다.

## <a name="summary"></a>요약

이 문서에서는 다양 한 Android 4를 사용 하 여 이제 사용할 수 있는 새로운 기술 설명 합니다. 와 같은 새로운 사용자 인터페이스 기능에서는 검토 합니다 *GridLayout*, *팝업 메뉴*, 및 *스위치* 위젯입니다. 또한 살펴보았습니다 사용 하는 방법은 시스템 UI를 제어 하는 것에 대 한 새로운 지원의 일부를 *TextureView*합니다. 그런 다음 다양 한 새 공유 기술에 설명 했습니다. 에서는 방법을 설명 *Android 보* 를 사용 하는 장치의 정보를 공유 하겠습니다 *NFC*, 새 설명 *달력 API*도 기본 제공 사용하는방법을보여주고및 *ShareActionProvider*합니다.
마지막으로 사용 하는 방법을 검사할 합니다 *ContactsContract* 사용자 프로필 데이터에 액세스 하는 공급자입니다.



## <a name="related-links"></a>관련 링크

- [Ice Cream Sandwich 샘플](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (샘플)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (샘플)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 플랫폼](http://developer.android.com/about/versions/android-4.0.html)
