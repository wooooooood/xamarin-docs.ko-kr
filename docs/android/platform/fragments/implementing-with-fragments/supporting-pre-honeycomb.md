---
title: 지원 패키지를 사용 하 여 사전 Honeycomb Android 지원
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 75e12821d96b98037c568fb5dac69ba53a507670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>지원 패키지를 사용 하 여 사전 Honeycomb Android 지원

*Android 지원 패키지* 다시 몇 가지 새로운 API를 이식 하는 라이브러리 이루어져 &ndash; 조각과 같은 &ndash; 이전 버전의 Android 합니다. 따라서 Android 지원 패키지를 추가 하 여 우리 실행 응용 프로그램이 Android 2.3 장치의 다음 화면으로 이동 하 여 표시 된 것 처럼 됩니다.

[![조각 연습 및 활동 세부 정보 스크린 샷](supporting-pre-honeycomb-images/01-sml.png)](supporting-pre-honeycomb-images/01.png#lightbox)

## <a name="adding-the-support-package"></a>지원 패키지를 추가합니다.

Android 지원 패키지 Xamarin.Android 응용 프로그램에 자동으로 추가 되지 않습니다. Xamarin에서는 [Android 지원 라이브러리 v4 NuGet 패키지](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 를 간단히 지원 라이브러리 Xamarin.Android 응용 프로그램에 추가할 합니다.
응용 프로그램에 포함 하 여 Xamarin.Android에 지원 패키지를 포함 하도록는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) 다음 스크린샷에 표시 된 것 처럼 Xamarin.Android 프로젝트에 구성 요소:

[![Android 지원 라이브러리 v4 패키지 추가](supporting-pre-honeycomb-images/02-sml.png)](supporting-pre-honeycomb-images/02.png#lightbox)

패키지 추가 되 면 대상 프레임 워크를 Android 2.2 이상 변경 합니다.

[![대상 프레임 워크 API 수준 변경의 스크린 샷](supporting-pre-honeycomb-images/03-sml.png)](supporting-pre-honeycomb-images/03.png#lightbox)

또한 동일한 API 수준 최소 Android 버전을 대상으로 있는지 확인 합니다.

[![최소 Android 버전을 설정할 때의 스크린 샷](supporting-pre-honeycomb-images/04-sml.png)](supporting-pre-honeycomb-images/04.png#lightbox)

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>MainActivity FragmentActivity에서 파생 되도록 변경 합니다.

조각을 사용 하는 모든 활동에서 상속 해야 `Xamarin.Support.V4.App.FragmentActivity`합니다. 이 클래스는 조각 Android 버전에 관계 없이 활동에 의해 호스트 될 수 있기 때문에 지원 패키지는 필요한 일부입니다. `MainActivity` 하나의 작은 변경 필요-에서 상속 해야 이제 `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>DetailsActivity FragmentActivity에서 파생 되도록 변경 합니다.

`DetailsActivity` 변경 해야는 `Activity` 에 `FragmentActivity`합니다. 으로 `FragmentManager` 은 사전 Honeycomb 버전 Android의 호환 되지 않는 Android 지원 패키지는 래퍼 클래스 포함 `SupportFragmentManager`, 이전 버전과 호환성을 제공 하 합니다. 각 `FragmentActivity` 에 `SupportFragmentManager` 속성 및 `DetailsActivity` 사용 하도록 변경는 `SupportFragmentManager` 대신는 `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

이러한 변경을 완료 하는 것 다음 조각을 사용 하 여 대상 장치의 크기에는 UI를 조정 하 고 Android 1.6 및 이상 버전에서 실행할 수 있는 응용 프로그램을 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [Android 라이브러리 v4 지원](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
