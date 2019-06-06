---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 12e7e31cda9818a3cb2e2efc331a0be5d0c334e5
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740856"
---
# <a name="gridviewpager"></a>GridViewPager

합니다 [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) 샘플 Android Wear 용 2D 선택기 탐색 패턴을 구현 하는 방법을 보여 줍니다.

![정사각형 화면 GridViewPager의 예에서는 스크린 샷](gridviewpager-images/gridviewpager.png)

먼저 추가 합니다 [Xamarin Android Wear 지원](https://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet 패키지를 프로젝트입니다.

레이아웃 XML은 다음과 같습니다.

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

만들기를 [`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(또는와 같은 하위 클래스입니다. [`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
사용자로 표시 하도록 뷰를 제공 하려면 다음을 탐색 합니다.

합니다 [샘플 어댑터](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) 에 대 한 재정의 포함 하 여 필요한 메서드를 구현 하는 방법을 보여 줍니다 `RowCount`합니다 `GetColumnCount`, `GetBackground`, 및 `GetFragment`

표시 된 것 처럼 어댑터를 연결 합니다.

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>관련 링크

- [Google의 2D 선택기 문서](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android.support.wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (샘플)](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/)
