---
title: "이전 버전과 Android 지원 패키지와의 호환성을 제공"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: 670ec465843bbe819b41a53fff71b01ab78b0059
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>이전 버전과 Android 지원 패키지와의 호환성을 제공

조각의 유용성 것이 이전 버전과 호환성을 미리 Android 3.0 (API 수준 11) 장치 없이 제한 합니다. 이 기능을 제공 하려면 Google 도입는 [지원 라이브러리](http://developer.android.com/sdk/compatibility-library.html) (원래 호출는 *Android 호환 라이브러리* 릴리스된 경우)는 backports 몇 가지 최신 버전의 Api 이전 버전의 Android는 android 합니다. Android 2.3.3를 Android 1.6 (API 수준 4)를 실행 하는 장치 수 있는 Android 지원 패키지는 (API 수준 10)입니다.

> [!NOTE]
> 만 `ListFragment` 및 `DialogFragment` Android 지원 패키지를 통해 사용할 수 있습니다. 하위 클래스와 같은 조각을 있는 다른는 `PreferenceFragment,` Android 지원 패키지에서 지원 됩니다. 3.0 미리 Android 응용 프로그램에서 작동 하지 않습니다. 


## <a name="adding-the-support-package"></a>지원 패키지를 추가합니다.

Android 지원 패키지 Xamarin.Android 응용 프로그램에 자동으로 추가 되지 않습니다. Xamarin에서는 [Android 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 를 간단히 지원 라이브러리 Xamarin.Android 응용 프로그램에 추가할 합니다. 응용 프로그램에 포함 하 여 Xamarin.Android에 지원 패키지를 포함 하도록는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 다음 스크린샷에 표시 된 것 처럼 Xamarin.Android 프로젝트에 구성 요소: 

[![Android 지원 라이브러리 스크린 샷 v4 패키지를 프로젝트에 추가 되 고](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png#lightbox)

다음이 단계를 수행한 후 이전 버전의 Android 조각을 사용 하 여 표시 됩니다. 조각 Api가 동일한는 이제, 다음의 예외가이 이전 버전에서 작동 하는 것: 

-   **최소 Android 버전을 변경** &ndash; 응용 프로그램이 더 이상 필요 없는 아래 표시 된 대로 Android 3.0 이상을 대상으로 합니다. 

    [![최소 Android의 스크린 샷 대상 응용 프로그램 속성에서 설정 되 고](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png#lightbox)

-   **FragmentActivity 확장** &ndash; 조각을 호스트 하는 활동에서 상속 해야 `Android.Support.V4.App.FragmentActivity` 에서 아니라 `Android.App.Activity` 합니다. 

-   **네임 스페이스 업데이트** &ndash; 에서 상속 된 클래스 `Android.App.Fragment` 에서 상속 해야 `Android.Support.V4.App.Fragment` 합니다. 사용 하 여 제거 문을 " `using Android.App;` " 소스 코드의 맨 위쪽에 파일을 사용 하 여 대체 " `using Android.Support.V4.App` "입니다. 

-   **SupportFragmentManager를 사용 하 여** &ndash; `Android.Support.V4.App.FragmentActivity` 노출 한 `SupportingFragmentManager` 속성에 대 한 참조를 가져오는 데 사용할 수 있어야 하는 `FragmentManager` 합니다. 예: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

위치에 이러한 변경 내용으로 Honeycomb 및 아이스크림 샌드위치 뿐 아니라 Android 1.6 또는 2.x에 조각의 기반 응용 프로그램을 실행할 수 됩니다. 


## <a name="related-links"></a>관련 링크

- [Android 지원 라이브러리 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
