---
title: Xamarin Android RatingBar
description: Android 작업에 RatingBar 위젯을 추가 하는 방법입니다.
ms.prod: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: de63a0f3f6564671a50594c66b55ed095329c95c
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887632"
---
# <a name="xamarinandroid-ratingbar"></a>Xamarin Android RatingBar

RatingBar는 별 1 ~ 5 개의 별 등급을 표시 하는 UI 위젯입니다. 사용자는이 섹션의 별모양에서 눌러 등급을 선택할 수 있습니다. 사용자가 [`RatingBar`](xref:Android.Widget.RatingBar) 위젯을 사용 하 여 등급을 제공할 수 있는 위젯을 만듭니다.

![RatingBar의 예](ratingbar-images/01-ratingbar.png)


## <a name="creating-a-ratingbar"></a>RatingBar 만들기

1. **리소스/레이아웃/기본. axml** 파일을 열고 다음을 추가 합니다.[`RatingBar`](xref:Android.Widget.RatingBar)
   요소 (내 [`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   특성 `android:numStars` 은 등급 표시줄에 표시할 별 수를 정의 합니다. 특성 `android:stepSize` 은 각 별모양의 세분성을 정의 합니다. 예를 들어의 `0.5` 값은 반쪽 별 등급을 허용 합니다.

2. 새 등급이 설정 된 경우 작업을 수행 하려면 다음 코드를의 끝에 추가 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   방법이

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    [`RatingBar`](xref:Android.Widget.RatingBar) [그러면`FindViewById`](xref:Android.App.Activity.FindViewById*) 레이아웃에서 위젯을 캡처한 다음 이벤트 메서드를 설정 하 고 사용자가 등급을 설정 하는 경우 수행할 동작을 정의 합니다. 이 경우 간단한 [`Toast`](xref:Android.Widget.Toast) 메시지에 새 등급이 표시 됩니다.

3. 애플리케이션을 실행합니다.

