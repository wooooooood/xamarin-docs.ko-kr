---
title: 더 스마트 한 Xamarin Android 지원 v4 / v13 NuGet 패키지
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 43627884c2f8bc4d9e5b5faa2c3af08f74487b65
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114647"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>더 스마트 한 Xamarin Android 지원 v4 / v13 NuGet 패키지

## <a name="about-the-android-support-libraries"></a>Android 지원 라이브러리에 대 한

Google에서 새로운 기능을 이전 버전의 Android 사용할 수 있도록 지원 라이브러리를 만들었습니다. 지원 라이브러리 최하위 수준인 Android API와 호환 되는 이름에 버전 번호를 지정은 일반적으로 (예: API 수준 4 이상에 지원 v4를 사용할 수 있습니다. 이 자세한 [스택 오버플로 토론](http://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)). 

지원 라이브러리의 두: `Support-v4` 고 `Support-v13` 사용할 수 없습니다 함께 동일한 앱에서,는 함께 사용할 수 없습니다. 왜냐하면 `Support-v13` 실제로 형식 및 구현의 모든 포함 `Support-v4`합니다. 시도 하 고 모두 동일한 프로젝트에서 참조 하는 경우 중복 된 형식 오류가 발생 합니다.

## <a name="problems-with-referencing"></a>참조 하는 문제

이후 `Support-v4` 인기가 많은 타사 라이브러리에 이제 종속 되었습니다. 대신 지원 v13에 따라 달라 지도록 했습니다 수 이지만 더 종속에 공통적으로 적용 _v4_ 하므로 이러한 타사 라이브러리를 사용 하 여 모든 앱 4까지 API 수준을 지 원하는 옵션을 제공 합니다.

Xamarin 3rd 파티 라이브러리를 참조 하는 경우는 `Xamarin.Android.Support.v4.dll` 바인딩할 `Support-v4`,이 라이브러리를 사용 하는 모든 앱 참조 해야 `Xamarin.Android.Support.v4.dll`합니다. 동일한 앱도의 기능 중 일부를 사용 하려는 경우 이것이 문제가 합니다 `Xamarin.Android.Support.v13.dll` 바인딩할 `Support-v13`합니다. 두 바인딩은 모두를 참조 하는 경우 중복 된 형식 오류가 발생 합니다.

## <a name="type-forwarded-v4-binding-assembly"></a>V4 형식이 전달 바인딩 어셈블리

특별 한을이 문제를 해결 하기 위해 만들었습니다 `Xamarin.Android.Support.v4.dll` 하지만 단순히 구현이 있는 어셈블리 `[assembly: TypeForwardedTo (..)]` 모두 전달 하는 특성을 `Support-v4` 내에서 구현 형식은 `Xamarin.Android.Support.v13.dll` 어셈블리.

즉, 개발자는이 참조할 수 있습니다 _형식이 전달_ 어셈블리에 대 한 참조를 만족 하는 해당 앱에 `Xamarin.Android.Support.v4.dll` 모든 타사 라이브러리를 동시에 의해 `Xamarin.Android.Support.v13.dll` 앱에서 사용할 수 있습니다.

## <a name="nuget-assistance"></a>NuGet 지원

올바른 어셈블리를 선택 하려면 NuGet을 사용 하는 개발자 올바른 참조가 필요한을 수동으로 추가 수 하는 동안 (중 하나는 보통 _v4_ 바인딩이나 형식이 전달 _v4_ 어셈블리) 때 NuGet 패키지가 설치 됩니다.

따라서는 `Xamarin.Android.Support.v4` NuGet 패키지에는 이제 다음 논리를 포함 합니다.

앱 API 수준 13 (Gingerbread 3.2) 대상으로 하는 경우 이상:

*   `Xamarin.Android.Support.v13` NuGet은 종속성으로 추가 됩니다.
*   합니다 _형식이 전달_ `Xamarin.Android.Support.v4.dll` 프로젝트에서 참조 됩니다

일반적인 앱 API 수준 13 보다 낮은 아무 것도 대상으로 하는 경우 하면 `Xamarin.Android.Support.v4.dll` 바인딩 프로젝트에서 참조 합니다.

## <a name="do-i-have-to-use-support-v13"></a>지원-v13 사용 해야 합니까?

API 수준 13 이상 앱을 대상으로 선택한 경우 사용 하는 `Xamarin Android Support-v4` NuGet 패키지를 해당 `Xamarin Android Support v13` NuGet 패키지는 필수 종속성입니다.

앱 크기 (17 기술 자료별 다른 두.jar 파일)에서 아주 약간 증가 한 가치가 호환성 및 그 결과 걱정거리 하다 고 생각 합니다.

사용에 대 한 adamant 라면 `Support-v4` 앱에서 대상으로 하는 API 수준 13 이상 항상 수동으로 다운로드할 수 있습니다 또는 `.nupkg`, 압축을 푼 및 어셈블리를 참조 합니다.
