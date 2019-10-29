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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019532"
---
# <a name="smarter-xamarin-android-support-v4--v13-nuget-packages"></a>더 스마트한 Xamarin Android 지원 v4/v13 NuGet 패키지

## <a name="about-the-android-support-libraries"></a>Android 지원 라이브러리 정보

Google은 이전 버전의 Android에서 새로운 기능을 사용할 수 있도록 지원 라이브러리를 만들었습니다. 일반적으로 지원 라이브러리는와 호환 되는 가장 낮은 Android API 수준인 이름에 버전 번호를 지정 합니다 (예: 지원-v4는 API 수준 4 이상 에서만 사용할 수 있음). 이 [Stack Overflow 토론](https://stackoverflow.com/questions/9926403/android-support-package-compatibility-library-use-v4-or-v13)의 추가 정보). 

두 지원 라이브러리: `Support-v4` 및 `Support-v13` 동일한 앱에서 함께 사용할 수 없습니다. 즉, 함께 사용할 수 없습니다. 이는 `Support-v13` 실제로 `Support-v4`의 모든 형식 및 구현을 포함 하기 때문입니다. 동일한 프로젝트에서 두 항목을 모두 참조 하는 경우 중복 형식 오류가 발생 합니다.

## <a name="problems-with-referencing"></a>참조 문제

`Support-v4`는 널리 사용 되 고 있으므로 이제 많은 타사 라이브러리에 의존 합니다. 대신 지원-v13에 종속 되도록 선택할 수 있지만, 이러한 타사 라이브러리를 사용 하는 모든 앱에는 API 수준을 4까지 지원 하는 옵션을 제공 하므로 _v4_ 에 의존 하는 것이 더 일반적입니다.

Xamarin 타사 라이브러리가 `Support-v4`에 대 한 `Xamarin.Android.Support.v4.dll` 바인딩을 참조 하는 경우이 라이브러리를 사용 하는 모든 앱은 `Xamarin.Android.Support.v4.dll`도 참조 해야 합니다. 이는 동일한 앱이 `Xamarin.Android.Support.v13.dll` 바인딩의 일부 기능을 `Support-v13`에도 사용 하려고 할 때 문제가 됩니다. 두 바인딩을 모두 참조 하는 경우 중복 형식 오류가 발생 합니다.

## <a name="type-forwarded-v4-binding-assembly"></a>형식 전달 v4 바인딩 어셈블리

이 문제를 해결 하기 위해 구현이 `[assembly: TypeForwardedTo (..)]` 없지만 모든 `Support-v4` 형식을 `Xamarin.Android.Support.v13.dll` 어셈블리 내의 구현으로 전달 하는 특수 한 `Xamarin.Android.Support.v4.dll` 어셈블리를 만들었습니다.

즉, 개발자는 앱에서 이러한 _형식 전달_ 어셈블리를 참조할 수 있습니다 .이 어셈블리는 타사 라이브러리에서 `Xamarin.Android.Support.v4.dll` 하는 참조를 충족 하는 동시에 앱에서 `Xamarin.Android.Support.v13.dll` 사용 하도록 허용 합니다.

## <a name="nuget-assistance"></a>NuGet 지원

개발자는 필요한 올바른 참조를 수동으로 추가할 수 있지만 nuget 패키지를 설치할 때 NuGet을 사용 하 여 올바른 어셈블리 (일반 _v4_ 바인딩 또는 형식 전달 _v4_ 어셈블리)를 선택할 수 있습니다.

따라서 `Xamarin.Android.Support.v4` NuGet 패키지는 이제 다음과 같은 논리를 포함 합니다.

앱이 API 수준 13 (Gingerbread 3.2) 이상을 대상으로 하는 경우:

* `Xamarin.Android.Support.v13` NuGet은 종속성으로 자동으로 추가 됩니다.
* _형식 전달_ `Xamarin.Android.Support.v4.dll` 프로젝트에서 참조 됩니다.

앱이 API 수준 13 보다 낮은 항목을 대상으로 하는 경우 프로젝트에서 참조 되는 일반 `Xamarin.Android.Support.v4.dll` 바인딩이 제공 됩니다.

## <a name="do-i-have-to-use-support-v13"></a>지원-v13를 사용 해야 하나요?

앱이 API 수준 13 이상을 대상으로 하 고 `Xamarin Android Support-v4` NuGet 패키지를 사용 하도록 선택 하는 경우 `Xamarin Android Support v13` NuGet 패키지는 필수 종속성입니다.

앱 크기에서 매우 약간 증가 하는 것을 느낄 수 있습니다 (두 .jar 파일은 17kb와 다름) .이로 인해 호환성과 더 적은 문제를 덜 수 있습니다.

API 수준 13 이상을 대상으로 하는 앱에서 `Support-v4`를 사용 하는 방법에 대 한 adamant 경우 항상 수동으로 `.nupkg`다운로드 하 고, 압축을 풀고, 어셈블리를 참조할 수 있습니다.
