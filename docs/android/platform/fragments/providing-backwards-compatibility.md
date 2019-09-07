---
title: Android 지원 패키지와의 이전 버전과의 호환성 제공
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/12/2017
ms.openlocfilehash: 838f8bdcf3bd82a31bf0d033eee628bd19ad1c30
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757545"
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Android 지원 패키지와의 이전 버전과의 호환성 제공

이전 버전의 Android 3.0 (API 수준 11) 장치와의 호환성 없이 조각의 유용성이 제한 됩니다. 이 기능을 제공 하기 위해 Google은 최신 버전의 Android에서 이전 버전의 Android로 몇 가지 Api를 제공 하는 [지원 라이브러리](https://developer.android.com/sdk/compatibility-library.html) (원래 출시 되었을 때 *android 호환성 라이브러리* 라고 함)를 도입 했습니다. Android 1.6 (API 수준 4)을 실행 하는 장치를 Android 2.3.3으로 사용할 수 있도록 하는 Android 지원 패키지입니다. (API 수준 10).

> [!NOTE]
> `ListFragment` 및만Android지원패키지를`DialogFragment` 통해 사용할 수 있습니다. 와 같은 다른 조각 서브 클래스는 Android 지원 패키지 `PreferenceFragment,` 에서 지원 되지 않습니다. Android 이전 3.0 응용 프로그램에서는 작동 하지 않습니다. 

## <a name="adding-the-support-package"></a>지원 패키지 추가

Android 지원 패키지는 Xamarin Android 응용 프로그램에 자동으로 추가 되지 않습니다. Xamarin은 [Android 지원 라이브러리 V4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 를 제공 하 여 지원 라이브러리를 xamarin.ios 응용 프로그램에 간편 하 게 추가할 수 있도록 합니다. Xamarin. Android 응용 프로그램에 지원 패키지를 포함 하려면 다음 스크린샷에 표시 된 것 처럼 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 구성 요소를 xamarin android 프로젝트에 포함 합니다. 

[![프로젝트에 추가 되는 Android 지원 라이브러리 v4 패키지의 스크린샷](providing-backwards-compatibility-images/02-sml.png)](providing-backwards-compatibility-images/02.png#lightbox)

이러한 단계를 수행한 후 이전 버전의 Android에서 조각을 사용할 수 있게 됩니다. 조각 Api는 다음과 같은 경우를 제외 하 고 이러한 이전 버전에서 동일 하 게 작동 합니다. 

- **최소 Android 버전 변경** &ndash; 응용 프로그램은 아래와 같이 더 이상 Android 3.0 이상을 대상으로 할 필요가 없습니다. 

    [![Android 매니페스트에서 설정 되는 최소 Android 대상의 스크린샷](providing-backwards-compatibility-images/03-sml.png)](providing-backwards-compatibility-images/03.png#lightbox)

- **FragmentActivity 확장** 이제 조각을 호스팅하는 활동은에서 `Android.Support.V4.App.FragmentActivity` `Android.App.Activity` 상속 되어야 합니다. &ndash; 

- **네임 스페이스 업데이트** 에서 상속 되는 클래스는 이제에서 `Android.Support.V4.App.Fragment` 상속 되어야 합니다. `Android.App.Fragment` &ndash; 소스 코드 파일의 맨 `using Android.App;` 위에 있는 using 문 ""을 제거 하 고 " `using Android.Support.V4.App` "로 바꿉니다. 

- **SupportFragmentManager 사용** 에 대 한 참조를 가져오는 데 사용 해야 하는 속성을 `SupportingFragmentManager` 노출 합니다. `FragmentManager` &ndash; `Android.Support.V4.App.FragmentActivity` 예를 들어: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

이러한 변경 내용을 적용 한 후에는 Android 1.6 또는 2.x 뿐만 아니라 Honeycomb 및 아이스크림의 샌드위치에서 조각 기반 응용 프로그램을 실행할 수 있습니다. 

## <a name="related-links"></a>관련 링크

- [Android 지원 라이브러리 v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
