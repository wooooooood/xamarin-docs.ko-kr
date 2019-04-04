---
title: 조각
description: Android 3.0 조각을, 휴대폰 및 태블릿에 많은 다양 한 화면 크기 보다 유연한 디자인을 지 원하는 방법을 보여 주는 도입 되었습니다. 이 문서에서는 조각을 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하는 방법 및 조각을 미리 Android 3.0 (API 수준 11) 장치에서 지 원하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 4347ee40b0b72cc2d182ba1ef8f111212dd0ee47
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669728"
---
# <a name="fragments"></a>조각

_Android 3.0 조각을, 휴대폰 및 태블릿에 많은 다양 한 화면 크기 보다 유연한 디자인을 지 원하는 방법을 보여 주는 도입 되었습니다. 이 문서에서는 조각을 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하는 방법 및 조각을 미리 Android 3.0 (API 수준 11) 장치에서 지 원하는 방법을 설명 합니다._

## <a name="fragments-overview"></a>조각 개요

Android 개발에는 추가적인 복잡성 추가할 대부분 태블릿에 더 큰 화면 크기-작은 화면 반드시에 작동 하지 않습니다도 큰 화면, 그 반대로 용으로 디자인 된 레이아웃 합니다. Android 3.0 수가 도입이 복잡성을 줄이기 위해 두 가지 새로운 기능을 추가 *조각* 하 고 *지원 패키지*합니다.

조각은 사용자 인터페이스에 대 한 모듈로 생각할 수 있습니다. 사용자 인터페이스를 별도 작업에서 실행할 수 있는 격리 된, 재사용 가능한 부분으로 나누고 개발자를 수 있습니다. 런타임 시 작업 자체가 있는 조각을 사용 하 여 결정 합니다.

지원 패키지를 처음으로 호출한 *호환성 라이브러리* 조각을 Android 3.0 (API 수준 11) 이전 android 버전을 실행 하는 장치에서 사용할 수 있습니다.

예를 들어 아래 이미지는 단일 응용 프로그램을 다양 한 장치 폼 팩터 간에 조각을 사용 하는 방법을 보여 줍니다.

[![송수화기 및 태블릿에 조각을 사용 하는 방법의 다이어그램](images/00.png)](images/00.png#lightbox)

*조각* 목록을 포함 하는 동안 *조각 B* 해당 목록에서 선택한 항목에 대 한 세부 정보를 포함 합니다. 응용 프로그램을 태블릿에서 실행 조각을 모두 동일한 활동에서 표시할 수 있습니다. 동일한 응용 프로그램 (사용 하 여 더 작은 화면 크기) 송수화기에서 실행 되 면 조각 별개의 두 작업에서 호스트 됩니다. 조각 B와 조각 A는 두 폼 팩터에 동일 하지만 호스트 하는 활동 다릅니다.

활동을 조정 하 고 이러한 모든 조각 관리를 위해 Android 라는 새 클래스를 도입 합니다 *FragmentManager*합니다. 각 작업에는 고유한 인스턴스의 `FragmentManager` 추가, 삭제 및 찾기 호스트 조각입니다. 다음 다이어그램은 조각 및 활동 간의 관계를 보여 줍니다.

[![활동, 조각 관리자 및 조각 간의 관계를 보여 주는 다이어그램](images/01.png)](images/01.png#lightbox)

몇 가지 관련 복합 컨트롤 또는 미니 활동으로 조각을의 생각할 수 있습니다. 이러한 번들 UI 부분을 사용할 수 있는 다음 하지 독립적으로 개발자가 활동에서 다시 사용할 수 있는 모듈을 등록 합니다. 조각 뷰 계층 구조를 없는-마찬가지로 활동-있지만 활동 달리 화면에 걸쳐 공유할 수 있습니다. 뷰 조각 점에서 다릅니다 조각에는 자체 기간이; 뷰 변환 되지 않습니다.

호스트를 하나 이상의 조각이 활동을 사용 하는 동안 자체 조각의 직접적으로 알지 아닙니다. 마찬가지로, 조각 직접 호스팅 작업에서 다른 조각을 확인 하지 않습니다. 하지만 조각 및 활동은 인식는 `FragmentManager` 해당 활동에서 합니다. 사용 하 여는 `FragmentManager`, 활동 또는 조각 조각인의 특정 인스턴스에 대 한 참조를 가져오고 해당 인스턴스에서 다음 메서드를 호출 하는 것이 가능 합니다. 이러한 방식으로 작업 또는 조각 통신 하 고 다른 조각을 사용 하 여 상호 작용할 수 있습니다.

이 가이드에 포함 하 여 조각을 사용 하는 방법에 대 한 포괄적인을 포함 되어 있습니다.

-   **조각 만들기** – 기본 조각 및 구현 해야 하는 주요 메서드를 만드는 방법.
-   **조각 관리와 트랜잭션** – 런타임 시 조각을 조작 하는 방법입니다.
-   **Android 지원 패키지** -이전 버전의 Android에서 사용할 조각 수 있도록 하는 라이브러리를 사용 하는 방법을 알아봅니다.


## <a name="requirements"></a>요구 사항

다음 스크린샷에 표시 된 것과 같이 조각 API 레벨 11 (Android 3.0)부터 Android SDK에서 사용할 수 있습니다.

[![Android SDK Manager의 API 수준 선택](images/02.png)](images/02.png#lightbox)

조각은 이상 Xamarin.Android 4.0에서 사용할 수 있습니다. Xamarin.Android 응용 프로그램을 최소한 대상으로 해야 합니다 (Android 3.0) 11 API 레벨 조각을 사용 하려면 이상. 속성을 아래와 같이 프로젝트의 대상 프레임 워크를 설정할 수 있습니다.

[![프로젝트 옵션에서 대상 프레임 워크 API 수준 설정](images/03-sml.png)](images/03.png#lightbox)

이전 버전의 Android 지원 패키지 및 Xamarin.Android 4.2를 사용 하 여 Android 이상 조각을 사용 하는 것이 가능 합니다. 이 작업을 수행 하는 방법은이 섹션의 문서에서 자세히 다룹니다.


## <a name="related-links"></a>관련 링크

- [Honeycomb 갤러리 (샘플)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [조각](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV 웹 세미나: 조각 소개](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
