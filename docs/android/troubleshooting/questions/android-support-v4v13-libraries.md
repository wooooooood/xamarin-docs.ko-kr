---
title: 더 스마트한 Xamarin Android 지원 v4/v13 NuGet 패키지
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: FE66A82A-6C05-4646-BC52-E806F5DC606C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: c74cac6a6d669385855999a565711a3fdc85f3b7
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019532"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>더 스마트한 Xamarin Android 지원 v4/v13 NuGet 패키지

## <a name="about-the-android-support-libraries"></a>Android 지원 라이브러리 정보

Google은 이전 버전의 Android에서 새로운 기능을 사용할 수 있도록 지원 라이브러리를 만들었습니다. 일반적으로 지원 라이브러리의 이름에는 호환되는 가장 낮은 Android API 수준인 버전 번호가 지정됩니다. 예를 들면 다음과 같습니다. 지원-v4는 API 수준 4 이상에서만 사용할 수 있습니다. 이 [Stack Overflow 토론](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13))에 추가 정보가 있습니다. 

두 지원 라이브러리: `Support-v4`와 `Support-v13`은 동일한 앱에서 함께 사용할 수 없습니다. 즉, 상호 배타적입니다. 이는 `Support-v13`이 실제로는 `Support-v4`의 모든 형식 및 구현을 포함하기 때문입니다. 동일한 프로젝트에서 둘 모두를 참조하려 하면 중복 형식 오류가 발생합니다.

## <a name="problems-with-referencing"></a>참조 문제

`Support-v4`는 널리 사용되고 있으므로 이제 많은 타사 라이브러리가 의존합니다. 지원-v13을 대신 사용하도록 선택할 수 있지만, 이러한 타사 라이브러리를 사용하는 모든 앱에는 API 수준을 4까지 지원하는 옵션을 제공하므로 _v4_에 의존하는 것이 더 일반적입니다.

Xamarin 타사 라이브러리가 `Support-v4`에 대한 `Xamarin.Android.Support.v4.dll` 바인딩을 참조하는 경우 이 라이브러리를 사용하는 모든 앱은 `Xamarin.Android.Support.v4.dll`도 참조해야 합니다. 이는 동일한 앱이 `Support-v13`에 대한 `Xamarin.Android.Support.v13.dll` 바인딩에서 일부 기능을 사용하려는 경우 문제가 됩니다. 두 바인딩을 모두 참조하는 경우 중복 형식 오류가 발생합니다.

## <a name="type-forwarded-v4-binding-assembly"></a>형식 전달 v4 바인딩 어셈블리

이 문제를 해결하기 위해 구현이 없는 특수한 `Xamarin.Android.Support.v4.dll` 어셈블리를 만들었지만 모든 `Support-v4` 형식을 `Xamarin.Android.Support.v13.dll` 어셈블리 내의 구현으로 전달하는 `[assembly: TypeForwardedTo (..)]` 특성을 단순화합니다.

즉, 개발자는 앱에서 이 _형식 전달_ 어셈블리를 참조할 수 있습니다. 이는 타사 라이브러리에서 `Xamarin.Android.Support.v4.dll`에 대한 참조를 충족하면서도 여전히 앱에서 `Xamarin.Android.Support.v13.dll`을 사용할 수 있도록 합니다.

## <a name="nuget-assistance"></a>NuGet 지원

개발자는 필요한 올바른 참조를 수동으로 추가할 수 있지만 NuGet 패키지를 설치할 때 NuGet을 사용하여 올바른 어셈블리(일반 _v4_ 바인딩 또는 형식 전달 _v4_ 어셈블리)를 선택할 수 있습니다.

따라서 `Xamarin.Android.Support.v4` NuGet 패키지는 이제 다음과 같은 논리를 포함합니다.

앱이 API 수준 13(Gingerbread 3.2) 이상을 대상으로 하는 경우:

* `Xamarin.Android.Support.v13` NuGet은 종속성으로 자동으로 추가됩니다.
* _형식 전달_ `Xamarin.Android.Support.v4.dll`은 프로젝트에서 참조됩니다.

앱이 API 수준 13 이하를 대상으로 하는 경우 프로젝트에서 참조되는 일반 `Xamarin.Android.Support.v4.dll` 바인딩을 얻게 됩니다.

## <a name="do-i-have-to-use-support-v13"></a>지원-v13을 사용해야 하나요?

앱이 API 수준 13 이상을 대상으로 하고 `Xamarin Android Support-v4` NuGet 패키지를 사용하도록 선택 하는 경우 `Xamarin Android Support v13` NuGet 패키지는 필수 종속성입니다.

앱 크기가 아주 약간 증가하는 것(두 .jar 파일은 17kb와 다름)은 호환성에 충분히 가치가 있으며 그로 인해 발생하는 골칫거리도 적다고 느낍니다.

API 수준 13 이상을 대상으로 하는 앱에서 `Support-v4`를 사용하려는 경우 항상 수동으로 `.nupkg`를 다운로드하고, 압축을 풀고, 어셈블리를 참조할 수 있습니다.
