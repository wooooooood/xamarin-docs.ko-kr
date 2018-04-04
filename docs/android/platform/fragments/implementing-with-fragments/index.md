---
title: 조각을 사용 하 여 구현
description: Android 3.0에 조각의 도입 되었습니다. 조각은 다양한 크기의 화면에서 실행될 수 있는 응용 프로그램 쓰기의 복잡성을 해결하는 데 사용되는 자체 포함 모듈식 구성 요소입니다. 이 문서 조각을 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하는 방법과 조각 3.0 미리 Android 장치에서 지 원하는 방법에 안내 합니다.
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 81f1f992de450ee62c4c1d2e80da858b024be594
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="implementing-with-fragments"></a>조각을 사용 하 여 구현

_Android 3.0에 조각의 도입 되었습니다. 조각은 다양한 크기의 화면에서 실행될 수 있는 응용 프로그램 쓰기의 복잡성을 해결하는 데 사용되는 자체 포함 모듈식 구성 요소입니다. 이 문서 조각을 사용 하 여 Xamarin.Android 응용 프로그램을 개발 하는 방법과 조각 3.0 미리 Android 장치에서 지 원하는 방법에 안내 합니다._


## <a name="overview"></a>개요

이 섹션의 역할 및 각 선택한 play에서 따옴표의 목록을 표시 하는 응용 프로그램을 만드는 방법을 살펴봅니다. 앱 म 우리의 UI 구성 요소를 한 곳에서 정의할 수 있지만 다음 다양 한 폼에서 사용할 수 있도록 조각이 사용 됩니다. 예를 들어 다음 스크린 샷과 10"태블릿 뿐 아니라 휴대폰 실행 응용 프로그램을 보여 줍니다.

[![태블릿 및 전화에서 실행 되는 예제 앱의 스크린 샷](images/intro-screenshot-sml.png)](images/intro-screenshot.png#lightbox)

이 섹션에서는 다음 항목을 설명 합니다.

- **조각을 만드는** &ndash; 의 재생 목록을 표시 하는 조각 및 각 play에서 견적을 표시 하려면 다른 조각을 만드는 방법을 보여 줍니다.

- **다양 한 화면 크기를 지 원하는** &ndash; 표시 더 큰 크기를 활용 하도록 응용 프로그램 레이아웃 하는 방법.

- **Android 지원 패키지를 사용 하 여** &ndash; 하므로 이전 버전의 Android에서 실행 하는 응용 프로그램에서 활동을 몇 가지 사소한 변경 하 한 다음 Android 지원 패키지를 구현 합니다.


## <a name="requirements"></a>요구 사항

이 연습에서는 Xamarin.Android 4.0 이상이 필요 합니다. 또한 됩니다 Android 지원 패키지를 설치 하는 데 필요한 조각 설명서에 설명 된 대로 합니다.


## <a name="introduction"></a>소개

이 섹션에서는 빌드합니다 예제에서는 활동 목록을 로드, 사용자 선택에 응답 또는 선택한 재생에 대 한 따옴표를 표시 하기 위한 논리를 포함 하지 않습니다. 이 논리는 개별 조각에 존재합니다.
자체 조각에서이 논리를 배치 하 여 각 활동에 대 한 다른 논리를 작성할 필요 없이 하나의 작업 또는 여러 작업을 작은 화면 큰 화면을 지원 하도록 응용 프로그램의 워크플로를 분석할 수 있습니다. 태블릿에서는 두 조각 한 작업에 있게 됩니다. 휴대폰의 조각 서로 다른 동작에 호스트 됩니다.

이 응용 프로그램에는 다음과 같은 부분이 포함 됩니다.

 **MainActivity** – 화면 크기에 따라 조각 중 하나 또는 모두 표시 됩니다. 활동의 시작입니다.

 **TitlesFragment** – 사용자가 선택할 수 있는 재생 목록이 표시 됩니다.

 **DetailsFragment** – 선택한 play에서 견적을 표시 합니다.

 **DetailsActivity** –를 호스트 하 고는 DetailsFragment 표시 됩니다.
이 작업은 휴대폰 등의 작은 화면을 사용 하 여 장치에서 사용 됩니다.



## <a name="related-links"></a>관련 링크

- [FragmentsWalkthrough (샘플)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [디자이너 개요](~/android/user-interface/android-designer/index.md)
- [Xamarin.Android 샘플: Honeycomb 갤러리](https://developer.xamarin.com/samples/HoneycombGallery/)
- [조각 구현](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [지원 패키지](http://developer.android.com/sdk/compatibility-library.html)
