---
title: Xamarin.Essentials 플랫폼 확장
description: Xamarin.Essentials는 Rect, 크기 및 포인트와 같은 플랫폼 형식으로 작업해야 할 때 몇 가지 플랫폼 확장 메서드를 제공합니다.
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: 4e43159fb9cae6646be54d8efc24c334bc071477
ms.sourcegitcommit: fec87846fcb262fc8b79774a395908c8c8fc8f5b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77545154"
---
# <a name="xamarinessentials-platform-extensions"></a>Xamarin.Essentials: 플랫폼 확장

Xamarin.Essentials는 Rect, 크기 및 포인트와 같은 플랫폼 형식으로 작업해야 할 때 몇 가지 플랫폼 확장 메서드를 제공합니다. 즉, iOS, Android 및 UWP의 특정 유형에 대해 이 유형의 `System` 버전 사이를 변환할 수 있습니다. 

## <a name="get-started"></a>시작

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-platform-extensions"></a>플랫폼 확장 사용

클래스에서 Xamarin.Essentials에 대한 참조를 추가합니다.

```csharp
using Xamarin.Essentials;
```

모든 플랫폼 확장은 iOS, Android 또는 UWP 프로젝트에서만 호출할 수 있습니다.

## <a name="android-extensions"></a>Android 확장

이러한 확장은 Android 프로젝트에서만 액세스할 수 있습니다.

### <a name="application-context--activity"></a>애플리케이션 컨텍스트 및 작업

`Platform` 클래스의 플랫폼 확장을 사용하여 실행 중인 앱에 대한 현재 `Context` 또는 `Activity`에 액세스할 수 있습니다.

```csharp

var context = Platform.AppContext;

// Current Activity or null if not initialized or not started.
var activity = Platform.CurrentActivity;
```

`Activity`가 필요하지만 애플리케이션이 완전히 시작되지 않은 상황의 경우 `WaitForActivityAsync` 메서드를 사용해야 합니다.

```csharp
var activity = await Platform.WaitForActivityAsync();
```

### <a name="activity-lifecycle"></a>활동 수명 주기

현재 작업을 가져오는 것 외에도 수명 주기 이벤트를 등록할 수 있습니다.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);

    Xamarin.Essentials.Platform.Init(this, bundle);

    Xamarin.Essentials.Platform.ActivityStateChanged += Platform_ActivityStateChanged;
}

protected override void OnDestroy()
{
    base.OnDestroy();
    Xamarin.Essentials.Platform.ActivityStateChanged -= Platform_ActivityStateChanged;
}

void Platform_ActivityStateChanged(object sender, Xamarin.Essentials.ActivityStateChangedEventArgs e) =>
    Toast.MakeText(this, e.State.ToString(), ToastLength.Short).Show();
```

작업 상태는 다음과 같습니다.

* 만든 날짜
* 다시 시작됨
* 일시 중지됨
* 제거됨
* SaveInstanceState
* 시작됨
* 중지됨

자세한 내용은 [작업 수명 주기](https://docs.microsoft.com/xamarin/android/app-fundamentals/activity-lifecycle/) 설명서를 참조하세요.

## <a name="ios-extensions"></a>iOS 확장

이러한 확장은 iOS 프로젝트에서만 액세스할 수 있습니다.

### <a name="current-uiviewcontroller"></a>현재 UIViewController

현재 표시되는 `UIViewController`에 대한 액세스 권한을 얻습니다.

```csharp
var vc = Platform.GetCurrentUIViewController();
```

이 메서드는 `UIViewController`를 검색할 수 없는 경우 `null`을 반환합니다.

## <a name="cross-platform-extensions"></a>플랫폼 간 확장

이러한 확장은 모든 플랫폼에 존재합니다.

### <a name="point"></a>요소

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### <a name="size"></a>Size

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### <a name="rectangle"></a>사각형

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## <a name="api"></a>API

- [변환기 소스 코드](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [포인트 변환기 API 설명서](xref:Xamarin.Essentials.PointExtensions)
- [사각형 변환기 API 설명서](xref:Xamarin.Essentials.RectangleExtensions)
- [크기 변환기 API 설명서](xref:Xamarin.Essentials.SizeExtensions)
