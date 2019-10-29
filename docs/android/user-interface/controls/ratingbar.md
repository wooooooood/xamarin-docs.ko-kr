---
title: Xamarin Android RatingBar
description: Android 작업에 RatingBar 위젯을 추가 하는 방법입니다.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 529fecb4e24e83ef7b783815843e132347d99262
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029148"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin Android RatingBar

RatingBar는 별 1 ~ 5 개의 별 등급을 표시 하는 UI 위젯입니다. 사용자는이 섹션의 별모양에서 눌러 등급을 선택할 수 있습니다. 사용자가 [`RatingBar`](xref:Android.Widget.RatingBar) 위젯을 사용 하 여 등급을 제공할 수 있는 위젯을 만듭니다.

![RatingBar의 예](ratingbar-images/01-ratingbar.png)

## <a name="creating-a-ratingbar"></a>RatingBar 만들기

1. **리소스/레이아웃/기본. axml** 파일을 열고 [`RatingBar`](xref:Android.Widget.RatingBar) 를 추가 합니다.
   요소 ( [`LinearLayout`](xref:Android.Widget.LinearLayout)내):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   `android:numStars` 특성은 등급 표시줄에 표시할 별 수를 정의 합니다. `android:stepSize` 특성은 각 별모양의 세분성을 정의 합니다. 예를 들어 `0.5`의 값은 별 등급 등급을 허용 합니다.

2. 새 등급이 설정 된 경우 작업을 수행 하려면 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 의 끝에 다음 코드를 추가 합니다.
   방법이

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    그러면 [`FindViewById`](xref:Android.App.Activity.FindViewById*) 를 사용 하 여 레이아웃에서 [`RatingBar`](xref:Android.Widget.RatingBar) 위젯을 캡처한 다음 이벤트 메서드를 설정 하 고 사용자가 등급을 설정 하는 경우 수행할 동작을 정의 합니다. 이 경우 간단한 [`Toast`](xref:Android.Widget.Toast) 메시지는 새 등급을 표시 합니다.

3. 애플리케이션을 실행합니다.
