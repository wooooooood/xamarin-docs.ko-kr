---
title: Ice Cream Sandwich 기능
description: 이 문서에서는 애플리케이션 개발자가 Android 4 API - Ice Cream Sandwich에서 사용할 수 있는 몇 가지 새로운 기능을 설명합니다. 몇 가지 새로운 사용자 인터페이스 기술에 대해 설명한 다음, Android 4에서 애플리케이션과 디바이스 간에 데이터를 공유할 수 있도록 제공하는 다양한 새로운 기능을 살펴봅니다.
ms.prod: xamarin
ms.assetid: 78E18A62-C12F-A699-37FA-44B9F6B44273
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 4fbbe1bec317e66166d5218ef0ed54247aa4f6dd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724377"
---
# <a name="ice-cream-sandwich-features"></a>Ice Cream Sandwich 기능

이 문서에서는 애플리케이션 개발자가 Android 4 API - Ice Cream Sandwich에서 사용할 수 있는 몇 가지 새로운 기능을 설명합니다.  몇 가지 새로운 사용자 인터페이스 기술에 대해 설명한 다음, Android 4에서 애플리케이션과 디바이스 간에 데이터를 공유할 수 있도록 제공하는 다양한 새로운 기능을 살펴봅니다.

## <a name="overview"></a>개요

Android OS 버전 4.0(API 레벨 14)은 Android 운영 체제의 대대적인 개정판으로, 다음과 같은 몇 가지 중요한 변경 사항 및 업그레이드를 포함합니다.

- **업데이트된 사용자 인터페이스** – 개발자가 애플리케이션 사용자 인터페이스를 만들 때 더욱 강력하고 유연하게 사용할 수 있는 새로운 UI 기능이 적용되었습니다. 새로운 기능에는 `GridLayout` ,  `PopupMenu` ,  `Switch` 위젯 및 `TextureView`가 있습니다.
- **향상된 하드웨어 가속** – 이제 모든 Android 컨트롤에 대해 2D 렌더링이 GPU에서 수행됩니다. 이에 더해, Android 4.0용으로 개발된 모든 애플리케이션에서 하드웨어 가속이 기본적으로 설정되어 있습니다.
- **새로운 데이터 API** – 캘린더 데이터, 디바이스 소유자의 사용자 프로필과 같이 이전에는 공식적으로 액세스할 수 없던 데이터에 액세스할 수 있습니다.
- **앱 데이터 공유** – 작업 모음에서 작업을 더 쉽게 공유할 수 있도록 지원하는 `ShareActionProvider`, 근처에 있는 디바이스끼리 손쉽게 데이터를 공유할 수 있도록 지원하는 ‘NFC(근거리 통신)’용 *Android Beam*과 같은 기술을 사용하여 그 어느 때보다 쉽게 애플리케이션과 디바이스 간에 데이터를 공유할 수 있습니다. 

이 문서에서는 해당 기능을 비롯해 Android 4.0 API에 적용된 그 밖의 변경 사항을 살펴보고 Xamarin.Android에서 각 기능을 사용하는 방법을 설명합니다.

## <a name="user-interface-features"></a>사용자 인터페이스 기능

Android 4에서는 다음을 비롯한 다양한 새로운 사용자 인터페이스 기술을 사용할 수 있습니다.

- **[GridLayout](~/android/user-interface/layouts/grid-layout.md)** – 컨트롤의 2D 그리드 레이아웃을 지원합니다.
- **[Switch 위젯](~/android/user-interface/controls/switch.md)** – ON과 OFF 간의 토글이 지원됩니다.
- **[TextureView](~/android/user-interface/controls/texture-view.md)** – 하나의 보기에서 동영상과 OpenGL 콘텐츠를 사용할 수 있습니다.
- **[탐색 모음](~/android/user-interface/controls/navigation-bar.md)** – 뒤로, 홈, 멀티태스킹을 위한 가상 단추를 포함합니다.

이 외에도 사용하기 쉬워진 `<a href"/guides/android/user_interface/popup_menus">PopupMenu</a>`와 더욱 세련된 디자인으로 거듭난 탭을 비롯해 여러 UI 요소가 개선되었습니다.

## <a name="sharing-features"></a>공유 기능

Android 4에는 여러 디바이스와 여러 애플리케이션 간에 데이터를 공유할 수 있도록 지원하는 다양한 새로운 기술이 포함되었습니다. 이에 더해 캘린더 정보, 디바이스 소유자의 사용자 프로필과 같이 이전에는 사용할 수 없었던 다양한 형식의 데이터에 대한 액세스도 제공합니다. 이 섹션에서는 다음을 비롯해 데이터 공유 문제를 다루는 Android 4의 다양한 기능을 살펴봅니다.

- **[Android Beam](~/android/platform/android-beam.md)** – NFC를 통한 데이터 공유를 지원합니다.
- **[ShareActionProvider](~/android/user-interface/controls/action-bar.md)** – 개발자가 작업 모음에서 작업의 공유를 지정할 수 있도록 지원하는 공급자를 만듭니다.
- **[사용자 프로필](~/android/user-interface/user-profile.md)** – 디바이스 소유자의 프로필 데이터에 대한 액세스를 제공합니다.
- **[캘린더 API](~/android/user-interface/controls/calendar.md)** – 캘린더 공급자의 캘린더 데이터에 대한 액세스를 제공합니다.

## <a name="x86-emulators"></a>x86 에뮬레이터

ICS에서는 x86 에뮬레이터를 사용한 개발이 아직 지원되지 않습니다. x86 에뮬레이터는 Android 2.3.3 API 레벨 10에서만 지원됩니다. 자세한 내용은 [x86 에뮬레이터 설정](~/android/get-started/installation/android-emulator/index.md)을 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Android 4에 새로 적용된 다양한 새로운 기술을 살펴보았습니다. *GridLayout*, *PopupMenu*, *Switch* 위젯과 같은 새로운 사용자 인터페이스 기능을 살펴보았고, 시스템 UI 제어를 위한 새로운 지원과 *TextureView*를 사용하는 방법도 살펴보았습니다. 몇 가지 새로운 공유 기술도 알아보았습니다. *Android Beam*을 통해 *NFC*를 사용하는 기기 간에 정보를 공유할 수 있다는 사실과 새로운 ‘캘린더 API’를 살펴보고, 기본 제공되는 *ShareActionProvider*를 사용하는 방법을 설명했습니다. 
마지막으로, *ContactsContract* 공급자를 사용하여 사용자 프로필 데이터에 액세스하는 방법을 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [TextureViewDemo(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/textureviewdemo)
- [CalendarDemo(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/calendardemo)
- [탭 레이아웃 자습서](~/android/user-interface/layouts/tab-layout/index.md)
- [Ice Cream Sandwich](https://developer.android.com/about/versions/android-4.0-highlights.html)
- [Android 4.0 플랫폼](https://developer.android.com/about/versions/android-4.0.html)
