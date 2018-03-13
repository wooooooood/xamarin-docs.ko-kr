---
title: "더 효율적인 Xamarin Android v4 지원 / v13 NuGet 패키지"
ms.topic: article
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: ab8d90ebd938a8417e5a48155347e03191ef9ca4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>더 효율적인 Xamarin Android v4 지원 / v13 NuGet 패키지

## <a name="about-the-android-support-libraries"></a>에 대 한 Android 지원 라이브러리

Google에서 새로운 기능을 이전 버전의 Android 사용할 수 있도록 지원 라이브러리를 만들었습니다. 지원 라이브러리 호환 되는 가장 낮은 Android API 수준 이름에 버전 번호를 제공은 일반적으로 (예:: v4 지원 API 수준 4 이상에 사용할 수 있습니다. 자세한 내용은이 [스택 오버플로 토론](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

두 지원 라이브러리: `Support-v4` 및 `Support-v13` 사용할 수 없습니다 함께 동일한 앱에서 즉는 함께 사용할 수 없습니다. 때문에 이것이 `Support-v13` 실제로 형식 및의 구현을 모두 포함 되어 `Support-v4`합니다. 시도 하 고 모두 동일한 프로젝트에서 참조 하는 경우에 중복 되는 형식 오류가 발생 합니다.

## <a name="problems-with-referencing"></a>참조를 사용한 문제

이후 `Support-v4` 많이 타사 라이브러리의 많은 이제 의존 되었습니다. 대신, v13 지원에 따라 달라 지도록 선택 있을 수 있지만 것이 더에 따라 달라 지도록 공통 _v4_ API 수준 4까지 지원 합니다. 이러한 타사 라이브러리를 사용 하 여 모든 앱을 제공 하는 이후입니다.

Xamarin 3rd 파티 라이브러리 참조 하는 경우는 `Xamarin.Android.Support.v4.dll` 바인딩할 `Support-v4`,이 라이브러리를 사용 하는 앱 참조 해야 `Xamarin.Android.Support.v4.dll`합니다. 이 문제가 동일한 응용 프로그램에서는 또한에서 기능 중 일부를 사용 하려고 하는 경우는 `Xamarin.Android.Support.v13.dll` 바인딩할 `Support-v13`합니다. 두 바인딩은 모두 참조 하는 경우에 중복 되는 형식 오류가 발생 합니다.

## <a name="type-forwarded-v4-binding-assembly"></a>V4 형식이 전달 바인딩 어셈블리

이 문제를 해결 하려면 특별 한 준비 되어 `Xamarin.Android.Support.v4.dll` 단순히 있지만 구현이 없는 있는 어셈블리 `[assembly: TypeForwardedTo (..)]` 모두 전달 하는 특성은 `Support-v4` 형식 내에서 구현에는 `Xamarin.Android.Support.v13.dll` 어셈블리입니다.

즉, 개발자는이 참조할 수 있습니다 _형식이 전달_ 어셈블리에 대 한 참조를 만족 하는 해당 응용 프로그램에서 `Xamarin.Android.Support.v4.dll` 모든 타사 라이브러리를 허용 하면서 여 `Xamarin.Android.Support.v13.dll` 응용 프로그램에서 사용할 수 있습니다.

## <a name="nuget-assistance"></a>NuGet 지원

올바른 어셈블리를 선택할 수 있도록 NuGet을 사용 하는 개발자 올바른 참조가 필요한에서는 추가 수동으로 수 (중 하나는 보통 _v4_ 바인딩이나 형식이 전달 _v4_ 어셈블리) 때 NuGet 패키지가 설치 됩니다.

따라서는 `Xamarin.Android.Support.v4` NuGet 패키지에는 이제 다음과 같은 논리 포함.

응용 프로그램 API 수준 13 (인형 3.2) 대상으로 하는 경우 이상:

*   `Xamarin.Android.Support.v13` NuGet는 자동으로 종속성으로 추가
*   _형식이 전달_ `Xamarin.Android.Support.v4.dll` 프로젝트에서 참조할 수

일반 응용 프로그램 API 수준 13 보다 낮은 아무 것도 대상으로 하는 경우 받아볼 `Xamarin.Android.Support.v4.dll` 프로젝트에서 참조 하는 바인딩.

## <a name="do-i-have-to-use-support-v13"></a>지원 v13 사용 해야 합니까?

응용 프로그램 API 레벨 13 개 이상의 대상으로 사용 하려는 경우는 `Xamarin Android Support-v4` NuGet 패키지는 다음 `Xamarin Android Support v13` NuGet 패키지는 필수 종속성.

느끼는 (17 기술 자료별 다른 두 개의.jar 파일)는 앱 크기가 매우 사소한 증가 충분 한 가치가 호환성 및 걱정거리 발생 합니다.

사용에 대 한 adamant 있다면 `Support-v4` API 수준 13을 대상으로 하는 응용 프로그램 또는 이상에 항상 직접 다운로드할 수 있습니다는 `.nupkg`, 압축을 푼 및 어셈블리를 참조 합니다.
