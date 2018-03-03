---
title: "아이스크림 샌드위치 기능"
description: "이 문서에서는 Android 4 api-아이스크림 샌드위치 응용 프로그램 개발자가 사용할 수 있는 새로운 기능인을 설명 합니다. 여러 가지 새로운 사용자 인터페이스 기술에 설명 하 고 다양 한 Android 4는 장치 및 응용 프로그램 간의 데이터를 공유 하는 새로운 기능을 검토 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 9726ec83cd38dd2f237152f0f8e7373f509a3e01
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ice-cream-sandwich-features"></a>아이스크림 샌드위치 기능

_이 문서에서는 Android 4 api-아이스크림 샌드위치 응용 프로그램 개발자가 사용할 수 있는 새로운 기능인을 설명 합니다. 여러 가지 새로운 사용자 인터페이스 기술에 설명 하 고 다양 한 Android 4는 장치 및 응용 프로그램 간의 데이터를 공유 하는 새로운 기능을 검토 합니다._

## <a name="overview"></a>개요

주요 수정 Android 운영 체제의 나타내며 다양 한 중요 한 변경 내용 및 업그레이드를 포함 하 여 포함 하는 android OS 버전 4.0 (API 수준 14):

-   **사용자 인터페이스 업데이트** 몇 가지 새로운 UI 기능 개발자 더 전원 하 고 응용 프로그램 사용자를 만들 때 유연성 인터페이스 – 합니다. 이러한 새로운 기능: `GridLayout` , `PopupMenu` , `Switch` 위젯, 및 `TextureView` 합니다. 
-   **향상 된 하드웨어 가속** – 2D 렌더링 이제 Android 모든 컨트롤에 대 한 GPU에서 수행 합니다. 또한, 하드웨어 가속이 on으로 Android 4.0 용으로 개발 하는 모든 응용 프로그램에서 기본적으로. 
-   **새로운 데이터 Api** – 이전에 공식적으로 액세스할 수 없으므로, 일정 데이터 및 장치 소유자의 사용자 프로필과 같은 데이터에 새 액세스할 수 있습니다. 
-   **응용 프로그램 데이터 공유** – 공유 응용 프로그램 및 장치 간에 데이터는 이제 기술을 통해 이전 보다 쉽게와 같은 `ShareActionProvider` 를 쉽게 공유 작업을 작업 모음에서 만들려는 및 *Android 빔* 에 대 한 *통신 NFC (근거리)* ,이 통해 서로 가까이 있는 장치 간에 데이터를 공유할 간단 합니다. 


이 문서에서는 이러한 기능 및 Android 4.0 API에 적용 된 기타 변경 내용 탐색 하 여 및 Xamarin.Android를 각 기능을 사용 하는 방법을 설명 합니다.

## <a name="user-interface-features"></a>사용자 인터페이스 기능

다양 한 새로운 사용자 인터페이스 기술 포함 하 여 Android 4를 사용할 수 있습니다.

-   **[GridLayout](~/android/user-interface/layouts/grid-layout.md)**  – 컨트롤의 2D 격자 레이아웃을 지원 합니다. 
-   **[위젯 전환](~/android/user-interface/controls/switch.md)**  – 간을 전환할 수 있도록 ON 또는 OFF입니다. 
-   **[TextureView](~/android/user-interface/controls/texture-view.md)**  – 비디오 및 OpenGL 콘텐츠 보기 내에서 사용 하도록 설정 합니다. 
-   **[탐색 모음](~/android/user-interface/controls/navigation-bar.md)**  – 역방향으로 가정 하 고 다중 작업에 대 한 가상 단추가 포함 되어 있습니다. 


또한 다른 UI 요소 향상 되어와 같은 `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`는 이제 보다 쉽게 작업할 수 이며 탭에서 더 세련 된 모양을 갖도록입니다.

## <a name="sharing-features"></a>공유 기능

Android 4에는 장치 및 응용 프로그램에서 데이터를 공유할 수 있는 여러 가지 새로운 기술을 포함 되어 있습니다. 또한 다양 한 유형의 일정 정보 및 장치 소유자의 사용자 프로필과 같은 이전에 사용할 수 없던 데이터에 대 한 액세스를 제공 합니다. 살펴봅니다이 섹션에서는 다양 한 기능에서 제공 Android 4 해당 주소 포함 하 여 이러한 영역:

-  **[Android 빔](~/android/platform/android-beam.md)**  – 수 있도록 데이터 NFC를 통해 공유 합니다.
-   **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)**  -개발자가 작업 모음에서 공유 작업을 지정할 수 있는 공급자를 만듭니다. 
-   **[사용자 프로필](~/android/user-interface/user-profile.md)**  – 장치 소유자의 프로필 데이터에 대 한 액세스를 제공 합니다. 
-   **[API 달력](~/android/user-interface/controls/calendar.md)**  – 달력 공급자에서 액세스 일정 데이터를 제공 합니다. 

## <a name="x86-emulators"></a>x86 에뮬레이터

ICS 아직 x86 사용한 개발을 지원 하지 않습니다 에뮬레이터입니다. x86 에뮬레이터는 Android 2.3.3 API 수준 10 하 에서만 지원 됩니다. 참조 [구성 x86 에뮬레이터](~/android/get-started/installation/android-emulator/index.md) 자세한 정보에 대 한 합니다.

## <a name="summary"></a>요약

이 문서에는 다양 한 이제 Android 4에 사용할 수 있는 새로운 기술 다룹니다. 와 같은 새로운 사용자 인터페이스 기능 평가한 결과 *GridLayout*, *팝업 메뉴*, 및 *스위치* 위젯입니다. 또한 살펴보았습니다 사용 하는 방법은 시스템 UI, 제어에 대 한 새로운 지원의 일부는 *TextureView*합니다. 그런 다음 다양 한 새 공유 기술에 설명 했습니다. 에서는 어떻게 적용 *Android 빔* 사용 하는 장치에서 정보를 공유할 보겠습니다 *NFC*, 새 설명 *일정 API*도 에기본제공사용하는방법을보여주고및 *ShareActionProvider*합니다.
마지막으로 사용 하는 방법을 검사 했습니다는 *ContactsContract* 사용자 프로필 데이터를 액세스 하는 공급자입니다.



## <a name="related-links"></a>관련 링크

- [아이스크림 샌드위치 샘플](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/)
- [TextureViewDemo (샘플)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [CalendarDemo (샘플)](https://developer.xamarin.com/samples/monodroid/CalendarDemo/)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
- [아이스크림 샌드위치](http://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 플랫폼](http://developer.android.com/about/versions/android-4.0.html)
