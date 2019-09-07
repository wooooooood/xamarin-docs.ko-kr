---
title: Android 마모 1.0에 대 한 시청 얼굴 만들기
description: 이 가이드에서는 Android 마모 1.0에 대 한 사용자 지정 시계 얼굴 서비스를 구현 하는 방법을 설명 합니다. 제거 된 디지털 watch face 서비스를 빌드하기 위한 단계별 지침이 제공 되며, 아날로그 스타일의 조사식을 만들기 위한 더 많은 코드가 제공 됩니다.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: a6dfab949eb19708f69d838a7c792f2e7bbd76b3
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758512"
---
# <a name="creating-a-watch-face"></a>시계 모드 만들기

_이 가이드에서는 Android 마모 1.0에 대 한 사용자 지정 시계 얼굴 서비스를 구현 하는 방법을 설명 합니다. 제거 된 디지털 watch face 서비스를 빌드하기 위한 단계별 지침이 제공 되며, 아날로그 스타일의 조사식을 만들기 위한 더 많은 코드가 제공 됩니다._

## <a name="overview"></a>개요

이 연습에서는 사용자 지정 Android 마모 1.0 watch 얼굴을 만들기 위한 필수 정보를 설명 하는 기본 watch 얼굴 서비스를 만듭니다.
초기 조사식 얼굴 서비스는 시간 및 분 단위로 현재 시간을 표시 하는 간단한 디지털 감시를 표시 합니다.

[![디지털 시청 얼굴](creating-a-watchface-images/01-initial-face.png "초기 디지털 시계 표면의 스크린샷 예")](creating-a-watchface-images/01-initial-face.png#lightbox)

이 디지털 시청 얼굴을 개발 하 고 테스트 한 후에는 세 가지를 사용 하 여 보다 정교한 아날로그 시청 표면으로 업그레이드 하는 더 많은 코드가 추가 됩니다.

[![아날로그 조사식 얼굴](creating-a-watchface-images/02-example-watchface.png "최종 아날로그 시청 표면의 스크린샷 예")](creating-a-watchface-images/02-example-watchface.png#lightbox)

시청 얼굴 서비스는 마모 된 1.0 앱의 일부로 번들 되 고 설치 됩니다. 다음 예제 `MainActivity` 에서는 응용 프로그램의 일부로 watch face 서비스를 패키지 하 고 스마트 watch에 배포할 수 있도록 마모 된 1.0 앱 템플릿의 코드를 포함 하지 않습니다. 실제로이 앱은 디버그 및 테스트를 위해 사용 하는 조사식 1.0 장치 (또는 에뮬레이터)에 조사식을 로드 하기 위한 수단으로 서만 제공 됩니다.

## <a name="requirements"></a>요구 사항

Watch face 서비스를 구현 하려면 다음이 필요 합니다.

- 마모 된 장치 또는 에뮬레이터에서 Android 5.0 (API 레벨 21) 이상

- Xamarin [Android 마모 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) 를 xamarin.ios 프로젝트에 추가 해야 합니다.

Android 5.0는 시청 얼굴 서비스를 구현 하는 데 필요한 최소 API 수준이 긴 하지만 Android 5.1 이상이 권장 됩니다. Android 5.1 (API 22) 이상을 실행 하는 android 장치는 장치를 저전력 *주변* 모드에 있는 동안 앱에서 화면에 표시 되는 내용을 제어할 수 있습니다. 장치가 저전력 *주변* 모드에서 벗어나면 *대화형* 모드입니다. 이러한 모드에 대 한 자세한 내용은 [앱 표시 유지](https://developer.android.com/training/wearables/apps/always-on.html)를 참조 하세요.

## <a name="start-an-app-project"></a>앱 프로젝트 시작

**WatchFace** 라는 새 Android 마모 1.0 프로젝트를 만듭니다. 새 xamarin.ios 프로젝트를 만드는 방법에 대 한 자세한 내용은 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md)를 참조 하세요.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-vs-sml.png "새 프로젝트 대화 상자에서 마모 된 앱 선택")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-xs-sml.png "새 프로젝트 대화 상자에서 마모 된 앱 선택")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----

