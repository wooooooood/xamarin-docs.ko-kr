---
title: "조각"
description: "Android 3.0에 조각의 휴대폰 및 태블릿에 있는 많은 다양 한 화면 크기에 대 한 보다 유연한 디자인을 지 원하는 방법을 보여 주는 도입 되었습니다. 이 문서에서는 Xamarin.Android 응용 프로그램을 개발할 조각을 사용 하는 방법 및 조각을 미리 Android 3.0 (API 수준 11) 장치에서 지 원하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0486b9e4371a1bcab02921da42bcb929f00a782f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="fragments"></a>조각

_Android 3.0에 조각의 휴대폰 및 태블릿에 있는 많은 다양 한 화면 크기에 대 한 보다 유연한 디자인을 지 원하는 방법을 보여 주는 도입 되었습니다. 이 문서에서는 Xamarin.Android 응용 프로그램을 개발할 조각을 사용 하는 방법 및 조각을 미리 Android 3.0 (API 수준 11) 장치에서 지 원하는 방법을 설명 합니다._

## <a name="fragments-overview"></a>조각 개요

대부분 태블릿에 화면 크기가 클수록 Android 개발에는 추가적인 복잡성이 추가-작은 화면 반드시 효과가 더 큰 화면 및 그 반대의 경우에 대 한 설계 된 레이아웃 합니다. Android 3.0의 두 가지 새로운 기능이 추가 수가 도입이 있는 복잡성을 줄이기 위해 *조각* 및 *지원 패키지*합니다.

사용자 인터페이스에 대 한 모듈로 조각 생각할 수 있습니다. 사용자 인터페이스를 별도 활동에서 실행 될 수 있는 격리 하 고 재사용 가능한 부분을 나누고 개발자는 데 사용할 수 있습니다. 실행 시 작업 자체가 사용 하는 조각을 결정 합니다.

지원 패키지를 처음으로 호출한 *호환성 라이브러리* 조각 Android 3.0 (API 수준 11) 이전 android 버전을 실행 하는 장치에서 사용 하도록 허용 합니다.

예를 들어 아래 이미지에서는 단일 응용 프로그램에서 다양 한 장치는 폼 팩터 조각을 사용 하는 방법을 보여 줍니다.

[![다이어그램의 조각 송수화기 및 태블릿에 사용 되는 방법](images/00.png)](images/00.png)

*조각 A* 는 목록을 포함 하는 동안 *조각 B* 해당 목록에서 선택한 항목에 대 한 세부 정보를 포함 합니다. 응용 프로그램을 태블릿 컴퓨터에서 실행 조각을 모두 동일한 활동에서 표시할 수 있습니다. 동일한 응용 프로그램을 핸드셋 (된 화면 크기)을 실행 조각 두 개의 별도 활동에 호스트 됩니다. 조각 B와 A 조각은 두 폼 팩터에에서 동일 하지만 호스트 하는 활동 다릅니다.

활동을 조정 하 고 이러한 모든 조각 관리를 위해 Android 라는 새 클래스를 도입 된 *FragmentManager*합니다. 각 활동에는 자체의 인스턴스는 `FragmentManager` 추가, 삭제 및 찾기 호스트 조각입니다. 다음 다이어그램은 조각 및 활동 간의 관계를 보여줍니다.

[![활동, 조각 관리자 및 조각을 간의 관계를 보여 주는 다이어그램](images/01.png)](images/01.png)

몇 가지 관련 복합 컨트롤 또는 미니 활동으로 조각을의 생각할 수 있습니다. 이러한 UI 부분을 사용할 수 있는 다음 하지 독립적으로 개발자가 활동에서 다시 사용할 수 있는 모듈로 번들입니다. 조각에는 계층 구조 보기-마찬가지로 활동-있지만 활동와 달리 화면에 걸쳐 공유할 수 있습니다. 뷰 조각 한다는 점에서 다릅니다 조각은 자신의 기간이; 뷰는 그렇지 않습니다.

활동은 호스트를 하나 이상의 조각이, 자체 조각의 직접 인식 하지 않습니다. 마찬가지로, 조각 호스팅 활동에 있는 다른 조각의 직접 인식 하지 않습니다. 그러나 조각 및 활동 않습니다는 `FragmentManager` 해당 활동에서 합니다. 사용 하 여는 `FragmentManager`, 조각의 특정 인스턴스에 대 한 참조를 얻고 해당 인스턴스에서 메서드를 호출 하 여 활동 또는 조각에 대 한 것 같습니다. 이러한 방식으로 작업 또는 조각을 통신 하 고 다른 조각 상호작용 수 있습니다.

이 가이드에 포함 하 여 조각을 사용 하는 방법에 대 한 포괄적인 적용 범위를 포함 되어 있습니다.

-   **조각 만들기** – 기본 조각 및 구현 해야 하는 주요 메서드를 만드는 방법.
-   **관리 및 트랜잭션을 조각** – 런타임 시 조각을 조작 하는 방법입니다.
-   **Android 지원 패키지** – 이전 버전의 Android에 사용할 조각 수를 허용 하는 라이브러리를 사용 하는 방법입니다.


## <a name="requirements"></a>요구 사항

다음 스크린샷에 표시 된 것 처럼 조각 API 수준 11 (Android 3.0)로 시작 하는 Android SDK에서 제공 됩니다.

[![Android SDK Manager API 수준 선택](images/02.png)](images/02.png)

조각은 이상 Xamarin.Android 4.0에서 사용할 수 있습니다. Xamarin.Android 응용 프로그램을 최소한 대상으로 해야 합니다 (Android 3.0) 11 API 레벨 조각을 사용 하려면 이상. 대상 프레임 워크 아래와 같이 프로젝트 옵션에서 설정할 수 있습니다.

[![프로젝트 옵션에서 대상 프레임 워크 API 수준 설정](images/03.png)](images/03.png)

이전 버전의 Android 지원 패키지 및 Xamarin.Android 4.2를 사용 하 여 Android 또는 더 높은에서 조각을 사용 하는 것이 불가능 합니다. 이 섹션의 문서에 자세히이 작업을 수행 하는 방법을 다룹니다.


## <a name="related-links"></a>관련 링크

- [Honeycomb 갤러리 (샘플)](https://developer.xamarin.com/samples/monodroid/HoneycombGallery)
- [조각](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](http://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV 웹 세미나: 조각 소개](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
