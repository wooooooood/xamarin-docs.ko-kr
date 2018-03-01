---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) 샘플 쓰는 유형 Android에 대 한 2D 선택 탐색 패턴을 구현 하는 방법을 보여 줍니다.

![정사각형 디스플레이에 GridViewPager의 예제 스크린 샷](gridviewpager-images/gridviewpager.png)

먼저 추가 [Xamarin Android 착용 지원](http://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet 패키지를 프로젝트입니다.

레이아웃 XML은 다음과 같습니다.

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

만들기는 [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (또는 하위 클래스와 같은 [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) 을 탐색 하기 때문에 표시할 보기를 제공 합니다.

[샘플 어댑터](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) 에 대 한 재정의 포함 하 여 필요한 메서드를 구현 하는 방법을 보여 줍니다. `RowCount`, `GetColumnCount`, `GetBackground`, 및 `GetFragment`

표시 된 대로 어댑터를 연결 합니다.

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>관련 링크

- [Google의 2D 선택 문서](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android.support.wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (샘플)](https://developer.xamarin.com/samples/GridViewPager/)
