---
title: RatingBar
description: Android 활동 RatingBar 위젯을 추가 하는 방법입니다.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 97d2a126be70e210d2e8f4ebf4d7a25ff8777a02
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131428"
---
# <a name="ratingbar"></a>RatingBar

RatingBar 1 ~ 5 개의 별 등급을 표시 하는 UI 위젯입니다. 이 섹션에 있는 별에서 눌러 사용자가 등급을 선택할 수, 사용 하 여 등급을 제공 하도록 사용자를 허용 하는 위젯을 만들어야 합니다 [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) 위젯입니다.

![RatingBar의 예](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>RatingBar 만들기

1. 엽니다는 **Resource/layout/Main.axml** 파일을 추가 합니다 [`RatingBar`](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/)
   요소 (내 합니다 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

    ```xml
    <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
    ```
   `android:numStars` 개수 별 등급 표시줄에 표시할 특성을 정의 합니다. 합니다 `android:stepSize` 각 스타 세분성을 정의 하는 특성 (예를 들어 값 `0.5` 별을 반 연령별 등급 허용 됩니다).

2. 새 등급 설정 되어 있는 것을 끝에 다음 코드를 추가 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
   방법:

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    캡처합니다 합니다 [ `RatingBar` ](https://developer.xamarin.com/api/type/Android.Widget.RatingBar/) 사용 하 여 레이아웃을 통해 위젯을 [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/) 다음 이벤트 메서드를 설정 하 고 사용자가 등급을 설정 하는 경우 수행할 동작을 정의 합니다. 에이 경우 간단한 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 메시지 새 등급을 표시 합니다.

3.  응용 프로그램을 실행합니다.

