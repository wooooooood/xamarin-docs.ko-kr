---
title: 이전 버전과 호환 Android 지원 패키지를 제공
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 18dac665eec1c5d3ac64065c37e73022670c1ba5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108849"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>이전 버전과 호환 Android 지원 패키지를 제공

조각의 유용성 것 없이 이전 버전과 호환성 사전 Android 3.0 (API 수준 11) 장치를 사용 하 여 제한 합니다. Google이이 기능을 제공 하기 위해 도입 합니다 [지원 라이브러리](http://developer.android.com/sdk/compatibility-library.html) (원래 호출를 *Android Compatibility Library* 릴리스된 경우)는 backports 몇 가지 최신 버전의 Api 이전 버전의 Android android입니다. Android 2.3.3에 Android 1.6 (API 수준 4)를 실행 하는 장치를 사용 하도록 설정 하는 Android 지원 패키지입니다. (API 수준 10)입니다.

> [!NOTE]
> 만 `ListFragment` 하며 `DialogFragment` Android 지원 패키지를 통해 사용할 수 있습니다. 하위 클래스를 같은 조각을 다른 하나도 `PreferenceFragment,` Android 지원 패키지에서 지원 됩니다. Android 3.0 응용 프로그램에서 작동 하지 않습니다. 


## <a name="adding-the-support-package"></a>지원 패키지를 추가합니다.

Android 지원 패키지를 Xamarin.Android 응용 프로그램에 자동으로 추가 되지 않습니다. Xamarin은 제공 된 [Android 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 지원 라이브러리 Xamarin.Android 응용 프로그램에 추가 하 고 간소화 하기 위해. 응용 프로그램에 포함 하 여 Xamarin.Android에 지원 패키지를 포함 하는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 다음 스크린샷에 표시 된 것과 같이 Xamarin.Android 프로젝트에 구성 요소: 

[![프로젝트에 추가 되는 Android 지원 라이브러리의 스크린 샷 v4 패키지](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

다음이 단계를 수행한 후 조각 이전 버전의 Android에서 사용할 수 있습니다. 조각 Api가 동일한 이제 다음과 같은 예외를 사용 하 여 이러한 버전에서 작동 하는 것: 

-   **최소 Android 버전 변경** &ndash; 응용 프로그램 아래와 같이 Android 3.0 이상을 대상으로 더 이상 필요 합니다. 

    [![Android 매니페스트에서 설정 되는 최소 Android의 스크린 샷 대상](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **FragmentActivity 확장** &ndash; 조각을 호스트 하는 The 활동에서 상속 해야 `Android.Support.V4.App.FragmentActivity` 에서 아니라 `Android.App.Activity` 합니다. 

-   **네임 스페이스를 업데이트할** &ndash; 클래스에서 상속 되 `Android.App.Fragment` 에서 상속 해야 `Android.Support.V4.App.Fragment` 합니다. 사용 하 여 제거 문을 " `using Android.App;` " 소스 코드의 맨 위에 있는 파일을 사용 하 여 대체 " `using Android.Support.V4.App` "입니다. 

-   **SupportFragmentManager 사용** &ndash; `Android.Support.V4.App.FragmentActivity` 노출를 `SupportingFragmentManager` 에 대 한 참조를 가져오는 데 사용할 수 있어야 하는 속성을 `FragmentManager` 입니다. 예를 들어: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

이러한 변경이 사용 하 여 Android 1.6 또는 2.x 에서도 Honeycomb 및 Ice Cream Sandwich 조각 기반 응용 프로그램을 실행할 수 됩니다. 


## <a name="related-links"></a>관련 링크

- [Android 지원 라이브러리 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
