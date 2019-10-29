---
title: Android 마모 컨트롤
ms.prod: xamarin
ms.assetid: 5B62A5F8-5E55-4B3C-BFC4-E21CDB27C08B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 54ef09af1da484305eecc2107d0245426d11a646
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030304"
---
# <a name="android-wear-controls"></a>Android 마모 컨트롤

Android 마모 된 앱은 `Button`, `TextView`및 이미지 drawables을 비롯 하 여 일반 Android 앱에 이미 사용 중인 동일한 컨트롤을 대부분 사용할 수 있습니다. `ScrollView`, `LinearLayout`및 `RelativateLayout`를 포함 하 여 레이아웃 컨트롤을 사용할 수도 있습니다.

이 페이지는 [Wearable Support](https://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet 패키지를 통해 Xamarin 프로젝트에서 사용할 수 있는 [wearable UI 라이브러리](https://developer.android.com/training/wearables/apps/layouts.html#UiLibrary) 의 Android 마모 관련 컨트롤에 연결 됩니다. 이러한 컨트롤에는 다음이 포함 됩니다.

- **GridViewPager** &ndash;사용자가 선택하여 선택할 수 있는 2 차원 탐색 인터페이스를 만듭니다 (자세한 내용은 [GridViewPager](~/android/wear/user-interface/controls/gridviewpager.md) 참조).

    ![GridViewPager의 예제 스크린샷](images/gridviewpager.png)

앱을 개발 하는 다른 중요 한 컨트롤은 다음과 같습니다.

- `BoxInsetLayout` ( [화면 크기 작업](~/android/wear/screen-sizes.md)참조),

- `WatchViewStub` ( [화면 크기 작업](~/android/wear/screen-sizes.md)참조),

- `CardFrame` ( [Android 카드 만들기](https://developer.android.com/training/wearables/ui/cards.html)참조),

- `CardScrollView` ( [Android 카드 만들기](https://developer.android.com/training/wearables/ui/cards.html)참조),

- `WearableListView` ( [Android 목록 만들기](https://developer.android.com/training/wearables/ui/lists.html)를 참조 하세요.)

## <a name="related-links"></a>관련 링크

- [Wearable 문서](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
