---
title: 조각
description: Android 3.0에는 휴대폰 및 태블릿에서 찾을 수 있는 다양 한 화면 크기에 대해 더 유연한 디자인을 지 원하는 방법을 보여 주는 조각이 도입 되었습니다. 이 문서에서는 조각을 사용 하 여 Xamarin Android 응용 프로그램을 개발 하는 방법과 Android 이전 3.0 (API 수준 11) 장치에서 조각을 지 원하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: b04ecf0685e78b73346ea5af815ed46f98b5da0f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524349"
---
# <a name="fragments"></a>조각

_Android 3.0에는 휴대폰 및 태블릿에서 찾을 수 있는 다양 한 화면 크기에 대해 더 유연한 디자인을 지 원하는 방법을 보여 주는 조각이 도입 되었습니다. 이 문서에서는 조각을 사용 하 여 Xamarin Android 응용 프로그램을 개발 하는 방법과 Android 이전 3.0 (API 수준 11) 장치에서 조각을 지 원하는 방법을 설명 합니다._

## <a name="fragments-overview"></a>조각 개요

대부분의 태블릿에 있는 큰 화면 크기는 Android 개발에 더 복잡 한 계층을 추가 했습니다. 작은 화면에 맞게 디자인 된 레이아웃은 큰 화면에서 잘 작동 하지 않을 수도 있고 그 반대의 경우도 마찬가지입니다. 이에 대 한 복잡 한 수를 줄이기 위해 Android 3.0에는 두 가지 새로운 기능, *조각* 및 *지원 패키지가*추가 되었습니다.

조각은 사용자 인터페이스 모듈로 간주할 수 있습니다. 개발자는 개별 작업에서 실행할 수 있는 분리 된 재사용 가능한 부분으로 사용자 인터페이스를 분할할 수 있습니다. 런타임에 작업 자체에서 사용할 조각을 결정 합니다.

지원 패키지는 원래 *호환성 라이브러리* 및 android 3.0 이전 버전의 android를 실행 하는 장치에서 사용할 수 있는 조각 (API 수준 11) 이라고 했습니다.

예를 들어 아래 이미지는 단일 응용 프로그램이 다양 한 장치 폼 팩터에서 조각을 사용 하는 방법을 보여 줍니다.

[![태블릿 및 송수화기에서 조각을 사용 하는 방법에 대 한 다이어그램](images/00.png)](images/00.png#lightbox)

*조각 a* 에는 목록이 포함 되 고, *조각 B* 는 해당 목록에서 선택한 항목에 대 한 세부 정보를 포함 합니다. 응용 프로그램을 태블릿에서 실행 하는 경우 동일한 활동에 두 조각이 모두 표시 될 수 있습니다. 동일한 응용 프로그램이 송수화기에서 실행 되는 경우 (더 작은 화면 크기) 조각은 두 개의 개별 작업에서 호스팅됩니다. 조각 A와 조각 B는 두 폼 팩터에서 동일 하지만이를 호스트 하는 활동은 서로 다릅니다.

활동을 조정 하 고 이러한 모든 조각을 관리 하기 위해 Android에서는 *FragmentManager*이라는 새 클래스를 도입 했습니다. 각 활동에는 `FragmentManager` 호스트 된 조각을 추가, 삭제 및 찾기 위한 자체 인스턴스가 있습니다. 다음 다이어그램에서는 조각과 활동 간의 관계를 보여 줍니다.

[![활동, 조각 관리자 및 조각 간의 관계를 보여 주는 다이어그램](images/01.png)](images/01.png#lightbox)

일부 측면에서 조각은 복합 컨트롤이 나 미니 활동으로 간주할 수 있습니다. UI를 다시 사용할 수 있는 모듈에 번들로 묶어 활동의 개발자가 독립적으로 사용할 수 있습니다. 조각은 활동 처럼 뷰 계층 구조를 포함 하지만 활동과 달리 화면에서 공유할 수 있습니다. 조각은 자체 수명 주기를 포함 한다는 점에서 조각과 다릅니다. 뷰는 그렇지 않습니다.

활동은 하나 이상의 조각에 대 한 호스트 이지만 직접 조각을 인식 하지 못합니다. 마찬가지로, 조각은 호스팅 활동의 다른 조각을 직접 인식 하지 못합니다. 그러나 조각 및 활동은 해당 활동 `FragmentManager` 에서을 인식 합니다. 를 사용 `FragmentManager`하 여 작업 또는 조각이 조각의 특정 인스턴스에 대 한 참조를 가져온 다음 해당 인스턴스에서 메서드를 호출할 수 있습니다. 이러한 방식으로 활동 또는 조각은 다른 조각과 통신 하 고 상호 작용할 수 있습니다.

이 가이드에는 다음을 포함 하 여 조각 사용 방법에 대 한 포괄적인 검사가 포함 되어 있습니다.

- **조각 만들기** – 구현 해야 하는 기본 조각과 키 메서드를 만드는 방법입니다.
- **조각 관리 및 트랜잭션** – 런타임에 조각을 조작 하는 방법입니다.
- **Android 지원 패키지** – 이전 버전의 Android에서 조각을 사용할 수 있도록 하는 라이브러리를 사용 하는 방법입니다.


## <a name="requirements"></a>요구 사항

다음 스크린샷에 표시 된 것 처럼 API 수준 11 (Android 3.0)부터 시작 하는 Android SDK에서 조각을 사용할 수 있습니다.

[![Android SDK Manager에서 API 수준 선택](images/02.png)](images/02.png#lightbox)

조각은 Xamarin Android 4.0 이상에서 사용할 수 있습니다. Xamarin Android 응용 프로그램은 조각을 사용 하기 위해 최소한 API 수준 11 (Android 3.0) 이상을 대상으로 해야 합니다. 다음과 같이 프로젝트 속성에서 대상 프레임 워크를 설정할 수 있습니다.

[![프로젝트 옵션에서 대상 프레임 워크 API 수준 설정](images/03-sml.png)](images/03.png#lightbox)

Android 지원 패키지 및 Xamarin Android 4.2 이상을 사용 하 여 이전 버전의 Android에서 조각을 사용할 수 있습니다. 이 작업을 수행 하는 방법은이 섹션의 문서에 자세히 설명 되어 있습니다.


## <a name="related-links"></a>관련 링크

- [Honeycomb 갤러리 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/honeycombgallery)
- [조각](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
- [MOTODEV 웹 세미나: 조각 소개](http://motodev.adobeconnect.com/p9h1aqk3ttn/)
