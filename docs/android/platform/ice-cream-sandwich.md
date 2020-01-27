---
title: 아이스크림 샌드위치 기능
description: 이 문서에서는 Android 4 API (Ice)를 사용 하는 응용 프로그램 개발자에 게 제공 되는 몇 가지 새로운 기능을 설명 합니다. 몇 가지 새로운 사용자 인터페이스 기술에 대해 설명 하 고 Android 4에서 응용 프로그램과 장치 간에 데이터를 공유 하기 위해 제공 하는 다양 한 새로운 기능을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 4fbbe1bec317e66166d5218ef0ed54247aa4f6dd
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724377"
---
# <a name="ice-cream-sandwich-features"></a>아이스크림 샌드위치 기능

_이 문서에서는 Android 4 API (Ice)를 사용 하는 응용 프로그램 개발자에 게 제공 되는 몇 가지 새로운 기능을 설명 합니다. 몇 가지 새로운 사용자 인터페이스 기술에 대해 설명 하 고 Android 4에서 응용 프로그램과 장치 간에 데이터를 공유 하기 위해 제공 하는 다양 한 새로운 기능을 살펴봅니다._

## <a name="overview"></a>概述

Android OS 버전 4.0 (API 수준 14)은 Android 운영 체제의 주요 수정 사항을 나타내며 다음과 같은 몇 가지 중요 한 변경 및 업그레이드를 포함 합니다.

- **업데이트 된 사용자 인터페이스** -여러 가지 새로운 UI 기능을 통해 개발자는 응용 프로그램 사용자 인터페이스를 만들 때 더 강력한 기능과 유연성을 제공 합니다. 이러한 새로운 기능에는 `GridLayout`, `PopupMenu`, `Switch` 위젯 및 `TextureView`가 포함 됩니다.
- **향상 된 하드웨어 가속** – 이제 모든 Android 컨트롤에 대해 GPU에서 2d 렌더링이 수행 됩니다. 또한 하드웨어 가속은 Android 4.0 용으로 개발 된 모든 응용 프로그램에서 기본적으로 설정 되어 있습니다.
- **새 데이터 api** – 이전에 공식적으로 액세스 하지 않은 데이터 (예: 일정 데이터 및 장치 소유자의 사용자 프로필)에 대 한 새로운 액세스 권한이 있습니다.
- **앱 데이터 공유** -응용 프로그램 및 장치 간에 데이터를 공유 하는 것은 이제 `ShareActionProvider`와 같은 기술을 통해 보다 쉽게 사용할 수 있습니다 .이를 통해 작업 모음에서 공유 작업을 쉽게 만들 수 있으며, *NFC (근거리 통신)* 를 위한 *Android 빔* 을 사용 하 여 서로 근접 한 여러 장치 간에 데이터를 공유할 수 있습니다.

이 문서에서는 Android 4.0 API에 대 한 이러한 기능 및 기타 변경 내용을 살펴보고, Xamarin. Android에서 각 기능을 사용 하는 방법을 설명 합니다.

## <a name="user-interface-features"></a>사용자 인터페이스 기능

Android 4에서 제공 되는 다양 한 새로운 사용자 인터페이스 기술은 다음과 같습니다.

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** – 컨트롤의 2d 모눈 레이아웃을 지원 합니다.
- **[스위치 위젯](~/android/user-interface/controls/switch.md)** – 설정 또는 해제 간을 전환할 수 있습니다.
- **[TextureView](~/android/user-interface/controls/texture-view.md)** – 보기 내에서 비디오 및 OpenGL 콘텐츠를 사용 하도록 설정 합니다.
- **[탐색 모음](~/android/user-interface/controls/navigation-bar.md)** – 뒤로, 홈 및 다중 태스킹에 대 한 가상 단추를 포함 합니다.

또한 더 세련 된 모양의 `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`와 같이 더 간단 하 게 작업할 수 있는 다른 UI 요소가 향상 되었습니다.

## <a name="sharing-features"></a>기능 공유

Android 4에는 여러 응용 프로그램 간에 데이터를 공유할 수 있는 몇 가지 새로운 기술이 포함 되어 있습니다. 또한 일정 정보 및 장치 소유자의 사용자 프로필과 같이 이전에는 사용할 수 없었던 다양 한 유형의 데이터에 대 한 액세스를 제공 합니다. 이 섹션에서는 다음을 포함 하 여 이러한 영역을 해결 하는 Android 4에서 제공 하는 다양 한 기능을 살펴봅니다.

- **[Android 보](~/android/platform/android-beam.md)** – NFC를 통해 데이터를 공유할 수 있습니다.
- **[Shareactionprovider](~/android/user-interface/controls/action-bar.md)** – 개발자가 작업 모음에서 공유 작업을 지정할 수 있도록 하는 공급자를 만듭니다.
- **[사용자 프로필](~/android/user-interface/user-profile.md)** – 장치 소유자의 프로필 데이터에 대 한 액세스를 제공 합니다.
- **[CALENDAR API](~/android/user-interface/controls/calendar.md)** -달력 공급자의 일정 데이터에 대 한 액세스를 제공 합니다.

## <a name="x86-emulators"></a>x86 에뮬레이터

ICS는 아직 x86 에뮬레이터를 사용 하 여 개발을 지원 하지 않습니다. x86 에뮬레이터는 Android 2.3.3 API 수준 10 에서만 지원 됩니다. 자세한 내용은 [X86 에뮬레이터 구성](~/android/get-started/installation/android-emulator/index.md) 을 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 현재 Android 4에서 제공 되는 다양 한 새로운 기술에 대해 설명 했습니다. *GridLayout*, *PopupMenu*, *스위치* 위젯 등의 새로운 사용자 인터페이스 기능을 검토 했습니다. 또한 *TextureView*를 사용 하는 방법 뿐만 아니라 시스템 UI를 제어 하기 위한 몇 가지 새로운 지원도 살펴보았습니다. 그런 다음 다양 한 새 공유 기술에 대해 설명 했습니다. *Android 빔* 에서 *NFC*를 사용 하는 장치 간에 정보를 공유 하는 방법을 설명 하 고, 새 *캘린더 API*에 대해 설명 하 고, 기본 제공 *shareactionprovider*를 사용 하는 방법을 살펴보았습니다.
마지막으로, *연락처* 를 사용 하 여 사용자 프로필 데이터에 액세스 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [TextureViewDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
- [아이스크림 샌드위치](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 플랫폼](https://developer.android.com/about/versions/android-4.0.html)