패키지 이름을 `com.xamarin.watchface`다음과 같이 설정 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![패키지 이름 설정](creating-a-watchface-images/04-package-name-vs.png "패키지 이름을 watchface로 설정") 합니다.](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![패키지 이름 설정](creating-a-watchface-images/04-package-name-xs.png "패키지 이름을 watchface로 설정") 합니다.](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

또한 아래로 스크롤하고 **인터넷** 및 **WAKE_LOCK** 사용 권한을 사용 하도록 설정 합니다.

[![필요한 권한](creating-a-watchface-images/05-required-permissions-vs.png "인터넷 및 WAKE_LOCK 권한 사용")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Android **5.1 (API 수준 22)** 의 최소 android 버전을 설정 합니다.
또한 **인터넷** 및 **WakeLock** 사용 권한을 사용 하도록 설정 합니다.

[![필요한 권한](creating-a-watchface-images/05-required-permissions-xs.png "인터넷 및 WakeLock 권한 사용")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

다음으로, [preview .png](creating-a-watchface-images/preview.png) &ndash; 를 다운로드 합니다 .이 연습의 뒷부분에 나오는 **drawables** 폴더에 추가 됩니다.

## <a name="add-the-xamarinandroid-wear-package"></a>Xamarin Android 패키지 추가

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

NuGet 패키지 관리자를 시작 합니다. Visual Studio에서 **솔루션 탐색기** **참조** 를 마우스 오른쪽 단추로 클릭 하 고 **nuget 패키지 관리 ...** 를 선택 합니다. 프로젝트를 안정적인 최신 버전의 **xamarin.ios**로 업데이트 합니다.

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.ios 패키지 추가")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

그런 다음 **v13** 가 설치 되 면 제거 합니다.

[![NuGet 패키지 관리자 제거](creating-a-watchface-images/07-uninstall-v13-sml.png "V13를 제거 합니다.")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

NuGet 패키지 관리자를 시작 합니다. Mac용 Visual Studio의 **솔루션 창** 에서 **패키지** 를 마우스 오른쪽 단추로 클릭 하 고 **패키지 추가**...를 선택 합니다. 프로젝트를 안정적인 최신 버전의 **xamarin.ios**로 업데이트 합니다.

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.ios 패키지 추가")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----

마모 된 장치 또는 에뮬레이터에서 앱을 빌드하고 실행 합니다 .이 작업을 수행 하는 방법에 대 한 자세한 내용은 [시작](~/android/wear/get-started/index.md) 가이드를 참조 하세요. 다음 앱 화면이 마모 장치에 표시 됩니다.

[![앱 스크린샷](creating-a-watchface-images/08-app-screen.png "장치 마모의 앱 화면")](creating-a-watchface-images/08-app-screen.png#lightbox)

이 시점에서 기본 마모 된 앱은 아직 watch 얼굴 서비스 구현을 제공 하지 않기 때문에 조사식 얼굴 기능을 제공 하지 않습니다. 이 서비스는 다음에 추가 될 예정입니다.

## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android 마모는 클래스를 통해 조사식 `CanvasWatchFaceService` 얼굴을 구현 합니다. `CanvasWatchFaceService`는 다음 다이어그램과 `WatchFaceService`같이에서 `WallpaperService` 파생 되는에서 파생 됩니다.

[![상속 다이어그램](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 상속 다이어그램")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService`에 중첩 `CanvasWatchFaceService.Engine`된가 포함 되어 있습니다 `CanvasWatchFaceService.Engine` .이는 조사식 얼굴을 그리는 실제 작업을 수행 하는 개체를 인스턴스화합니다. `CanvasWatchFaceService.Engine`는 위의 다이어그램 `WallpaperService.Engine` 에 표시 된 것 처럼에서 파생 됩니다.

이 다이어그램 `Canvas` 에 표시 되지 않은는를 `CanvasWatchFaceService` 사용 하 여 조사식을 그리는 &ndash; 데 `Canvas` 사용 됩니다 .이는 `OnDraw` 아래에 설명 된 대로 메서드를 통해 전달 됩니다.

다음 섹션에서는 다음 단계를 수행 하 여 사용자 지정 watch face 서비스를 만듭니다.

1. `MyWatchFaceService` 에서`CanvasWatchFaceService`파생 되는 이라는 클래스를 정의 합니다.

2. 내 `MyWatchFaceService`에서 `MyWatchFaceEngine` 에서`CanvasWatchFaceService.Engine`파생 된 이라는 중첩 된 클래스를 만듭니다.

3. 에서 `MyWatchFaceService` `CreateEngine` 인스턴스화하고 반환`MyWatchFaceEngine` 하는 메서드를 구현 합니다.

4. 에서 `MyWatchFaceEngine`메서드를 `OnCreate` 구현 하 여 조사식 얼굴 스타일을 만들고 다른 초기화 작업을 수행 합니다.

5. `OnDraw` 의`MyWatchFaceEngine`메서드를 구현 합니다. 이 메서드는 조사식 얼굴을 다시 그려야 할 때마다 호출 됩니다 (즉, *무효화*됨). `OnDraw`는 시간, 분 및 초 바늘과 같은 조사식 얼굴 요소를 그리고 다시 그리는 메서드입니다.

6. `OnTimeTick` 의`MyWatchFaceEngine`메서드를 구현 합니다.
    `OnTimeTick`는 1 분에 한 번 이상 (주변 모드와 대화형 모드에서) 또는 날짜/시간이 변경 된 경우 호출 됩니다.

에 대 한 `CanvasWatchFaceService`자세한 내용은 Android [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API 설명서를 참조 하세요.
마찬가지로 [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) 는 시계 모양의 실제 구현을 설명 합니다.

### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService 추가

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

**MyWatchFaceService.cs** 라는 새 파일을 추가 합니다. Visual Studio에서 **솔루션 탐색기** **WatchFace** 를 마우스 오른쪽 단추로 클릭 하 고 **> 새 항목 추가**...를 클릭 한 다음 **클래스**를 선택 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

**MyWatchFaceService.cs** 라는 새 파일을 추가 합니다. Mac용 Visual Studio에서 **WatchFace** 프로젝트를 마우스 오른쪽 단추로 클릭 하 **> 새 파일 추가**...를 클릭 하 고 **빈 클래스**를 선택 합니다.

-----

이 파일의 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService`(에서 `CanvasWatchFaceService`파생 됨)은 조사식 표면의 "주 프로그램"입니다. `MyWatchFaceService``OnCreateEngine`는에서 `MyWatchFaceEngine` `MyWatchFaceEngine` 파생 되는 개체를 인스턴스화하고 반환 하는 메서드를 하나만 구현 합니다. `CanvasWatchFaceService.Engine` 인스턴스화된 `MyWatchFaceEngine` 개체는 `WallpaperService.Engine`로 반환 되어야 합니다. 캡슐화 `MyWatchFaceService` 된 개체는 생성자에 전달 됩니다.

`MyWatchFaceEngine`실제 조사식 얼굴 구현 &ndash; 에는 조사식 얼굴을 그리는 코드가 포함 됩니다. 또한 화면 변경 (주변/대화형 모드, 화면 끄기 등) 등의 시스템 이벤트도 처리 합니다.

### <a name="implement-the-engine-oncreate-method"></a>엔진 OnCreate 메서드 구현

메서드 `OnCreate` 는 조사식 표면을 초기화 합니다. 다음 필드를에 `MyWatchFaceEngine`추가 합니다.

```csharp
Paint hoursPaint;
```

이 `Paint` 개체는 시계 화면에 현재 시간을 그리는 데 사용 됩니다. 다음으로 다음 메서드를에 `MyWatchFaceEngine`추가 합니다.

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate`는 `MyWatchFaceEngine` 가 시작 된 직후에 호출 됩니다. 을 `WatchFaceStyle` 설정 하 고 (장치에서 사용자와 상호 작용 하는 방법을 제어) 시간을 표시 `Paint` 하는 데 사용 되는 개체를 인스턴스화합니다.

를 `SetWatchFaceStyle` 호출 하면 다음이 수행 됩니다.

1. *미리 보기 모드* 를 `PeekModeShort`로 설정 합니다. 그러면 알림이 디스플레이에서 작은 "피킹" 카드로 표시 됩니다.

2. 배경 표시 유형을로 `Interruptive`설정 합니다. 그러면 피킹 (peeking) 카드의 배경이 비파괴적 이며 방해 알림을 나타내는 경우 간략하게 표시 됩니다.

3. 사용자 지정 조사식이 대신 시간을 표시할 수 있도록 기본 시스템 UI 시간이 조사식에 그려지지 않도록 합니다.

이러한 색 및 기타 조사식 얼굴 스타일 옵션에 대 한 자세한 내용은 Android [WatchFaceStyle](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API 설명서를 참조 하세요.

이 `SetWatchFaceStyle` 완료 되 `OnCreate` 면는 `Paint` 개체 (`hoursPaint`)를 인스턴스화하고 해당 색을 흰색으로, 텍스트 크기를 48 픽셀 ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) 로 지정 해야 함)로 설정 합니다.

### <a name="implement-the-engine-ondraw-method"></a>엔진 OnDraw 메서드 구현

메서드 `OnDraw` 는 숫자 및 시계 표면 바늘 `CanvasWatchFaceService.Engine` 과 &ndash; 같은 실제로 조사식 얼굴 요소를 그리는 메서드인 경우에 가장 중요 한 메서드입니다.
다음 예제에서는 조사식 면에 시간 문자열을 그립니다.
`MyWatchFaceEngine`에 다음 메서드를 추가합니다.

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Android는를 `OnDraw`호출할 때 `Canvas` 인스턴스 및 얼굴을 그릴 수 있는 범위를 전달 합니다. 위의 코드 예제 `DateTime` 에서는 시간 및 분 (12 시간 형식)으로 현재 시간을 계산 하는 데 사용 됩니다. 결과 시간 문자열은 메서드를 `Canvas.DrawText` 사용 하 여 캔버스에 그려집니다. 문자열은 왼쪽 가장자리에서 70 픽셀, 위쪽 가장자리부터 80 픽셀까지 표시 됩니다.

`OnDraw` 메서드에 대 한 자세한 내용은 Android [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) API 설명서를 참조 하세요.

### <a name="implement-the-engine-ontimetick-method"></a>엔진 OnTimeTick 메서드 구현

Android는 주기적으로 `OnTimeTick` 메서드를 호출 하 여 시계 면에서 표시 되는 시간을 업데이트 합니다. 1 분에 한 번 이상 (주변 모드와 대화형 모드에서) 또는 날짜/시간 또는 표준 시간대가 변경 된 경우에 호출 됩니다. `MyWatchFaceEngine`에 다음 메서드를 추가합니다.

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

의 `OnTimeTick` 이 구현은를 단순히 `Invalidate`호출 합니다. 메서드 `Invalidate` 는 조사식 `OnDraw` 얼굴을 다시 그리도록 예약 합니다.

`OnTimeTick` 메서드에 대 한 자세한 내용은 Android [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API 설명서를 참조 하세요.

## <a name="register-the-canvaswatchfaceservice"></a>CanvasWatchFaceService 등록

`MyWatchFaceService`연결 된 앱의 **Androidmanifest** 에 등록 해야 합니다. 이렇게 하려면 `<application>` 섹션에 다음 XML을 추가 합니다.

```xml
<service
    android:name="watchface.MyWatchFaceService"
    android:label="Xamarin Sample"
    android:allowEmbedded="true"
    android:taskAffinity=""
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data
        android:name="android.service.wallpaper"
        android:resource="@xml/watch_face" />
    <meta-data
        android:name="com.google.android.wearable.watchface.preview"
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

이 XML은 다음을 수행 합니다.

1. `android.permission.BIND_WALLPAPER` 사용 권한을 설정 합니다. 이 사용 권한은 장치에서 시스템 배경 화면을 변경할 수 있는 시계 표면 서비스 권한을 제공 합니다. 이 권한은 외부 `<service>` `<application>` 섹션이 아닌 섹션에서 설정 해야 합니다.

2. 리소스를 `watch_face` 정의 합니다. 이 리소스는 리소스를 `wallpaper` 선언 하는 짧은 XML 파일입니다 (이 파일은 다음 섹션에서 만들어짐).

3. 조사식 선택 선택 화면에 `preview` 표시 되는 라는 그릴 수 있는 이미지를 선언 합니다.

4. `intent-filter` 에는에대한`MyWatchFaceService` 정보가 포함 되어 있습니다.

기본 `WatchFace` 예제에 대 한 코드를 완료 합니다. 다음 단계는 필요한 리소스를 추가 하는 것입니다.

## <a name="add-resource-files"></a>리소스 파일 추가

Watch 서비스를 실행 하려면 먼저 **watch_face** 리소스와 미리 보기 이미지를 추가 해야 합니다. 먼저 **Resources/XML/watch_face** 에서 새 xml 파일을 만들고 해당 내용을 다음 xml로 바꿉니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

이 파일의 빌드 작업을 **Androidresource**로 설정 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![빌드 작업](creating-a-watchface-images/10-android-resource-vs.png "빌드 작업을 AndroidResource로 설정")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![빌드 작업](creating-a-watchface-images/10-android-resource-xs.png "빌드 작업을 AndroidResource로 설정")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

이 리소스 파일은 조사식에 `wallpaper` 사용 되는 간단한 요소를 정의 합니다.

아직 수행 하지 않은 경우 [preview .png](creating-a-watchface-images/preview.png)를 다운로드 합니다.
**Resources/그릴 수 있는/preview .png**에 설치 합니다. 이 파일을 `WatchFace` 프로젝트에 추가 해야 합니다. 이 미리 보기 이미지는 사용자에 게 마모 된 장치에 대 한 조사식 얼굴 선택기에 표시 됩니다. 사용자 고유의 watch에 대 한 미리 보기 이미지를 만들기 위해 실행 되는 동안에는 시계 모양의 스크린 샷을 사용할 수 있습니다. (착용 장치에서 스크린샷 받는 방법에 대 한 자세한 내용은 [스크린샷 수행](~/android/wear/deploy-test/debug-on-device.md#screenshots)을 참조 하세요.)

## <a name="try-it"></a>사용해 보세요!

앱을 빌드하여 마모 된 장치에 배포 합니다. 마모 된 앱 화면이 이전과 같이 표시 되어야 합니다. 새 조사식 얼굴을 사용 하도록 설정 하려면 다음을 수행 합니다.

1. 조사식 화면의 배경이 표시 될 때까지 오른쪽으로 살짝 밉니다.

2. 2 초 동안 화면 배경의 모든 위치를 터치 하 고 유지 합니다.

3. 다양 한 조사식 얼굴을 찾아보려면 왼쪽에서 오른쪽으로 살짝 밉니다.

4. **Xamarin 샘플** 조사식 얼굴 (오른쪽에 표시 됨)을 선택 합니다.

    [![Watchface 선택기](creating-a-watchface-images/11-watchface-picker.png "Xamarin 샘플 시청 얼굴을 찾으려면 살짝 밀기")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5. **Xamarin 샘플** 조사식 얼굴을 탭 하 여 선택 합니다.

이렇게 하면 지금까지 구현 된 사용자 지정 시청 얼굴 서비스를 사용 하도록 마모 된 장치의 조사식이 변경 됩니다.

[![디지털 시청 얼굴](creating-a-watchface-images/12-digital-watchface.png "마모 장치에서 실행 되는 사용자 지정 디지털 보기")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

이는 앱 구현이 최소화 되기 때문에 (예: 조사식 얼굴 배경을 포함 하지 않고 모양새를 개선 하기 위해 앤티앨리어스 메서드를 호출 `Paint` 하지 않음),이는 상대적으로 조잡 된 조사식입니다.
그러나 사용자 지정 조사식을 만드는 데 필요한 완전 한 뼈 기능을 구현 합니다.

다음 섹션에서이 조사식 얼굴은 보다 정교한 구현으로 업그레이드 됩니다.

## <a name="upgrading-the-watch-face"></a>Watch 얼굴 업그레이드

이 연습의 `MyWatchFaceService` 나머지 부분에서는 아날로그 스타일의 조사식을 표시 하도록 업그레이드 되 고 더 많은 기능을 지원 하도록 확장 됩니다. 업그레이드 된 watch 얼굴을 만들기 위해 다음과 같은 기능이 추가 됩니다.

1. 아날로그 시간, 분 및 초 바늘의 시간을 나타냅니다.

2. 표시 여부 변경에 반응 합니다.

3. 앰비언트 모드와 대화형 모드 간의 변경 내용에 응답 합니다.

4. 기본 마모 장치의 속성을 읽습니다.

5. 표준 시간대 변경이 발생 하는 시간을 자동으로 업데이트 합니다.

아래 코드 변경을 구현 하기 전에, 그릴 수 있는 [.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)파일을 다운로드 하 고 압축을 풀고 압축을 푼 .png 파일을 **리소스/그릴** 수 있는 .png로 이동 합니다 (이전 **preview .png**덮어쓰기). 새 .png 파일을 `WatchFace` 프로젝트에 추가 합니다.

### <a name="update-engine-features"></a>업데이트 엔진 기능

다음 단계는 아날로그 watch 얼굴을 **MyWatchFaceService.cs** 새 기능을 지 원하는 구현으로 업그레이드 하는 것입니다. **MyWatchFaceService.cs** 의 콘텐츠를 [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) 에 있는 조사식 얼굴 코드의 아날로그 버전으로 바꿉니다 .이 소스를 잘라내어 기존 **MyWatchFaceService.cs**에 붙여 넣을 수 있습니다.

이 버전의 **MyWatchFaceService.cs** 는 기존 메서드에 더 많은 코드를 추가 하 고 추가로 재정의 된 메서드를 포함 하 여 더 많은 기능을 추가 합니다. 다음 섹션에서는 소스 코드 둘러보기를 제공 합니다.

#### <a name="oncreate"></a>OnCreate

업데이트 된 **OnCreate** 메서드는 이전 처럼 조사식 얼굴 스타일을 구성 하지만 몇 가지 추가 단계를 포함 합니다.

1. **리소스/drawable-hdpi/xamarin_background**에 있는 **xamarin_background** 리소스로 배경 이미지를 설정 합니다.

2. 시 `Paint` , 분 및 초를 그리는 개체를 초기화 합니다.

3. 조사식 표면의 `Paint` 가장자리 둘레의 시간 틱을 그리기 위한 개체를 초기화 합니다.

4. 두 번째 손을 매 초 `Invalidate` 마다 다시 그리도록 (다시 그리기) 메서드를 호출 하는 타이머를 만듭니다. 이 타이머는 1 분 마다 한 `OnTimeTick` 번 `Invalidate` 만 호출 하므로 필요 합니다.

이 예제는 xamarin_background 이미지 **를** 하나만 포함 합니다. 그러나 사용자 지정 시청 얼굴에서 지원할 각 화면 밀도 마다 다른 배경 이미지를 만들 수 있습니다.

#### <a name="ondraw"></a>OnDraw

업데이트 된 **OnDraw** 메서드는 다음 단계를 사용 하 여 아날로그 스타일의 조사식 모양을 그립니다.

1. 현재 `time` 개체에서 유지 관리 되는 현재 시간을 가져옵니다.

2. 그리기 화면 및 해당 중심의 범위를 결정 합니다.

3. 배경을 그릴 때 장치에 맞게 크기를 조정 하 여 배경을 그립니다.

4. 시계의 표면 주위에 12 개의 *틱* 을 그립니다 (클록 표면의 시간에 해당).

5. 각 조사식의 각도, 회전 및 길이를 계산 합니다.

6. 각 손을 조사식 화면에 그립니다. 조사식이 앰비언트 모드인 경우 두 번째 손은 그려지지 않습니다.

#### <a name="onpropertieschanged"></a>OnPropertiesChanged

이 메서드는 마모 된 장치의 `MyWatchFaceEngine` 속성 (예: 낮은 비트 앰비언트 모드 및 굽기 방지)에 대해 알리기 위해 호출 됩니다. 에서 `MyWatchFaceEngine`이 메서드는 낮은 비트 앰비언트 모드 (하위 비트 앰비언트 모드에서는 각 색에 대해 적은 비트를 지원함)만 확인 합니다.

이 메서드에 대 한 자세한 내용은 Android [On등록 변경](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API 설명서를 참조 하세요.

#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

이 메서드는 마모 장치가 앰비언트 모드에 들어가거나 종료 될 때 호출 됩니다. `MyWatchFaceEngine` 구현에서 조사식은 앰비언트 모드일 때 앤티앨리어싱을 사용 하지 않도록 설정 합니다.

이 메서드에 대 한 자세한 내용은 Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API 설명서를 참조 하세요.

#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

이 메서드는 조사식이 표시 되거나 숨겨질 때마다 호출 됩니다. 에서 `MyWatchFaceEngine`이 메서드는 표시 상태에 따라 표준 시간대 수신기 (아래 설명)를 등록/등록 취소 합니다.

이 메서드에 대 한 자세한 내용은 Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API 설명서를 참조 하세요.

### <a name="time-zone-feature"></a>표준 시간대 기능

새 **MyWatchFaceService.cs** 에는 표준 시간대가 변경 될 때마다 (예: 표준 시간대 간에 이동 하는 동안) 현재 시간을 업데이트 하는 기능이 포함 되어 있습니다. **MyWatchFaceService.cs**의 끝 부분에서 표준 시간대 변경이 `BroadcastReceiver` 정의 되어 표준 시간대 변경 의도 개체를 처리 합니다.

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver` 및`UnregisterTimezoneReceiver`메서드는 메서드에`OnVisibilityChanged` 의해 호출 됩니다.
`UnregisterTimezoneReceiver`는 조사식 표면의 표시 상태가 숨김으로 변경 될 때 호출 됩니다. 조사식 면 `RegisterTimezoneReceiver` 이 다시 표시 되 면이 호출 됩니다 ( `OnVisibilityChanged` 메서드 참조).

엔진 `RegisterTimezoneReceiver` 메서드는이 표준 시간대 수신기의 `Receive` 이벤트에 대 한 처리기를 선언 합니다 .이 `time` 처리기는 표준 시간대가 교차할 때마다 개체를 새 시간으로 업데이트 합니다.

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

표준 시간대 수신기에 대해 의도 필터를 만들고 등록 합니다.

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

메서드 `UnregisterTimezoneReceiver` 는 표준 시간대 수신기의 등록을 취소 합니다.

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>향상 된 시청 얼굴 실행

앱을 빌드하여 마모 된 장치에 다시 배포 합니다. 이전 처럼 조사식 얼굴 선택기에서 조사식 얼굴을 선택 합니다. 조사식 선택기에서 미리 보기가 왼쪽에 표시 되 고 새 조사식 얼굴은 오른쪽에 표시 됩니다.

[![아날로그 조사식 얼굴](creating-a-watchface-images/13-analog-watchface.png "선택 및 장치에서 아날로그 얼굴 개선")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

이 스크린샷에서 두 번째 손은 초당 한 번 이동 합니다. 마모 된 장치에서이 코드를 실행 하면 조사식이 앰비언트 모드로 전환 될 때 두 번째 손가 사라집니다.

## <a name="summary"></a>요약

이 연습에서는 사용자 지정 Android 마모 1.0 watchface이 구현 되 고 테스트 되었습니다. `CanvasWatchFaceService` 및`CanvasWatchFaceService.Engine` 클래스가 도입 되었으며 엔진 클래스의 필수 메서드는 간단한 디지털 조사식을 만들기 위해 구현 되었습니다. 이 구현은 아날로그 시청 얼굴을 만들기 위한 더 많은 기능으로 업데이트 되었으며 표시 유형, 앰비언트 모드 및 장치 속성의 차이점을 처리 하기 위해 추가 메서드가 구현 되었습니다. 마지막으로 표준 시간대 브로드캐스트 수신기가 구현 되어 표준 시간대를 초과 하는 시간이 자동으로 업데이트 됩니다.

## <a name="related-links"></a>관련 링크

- [조사식 얼굴 만들기](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-watchface)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
