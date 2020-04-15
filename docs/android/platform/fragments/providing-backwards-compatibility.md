---
title: Android 지원 패키지와 이전 버전과의 호환성 제공
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027292"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android 지원 패키지와 이전 버전과의 호환성 제공

이전 버전의 Android 3.0(API 수준 11) 디바이스와의 호환성 없이 조각의 유용성이 제한됩니다. 이 기능을 제공하기 위해 Google은 최신 버전의 Android에서 이전 버전의 Android로 일부 API를 제공하는 [지원 라이브러리](https://developer.android.com/sdk/compatibility-library.html)(원래 출시되었을 때 *Android 호환성 라이브러리*라고 함)를 도입했습니다. Android 1.6(API 수준 4)에서 Android 2.3.3까지 실행하는 디바이스를 지원하는 Android 지원 패키지입니다. (API 수준 10).

> [!NOTE]
> `ListFragment` 및 `DialogFragment`만 Android 지원 패키지를 통해 사용할 수 있습니다. `PreferenceFragment,`와 같은 다른 조각 서브클래스는 Android 지원 패키지에서 지원되지 않습니다. 이전 Android 3.0 애플리케이션에서는 작동하지 않습니다. 

## <a name="adding-the-support-package"></a>지원 패키지 추가

Android 지원 패키지는 Xamarin.Android 애플리케이션에 자동으로 추가되지 않습니다. Xamarin은 Xamarin.Android 애플리케이션에 지원 라이브러리를 간단하게 추가할 수 있도록 [Android 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)를 제공합니다. 지원 패키지를 Xamarin.Android에 포함시키려면 다음 스크린샷에 표시된 것처럼 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 구성 요소를 Xamarin.Android 프로젝트에 포함합니다. 

[![프로젝트에 추가되는 Android 지원 라이브러리 v4 패키지의 스크린샷](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

이러한 단계를 수행한 후에는 이전 버전의 Android에서 조각을 사용할 수 있습니다. Fragment API는 다음과 같은 경우를 제외하고 이러한 이전 버전에서 동일하게 작동합니다. 

- **최소 Android 버전 변경** &ndash; 아래와 같이 애플리케이션은 더 이상 Android 3.0 이상을 대상으로 하지 않아도 됩니다. 

    [![Android 매니페스트에서 설정되는 최소 Android 대상의 스크린샷](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity 확장** &ndash; 조각을 호스팅하는 활동은 이제 `Android.App.Activity`가 아닌 `Android.Support.V4.App.FragmentActivity`에서 상속되어야 합니다. 

- **네임스페이스 업데이트** &ndash; 이제 `Android.App.Fragment`에서 상속되는 클래스는 `Android.Support.V4.App.Fragment`에서 상속되어야 합니다. 소스 코드 파일 상단에 있는 using 문 " `using Android.App;` "을 제거하고 " `using Android.Support.V4.App` "으로 바꿉니다. 

- **SupportFragmentManager 사용** &ndash; `Android.Support.V4.App.FragmentActivity`는 `FragmentManager`에 대한 참조를 가져오는 데 사용해야 하는 `SupportingFragmentManager` 속성을 노출합니다. 예를 들어: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

이러한 변경 내용이 적용되면 Honeycomb 및 Ice Cream Sandwich뿐만 아니라 Android 1.6 또는 2.x에서도 조각 기반 애플리케이션을 실행할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [Android 지원 라이브러리 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
