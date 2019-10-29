---
title: Android 지원 패키지와의 이전 버전과의 호환성 제공
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/12/2017
ms.openlocfilehash: c32d666da1347b947c55209ed7c7ec31a97a70e0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027292"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android 지원 패키지와의 이전 버전과의 호환성 제공

이전 버전의 Android 3.0 (API 수준 11) 장치와의 호환성 없이 조각의 유용성이 제한 됩니다. 이 기능을 제공 하기 위해 Google은 최신 버전의 Android에서 이전 버전의 Android로 몇 가지 Api를 제공 하는 [지원 라이브러리](https://developer.android.com/sdk/compatibility-library.html) (원래 출시 되었을 때 *android 호환성 라이브러리* 라고 함)를 도입 했습니다. Android 1.6 (API 수준 4)을 실행 하는 장치를 Android 2.3.3으로 사용할 수 있도록 하는 Android 지원 패키지입니다. (API 수준 10).

> [!NOTE]
> Android 지원 패키지를 통해 `ListFragment` 및 `DialogFragment`만 사용할 수 있습니다. `PreferenceFragment,`와 같은 다른 조각 서브 클래스는 Android 지원 패키지에서 지원 되지 않습니다. Android 이전 3.0 응용 프로그램에서는 작동 하지 않습니다. 

## <a name="adding-the-support-package"></a>지원 패키지 추가

Android 지원 패키지는 Xamarin Android 응용 프로그램에 자동으로 추가 되지 않습니다. Xamarin은 [Android 지원 라이브러리 V4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 를 제공 하 여 지원 라이브러리를 xamarin.ios 응용 프로그램에 간편 하 게 추가할 수 있도록 합니다. Xamarin. Android 응용 프로그램에 지원 패키지를 포함 하려면 다음 스크린샷에 표시 된 것 처럼 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 구성 요소를 xamarin android 프로젝트에 포함 합니다. 

[프로젝트에 추가 되는 Android 지원 라이브러리 v4 패키지의![스크린샷](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

이러한 단계를 수행한 후 이전 버전의 Android에서 조각을 사용할 수 있게 됩니다. 조각 Api는 다음과 같은 경우를 제외 하 고 이러한 이전 버전에서 동일 하 게 작동 합니다. 

- 아래와 같이 응용 프로그램에서 더 이상 Android 3.0 이상을 대상으로 하지 않아도 되는 **최소 Android 버전 &ndash; 변경 합니다** . 

    [Android 매니페스트에서 설정 되는 최소 Android 대상의 스크린샷![](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity &ndash; 확장** 호스트 조각이 있는 활동은 이제 `Android.App.Activity` 아닌 `Android.Support.V4.App.FragmentActivity`에서 상속 되어야 합니다. 

- `Android.App.Fragment`에서 상속 되는 클래스 &ndash; **네임 스페이스 업데이트** 는 이제 `Android.Support.V4.App.Fragment`에서 상속 되어야 합니다. 소스 코드 파일의 맨 위에 있는 using 문 "`using Android.App;`"를 제거 하 고 "`using Android.Support.V4.App`"로 바꿉니다. 

- **SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity`를 사용 하 여 `FragmentManager`에 대 한 참조를 가져오는 데 사용 해야 하는 `SupportingFragmentManager` 속성을 노출 합니다. 예를 들면, 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

이러한 변경 내용을 적용 한 후에는 Android 1.6 또는 2.x 뿐만 아니라 Honeycomb 및 아이스크림의 샌드위치에서 조각 기반 응용 프로그램을 실행할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [Android 지원 라이브러리 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
