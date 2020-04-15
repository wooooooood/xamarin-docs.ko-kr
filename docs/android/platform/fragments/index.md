---
title: 조각
description: Android 3.0은 휴대폰과 태블릿에 탑재된 다양한 화면 크기에 더 유연한 디자인을 지원하는 방법을 보여주는 조각을 도입했습니다. 이 문서에서는 조각을 사용하여 Xamarin.Android 애플리케이션을 개발하는 방법과 Android 3.0(API 수준 11) 이전의 디바이스에서 조각을 지원하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 1AFB4242-A337-F8E0-83D9-B8D850D7F384
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 5d243429fe4f61768568a634b205055c1ad94297
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020260"
---
# <a name="fragments"></a>조각

_Android 3.0은 휴대폰과 태블릿에 탑재된 다양한 화면 크기에 더 유연한 디자인을 지원하는 방법을 보여주는 조각을 도입했습니다. 이 문서에서는 조각을 사용하여 Xamarin.Android 애플리케이션을 개발하는 방법과 Android 3.0(API 수준 11) 이전의 디바이스에서 조각을 지원하는 방법을 설명합니다._

## <a name="fragments-overview"></a>조각 개요

대부분의 태블릿에서 볼 수 있는 대형 화면은 Android 개발에 또 다른 복잡성을 더했습니다. 소형 화면에 맞게 디자인된 레이아웃은 대형 화면에서 잘 작동하지 않을 수 있고 그 반대의 경우도 마찬가지입니다. 이로 인해 발생하는 복잡한 문제를 줄이기 위해 Android 3.0에는 *조각* 및 *지원 패키지*라는 두 가지 새로운 기능이 추가되었습니다.

조각은 사용자 인터페이스 모듈로 간주할 수 있습니다. 개발자는 이를 통해 개별 작업에서 실행할 수 있는 분리되고 재사용 가능한 부분으로 사용자 인터페이스를 분할할 수 있습니다. 런타임에 작업 자체에서 사용할 조각을 결정합니다.

지원 패키지는 원래 *호환성 라이브러리*였으며, Android 3.0(API 수준 11) 이전 버전의 Android를 실행하는 디바이스에서 조각을 사용할 수 있도록 지원했습니다.

예를 들어 아래 이미지는 단일 애플리케이션이 다양한 디바이스 폼 팩터에서 조각을 사용하는 방법을 보여줍니다.

[![태블릿 및 송수화기에서 조각을 사용하는 방법에 대한 다이어그램](images/00.png)](images/00.png#lightbox)

*조각 A*에는 목록이 포함되어 있고, *조각 B*에는 해당 목록에서 선택된 항목에 대한 세부 정보가 포함되어 있습니다. 애플리케이션이 태블릿에서 실행되는 경우 동일한 작업에 두 조각이 모두 표시될 수 있습니다. 동일한 애플리케이션이 송수화기(소형 화면)에서 실행되는 경우 조각은 두 개의 개별 작업에서 호스팅됩니다. 조각 A와 조각 B는 두 폼 팩터에서 동일하지만 이를 호스팅하는 작업은 서로 다릅니다.

작업이 이러한 모든 조각을 조정하고 관리하는 데 도움이 되도록 Android는 *FragmentManager*라는 새 클래스를 도입했습니다. 각 작업에는 호스팅되는 조각을 추가, 삭제하고 찾기 위한 자체 `FragmentManager` 인스턴스가 있습니다. 다음 다이어그램은 조각과 작업 간의 관계를 보여줍니다.

[![작업, 조각 관리자 및 조각 간의 관계를 보여주는 다이어그램](images/01.png)](images/01.png#lightbox)

일부 측면에서 조각은 복합 컨트롤이나 미니 작업으로 간주할 수 있습니다. UI 조각을 재사용 가능한 모듈에 번들로 묶어 작업의 개발자가 독립적으로 사용하도록 할 수 있습니다. 조각은 작업처럼 뷰 계층 구조를 포함하지만 활동과 달리 화면 간에 공유될 수 있습니다. 뷰가 조각과 다른 점은 조각은 자체 수명 주기가 있지만 뷰는 그렇지 않다는 점입니다.

작업은 하나 이상의 조각에 대한 호스트이지만 직접 조각 자체를 인식하지는 못합니다. 마찬가지로, 조각은 호스팅 작업의 다른 조각을 직접 인식하지 못합니다. 그러나 조각과 작업은 작업의 `FragmentManager`를 인식합니다. `FragmentManager`를 사용하면 작업 또는 조각이 조각의 특정 인스턴스에 대한 참조를 가져온 다음, 해당 인스턴스에서 메서드를 호출할 수 있습니다. 작업 또는 조각은 이러한 방식으로 다른 조각과 통신하고 상호 작용할 수 있습니다.

이 가이드에는 다음을 포함한 조각 사용 방법에 대한 포괄적인 내용이 포함되어 있습니다.

- **조각 만들기** – 구현해야 하는 기본 조각과 주요 메서드를 만드는 방법입니다.
- **조각 관리 및 트랜잭션** – 런타임에 조각을 조작하는 방법입니다.
- **Android 지원 패키지** – 이전 버전의 Android에서 조각을 사용할 수 있도록 지원하는 라이브러리를 사용하는 방법입니다.

## <a name="requirements"></a>요구 사항

다음 스크린샷에 표시된 것처럼 API 수준 11(Android 3.0)부터 Android SDK에서 조각을 사용할 수 있습니다.

[![Android SDK Manager에서 API 수준 선택](images/02.png)](images/02.png#lightbox)

조각은 Xamarin.Android 4.0 이상에서 사용할 수 있습니다. 조각을 사용하려면 Xamarin Android 애플리케이션이 API 수준 11(Android 3.0) 이상을 대상으로 해야 합니다. 다음과 같이 프로젝트 속성에서 대상 프레임워크를 설정할 수 있습니다.

[![프로젝트 옵션에서 대상 프레임워크 API 수준 설정](images/03-sml.png)](images/03.png#lightbox)

Android 지원 패키지 및 Xamarin.Android 4.2 이상을 사용하여 이전 버전의 Android에서 조각을 사용할 수 있습니다. 이 작업을 수행하는 방법은 이 섹션의 문서에 자세히 설명되어 있습니다.

## <a name="related-links"></a>관련 링크

- [Honeycomb Gallery(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/honeycombgallery)
- [조각](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](https://developer.android.com/sdk/compatibility-library.html)
