---
title: Android Wear 1.0에 대 한 시계 모드 만들기
description: 이 가이드에서는 Android Wear 1.0에 대 한 사용자 지정 조사 얼굴 서비스를 구현 하는 방법을 설명 합니다. 단지는 아날로그 방식의 watch 화면을 만드는 코드 뒤에 디지털 조사식 얼굴 서비스를 빌드하기 위한 단계별 지침이 제공 됩니다.
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/23/2018
ms.openlocfilehash: f46e0d27a39c7734e63bf5603ef2e47cd5fa7aa9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105534"
---
# <a name="creating-a-watch-face"></a>시계 모드 만들기

_이 가이드에서는 Android Wear 1.0에 대 한 사용자 지정 조사 얼굴 서비스를 구현 하는 방법을 설명 합니다. 단지는 아날로그 방식의 watch 화면을 만드는 코드 뒤에 디지털 조사식 얼굴 서비스를 빌드하기 위한 단계별 지침이 제공 됩니다._

## <a name="overview"></a>개요

이 연습에서는 기본 조사식 얼굴 서비스는 사용자 지정 Android Wear 1.0 시계 모드 만들기의 필수 요소를 설명 하기 위해 생성 됩니다.
초기 조사식 얼굴 서비스에는 현재 시간 및 분 단위로 표시 하는 간단한 디지털 조사식을 표시 합니다.

[![디지털 watch 화면의](creating-a-watchface-images/01-initial-face.png "초기 디지털 시계 화면의 스크린샷 예제")](creating-a-watchface-images/01-initial-face.png#lightbox)

이 디지털 watch 화면의 개발 하 고 테스트를 거친 후으로 업그레이드 하는 보다 정교한 아날로그 watch 화면의 세 가지 실습을 사용 하 여 코드를 더 추가 됩니다.

[![아날로그 watch 화면의](creating-a-watchface-images/02-example-watchface.png "최종 아날로그 시계 화면의 스크린샷 예제")](creating-a-watchface-images/02-example-watchface.png#lightbox)

조사식 얼굴 서비스 번들로 제공 되며 1.0 Wear 앱의 일부로 설치 됩니다. 다음 예제에서는 `MainActivity` 조사식 얼굴 서비스를 패키지 하 고 앱의 일부로 스마트 워치를 배포할 수 있도록 1.0 Wear 앱 템플릿에서 코드 이상의 아무 것도 포함 되어 있습니다. 사실이 앱은 디버깅 및 테스트에 대 한 조사식 얼굴 서비스 1.0 Wear 장치 (또는 에뮬레이터)를 로드 하기 위한 수단으로 순수 하 게 사용 됩니다.

## <a name="requirements"></a>요구 사항

조사식 얼굴 서비스를 구현 하려면 다음이 필요 합니다.

-   Android 5.0 (API 수준 21) Wear 장치 또는 에뮬레이터에서 이상.

-   합니다 [Xamarin Android Wear 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) Xamarin.Android 프로젝트에 추가 해야 합니다.

하지만 Android 5.0 최소 API 수준 Android 5.1 조사식 얼굴 서비스를 구현 하는 것에 대 한 되었거나 나중 것이 좋습니다. Android Wear (API 22) Android 5.1을 실행 하는 장치 또는 더 높은 장치 성능이 낮은 되는 동안 화면에 표시 되는 항목을 제어 하려면 Wear 앱을 허용 *앰비언트* 모드입니다. 장치가 절전을 벗어날 때 *앰비언트* 모드에 있기 *대화형* 모드입니다. 이러한 모드에 대 한 자세한 내용은 참조 하세요 [유지 Your App 표시](https://developer.android.com/training/wearables/apps/always-on.html)합니다.


## <a name="start-an-app-project"></a>앱 프로젝트를 시작 합니다.

라는 새 Android Wear 1.0 프로젝트를 만듭니다 **WatchFace** (새 Xamarin.Android 프로젝트를 만드는 방법에 대 한 자세한 내용은 참조 하세요. [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-vs-sml.png "새 프로젝트 대화 상자에서 Wear 앱 선택")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-xs-sml.png "새 프로젝트 대화 상자에서 Wear 앱 선택")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


패키지 이름으로 설정할 `com.xamarin.watchface`:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![패키지 이름 설정](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface로 패키지 이름 설정")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![패키지 이름 설정](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface로 패키지 이름 설정")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

또한 아래로 스크롤하여 사용 하도록 설정 합니다 **인터넷** 하 고 **WAKE_LOCK** 권한:

[![필요한 권한](creating-a-watchface-images/05-required-permissions-vs.png "WAKE_LOCK 권한과 인터넷을 사용 하도록 설정")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

최소 Android 버전을 설정 합니다 **Android 5.1 (API 수준 22)** 합니다.
또한를 사용 하도록 설정 합니다 **인터넷** 하 고 **WakeLock** 권한:

[![필요한 권한](creating-a-watchface-images/05-required-permissions-xs.png "WakeLock 권한과 인터넷을 사용 하도록 설정")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

그런 다음 다운로드 [preview.png](creating-a-watchface-images/preview.png) &ndash; 에 추가 됩니다 합니다 **드로어 블** 이 연습의 뒷부분에서 폴더입니다.


## <a name="add-the-xamarinandroid-wear-package"></a>Xamarin.Android Wear 패키지 추가

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

NuGet 패키지 관리자를 시작 (Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조가** 에 **솔루션 탐색기** 선택한 **NuGet 패키지 관리...** ). 프로젝트의 안정적인 최신 버전으로 업데이트 **Xamarin.Android.Wear**:

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear 패키지를 추가 합니다.")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

다음으로 **Xamarin.Android.Support.v13** 는 설치를 제거 합니다.

[![NuGet 패키지 관리자 제거](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 제거")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

NuGet 패키지 관리자를 시작 (Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지** 에 **솔루션 창** 선택한 **패키지 추가...** ). 프로젝트의 안정적인 최신 버전으로 업데이트 **Xamarin.Android.Wear**:

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear 패키지를 추가 합니다.")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


빌드 및 Wear 장치 또는 에뮬레이터에서 앱을 실행 (이 작업을 수행 하는 방법에 대 한 자세한 내용은 참조는 [Getting Started](~/android/wear/get-started/index.md) 가이드). Wear 장치에서 다음 앱 화면이 표시 됩니다.

[![앱 스크린 샷을](creating-a-watchface-images/08-app-screen.png "Wear 장치에서 앱 화면")](creating-a-watchface-images/08-app-screen.png#lightbox)

이 시점에서 기본 Wear 앱이 없습니다 조사식 얼굴 기능 조사식 얼굴 서비스 구현 아직 제공 하지 않습니다. 이 서비스는 다음 추가 됩니다.


## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android Wear 구현 보기를 통해 얼굴을 `CanvasWatchFaceService` 클래스입니다. `CanvasWatchFaceService` 파생 됩니다 `WatchFaceService`에서 파생 되는 자체 `WallpaperService` 다음 다이어그램에 표시 된 대로:

[![상속 다이어그램](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 상속 다이어그램")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` 중첩 된 포함 `CanvasWatchFaceService.Engine`; 인스턴스화할는 `CanvasWatchFaceService.Engine` 시계 화면의 그리기의 실제 작업을 수행 하는 개체입니다. `CanvasWatchFaceService.Engine` 파생 된 `WallpaperService.Engine` 위의 다이어그램에 표시 된 대로 합니다.

이 다이어그램에 표시 되지 않습니다는 `Canvas` 하는 `CanvasWatchFaceService` watch 화면을 그리는 데 사용 하 여 &ndash; 이 `Canvas` 를 통해 전달 됩니다는 `OnDraw` 아래 설명 된 대로 메서드.

다음 섹션에서는 사용자 지정 조사 얼굴 서비스는 다음이 단계를 수행 하 여 만들어집니다.

1.  라는 클래스를 정의할 `MyWatchFaceService` 에서 파생 된 `CanvasWatchFaceService`합니다.

2.  내 `MyWatchFaceService`를 호출 하는 중첩 된 클래스를 만듭니다 `MyWatchFaceEngine` 에서 파생 된 `CanvasWatchFaceService.Engine`합니다.

3.  `MyWatchFaceService`를 구현를 `CreateEngine` 인스턴스화하는 메서드가 `MyWatchFaceEngine` 반환 합니다.

4.  `MyWatchFaceEngine`를 구현 합니다 `OnCreate` 조사식 글꼴 스타일을 만들고 다른 초기화 작업을 수행 하는 메서드.

5.  구현 된 `OnDraw` 메서드의 `MyWatchFaceEngine`합니다. 이 메서드는 시계 화면의 다시 그려야 할 때마다 호출 됩니다 (즉 *무효화*). `OnDraw` 즉 시간, 분 및 두 번째 바늘 같은 조사식 얼굴 요소를 그립니다 (및 다시 그립니다) 방법이입니다.

6.  구현 된 `OnTimeTick` 메서드의 `MyWatchFaceEngine`합니다.
    `OnTimeTick` 분 (앰비언트 및 대화형 모드)에서 또는 날짜/시간 변경 되 면 당 최소 한 번이 하 라고 합니다.

에 대 한 자세한 내용은 `CanvasWatchFaceService`, Android를 참조 하세요 [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API 설명서입니다.
마찬가지로 [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) 시계 화면의의 실제 구현에 설명 합니다.


### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService 추가

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

이라는 새 파일 추가 **MyWatchFaceService.cs** (Visual Studio에서 마우스 오른쪽 단추로 클릭 **WatchFace** 에 **솔루션 탐색기**, 클릭 **추가 > 새 항목...** 를 선택 하 고 **클래스**).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

이라는 새 파일 추가 **MyWatchFaceService.cs** (Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 합니다 **WatchFace** 프로젝트 **추가 > 새 파일...** 를 선택 하 고 **빈 클래스**).

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

`MyWatchFaceService` (에서 파생 된 `CanvasWatchFaceService`)은 시계 화면의 "기본 프로그램"입니다. `MyWatchFaceService` 하나의 메서드만 구현 `OnCreateEngine`를 인스턴스화하고 반환 하는 `MyWatchFaceEngine` 개체 (`MyWatchFaceEngine` 에서 파생 된 `CanvasWatchFaceService.Engine`). 인스턴스화된 `MyWatchFaceEngine` 으로 개체를 반환 되어야 합니다는 `WallpaperService.Engine`합니다. 캡슐화 `MyWatchFaceService` 개체 생성자에 전달 합니다.

`MyWatchFaceEngine` 실제 조사식 얼굴 구현은 &ndash; watch 화면을 그리는 코드를 포함 합니다. 또한 화면 변경 (앰비언트/대화형 모드를 해제 하면, 화면입니다.)와 같은 시스템 이벤트를 처리 합니다.


### <a name="implement-the-engine-oncreate-method"></a>엔진 OnCreate 메서드에 구현 합니다.

`OnCreate` 메서드 시계 화면의 초기화 합니다. 다음 필드를 추가 `MyWatchFaceEngine`:

```csharp
Paint hoursPaint;
```

이 `Paint` watch 화면에 현재 시간을 그릴 개체를 사용 합니다. 다음 메서드를 다음으로, 추가 `MyWatchFaceEngine`:

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

`OnCreate` 잠시 후 라고 `MyWatchFaceEngine` 시작 됩니다. 설정 합니다 `WatchFaceStyle` (제어 Wear 장치 사용자와 상호 작용 하는 방법) 하 고 인스턴스화합니다는 `Paint` 시간을 표시 하는 데 사용할 개체입니다.

에 대 한 호출 `SetWatchFaceStyle` 다음을 수행 합니다.

1.  집합 *미리 보기 모드* 에 `PeekModeShort`, 알림이 화면의 작은 "보기" 카드로 표시 되도록 하면 됩니다.

2.  배경 표시 유형을 설정 `Interruptive`, 표시할 아주 잠시 동안만 중단 알림을 나타내는 경우 peek 카드의 백그라운드에 이르게 합니다.

3.  사용자 지정 조사 발생 하는 대신 시간을 표시할 수 있도록 watch 화면에 그려지는에서 기본 시스템 UI 시간을 사용 하지 않도록 설정 합니다.

이러한 및 기타 조사식 얼굴 스타일 옵션에 대 한 자세한 내용은 Android을 참조 하세요 [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API 설명서입니다.

후 `SetWatchFaceStyle` 완료 되 면 `OnCreate` 인스턴스화하는 `Paint` 개체 (`hoursPaint`)을 흰색으로 텍스트 크기로 48 픽셀의 색을 설정 하 고 ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) 픽셀로 지정 해야 합니다).


### <a name="implement-the-engine-ondraw-method"></a>엔진 OnDraw 메서드를 구현 합니다.

합니다 `OnDraw` 메서드는 가장 중요 한 것 `CanvasWatchFaceService.Engine` 메서드 &ndash; 메서드는 실제로 그립니다 숫자 등의 얼굴 요소 보기와 클록 얼굴 손에 것입니다.
다음 예제에서는 시계 화면의에 시간 문자열을 그립니다.
다음 메서드를 추가 `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str,
        (float)(frame.Left + 70),
        (float)(frame.Top  + 80), hoursPaint);
}
```

Android 호출 하는 경우 `OnDraw`, 전달는 `Canvas` 인스턴스 및 얼굴을 그릴 수 있는 범위입니다. 위의 코드 예에서 `DateTime` 시간이 현재 시간 및 분 (12 시간 형식)의 계산에 사용 됩니다. 결과 시간 문자열을 사용 하 여 캔버스에 그리는 `Canvas.DrawText` 메서드. 문자열 70 픽셀 위에 나타납니다 왼쪽된 가장자리에서 80 픽셀 아래로 위쪽 가장자리에서.

에 대 한 자세한 내용은 합니다 `OnDraw` 메서드를 Android 참조 [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine#ondraw) API 설명서입니다.


### <a name="implement-the-engine-ontimetick-method"></a>엔진 OnTimeTick 메서드 구현

Android에서 주기적으로 호출 된 `OnTimeTick` watch 화면에 표시 된 시간을 업데이트 하는 방법입니다. (모두 앰비언트 모드와 대화형 모드에서), 분당 최소한 한 번 또는 날짜/시간 또는 표준 시간대 변경 된 경우에 호출 됩니다. 다음 메서드를 추가 `MyWatchFaceEngine`:

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

이 구현의 `OnTimeTick` 호출 하기만 하면 `Invalidate`합니다. 합니다 `Invalidate` 메서드 일정 `OnDraw` watch 화면을 다시 그리게 합니다.

에 대 한 자세한 내용은 합니다 `OnTimeTick` 메서드를 Android 참조 [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API 설명서입니다.


## <a name="register-the-canvaswatchfaceservice"></a>등록 된 CanvasWatchFaceService

`MyWatchFaceService` 에 등록 해야 합니다 **AndroidManifest.xml** 연결 Wear 앱. 이렇게 하려면 다음 XML을 추가 하 여 `<application>` 섹션:

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

1.  집합의 `android.permission.BIND_WALLPAPER` 권한. 이 권한은 장치에서 시스템 배경 화면을 변경 하려면 조사식 얼굴 서비스 권한을 부여 합니다. 이 사용 권한을 설정 해야 하는 참고 합니다 `<service>` 섹션 아닌 외부 `<application>` 섹션입니다.

2.  정의 `watch_face` 리소스입니다. 이 리소스는 선언 하는 간단한 XML 파일을 `wallpaper` 리소스 (다음 섹션에서이 파일이 생성 됩니다).

3.  라는 이미지를 그릴 수 있는 선언 `preview` 는 조사식 선택기 선택 화면이 표시 됩니다.

4.  포함을 `intent-filter` 사실을 Android 있도록 `MyWatchFaceSevice` 시계 모드를 표시 합니다.

기본 코드를 완료 하는 `WatchFace` 예제입니다. 다음 단계는 데 필요한 리소스를 추가 하는 것입니다.


## <a name="add-resource-files"></a>리소스 파일 추가

조사식 서비스를 실행 하려면 먼저 추가 해야 합니다 **watch_face** 리소스 및 미리 보기 이미지입니다. 새 XML 파일을 먼저 만듭니다 **Resources/xml/watch_face.xml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

이 파일의 빌드 작업을 설정 **AndroidResource**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![빌드 작업](creating-a-watchface-images/10-android-resource-vs.png "AndroidResource 작업 빌드 설정")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![빌드 작업](creating-a-watchface-images/10-android-resource-xs.png "AndroidResource 작업 빌드 설정")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

이 리소스 파일은 간단한 정의 `wallpaper` watch 화면에 대 한 사용 될 요소입니다.

아직 수행 하는 경우 다운로드할 [preview.png](creating-a-watchface-images/preview.png)합니다.
설치할 **Resources/drawable/preview.png**합니다. 이 파일을 추가 해야 합니다 `WatchFace` 프로젝트입니다. 이 미리 보기 이미지 조사식 얼굴 선택기에서 Wear 장치에서 사용자에 게 표시 됩니다. 사용자 고유의 watch 화면에 대 한 미리 보기 이미지를 만들려면 실행 중일 때 시계 화면의 스크린샷 작업을 수행할 수 있습니다. (한 스크린샷을 Wear 장치에서 시작 하는 방법에 대 한 자세한 내용은 [스크린샷을 만들](~/android/wear/deploy-test/debug-on-device.md#screenshots)).


## <a name="try-it"></a>지금 사용해 보세요!

빌드하고 Wear 장치에 앱을 배포 합니다. 이전과 마찬가지로 표시 Wear 앱 화면이 표시 됩니다. 새 watch 화면을 사용 하도록 설정 하려면 다음을 수행 합니다.

1.  Watch 화면의 배경 표시 될 때까지 오른쪽으로 살짝 밉니다.

2.  터치 및 2 초 동안 화면의 배경을 아무 곳 이나 저장 합니다.

3.  다양 한 조사식 얼굴을 검색할 오른쪽을 왼쪽에서 살짝 밉니다.

4.  선택 된 **Xamarin 샘플** 시계 페이스 (오른쪽에 표시 됨):

    [![Watchface 선택기](creating-a-watchface-images/11-watchface-picker.png "Xamarin 샘플 watch 화면을 찾으려고 살짝 밀기")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  탭의 **Xamarin 샘플** 시계 페이스를 선택 합니다.

지금까지 구현 하는 사용자 지정 조사 얼굴 서비스를 사용 하 여 Wear 장치의 watch 화면 변경:

[![디지털 watch 화면의](creating-a-watchface-images/12-digital-watchface.png "Wear 장치에서 실행 되는 사용자 지정 디지털 시청")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

앱 구현은 하므로 최소 비교적 조잡 시계 모드 이므로 (예를 들어, 조사식 얼굴 배경이 포함 되지 않습니다 하 고 호출 하지 않으면 `Paint` 앤티앨리어싱 모양을 개선 방법).
그러나 사용자 지정 watch 화면을 만드는 데 필요한 핵심 기능을 구현지 않습니다.

다음 섹션에서는이 watch 화면 보다 복잡 한 구현에 업그레이드 됩니다.


## <a name="upgrading-the-watch-face"></a>시계 화면의 업그레이드

이 연습의 나머지 부분에서는 `MyWatchFaceService` 는 아날로그 방식의 watch 화면 표시로 업그레이드 되 고 더 많은 기능을 지원 하도록 확장 되었습니다. 업그레이드 된 watch 화면을 만들려면 다음과 같은 기능이 추가 됩니다.

1.  아날로그 시간, 분 및 두 번째 실습을 사용 하 여 시간을 나타냅니다.

2.  표시 유형 변경에 반응합니다.

3.  앰비언트 모드와 대화형 모드 간에 변경 내용에 응답합니다.

4.  기본 Wear 장치 속성을 읽습니다.

5.  표준 시간대 변경 위치를 사용 하는 경우에 자동으로 업데이트 합니다.

아래 코드 변경 내용의 구현 하기 전에 다운로드 [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true)압축을 고 압축 푼된.png 파일을 이동할 **리소스/drawable** (이전 덮어쓰기 **preview.png**). 새.png 파일을 추가 합니다 `WatchFace` 프로젝트입니다.


### <a name="update-engine-features"></a>엔진 기능 업데이트

다음 단계 업그레이드가 **MyWatchFaceService.cs** 아날로그 watch 화면을 그리고 새로운 기능을 지원 되는 구현에 있습니다. 내용을 바꿉니다 **MyWatchFaceService.cs** 조사식 얼굴 코드의 아날로그 버전과 [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (잘라내어 기존이 소스를 붙여 넣을 수 있습니다  **MyWatchFaceService.cs**).

이 버전의 **MyWatchFaceService.cs** 기존 메서드를 더 많은 코드를 추가 하 고 더 많은 기능을 추가 하려면 추가 재정의 된 메서드를 포함 합니다. 다음 섹션에서는 소스 코드의 둘러보기를 제공합니다.

#### <a name="oncreate"></a>OnCreate

업데이트 된 **OnCreate** 메서드 조사식 글꼴 스타일을 이전과 마찬가지로 구성 하지만 몇 가지 추가 단계가 포함 됩니다.

1.  배경 이미지를 설정 합니다 **xamarin_background** 에 있는 리소스 **Resources/drawable-hdpi/xamarin_background.png**합니다.

2.  초기화 `Paint` 직접 시간, 분 한편 및 초침 그리기에 대 한 개체입니다.

3.  초기화는 `Paint` 시계 화면의 가장자리 주위 시간 틱 그리기에 대 한 개체입니다.

4.  호출 하는 타이머를 만듭니다는 `Invalidate` (다시 그리기) 메서드 초침 그릴 매초 되도록 합니다. 이 타이머 매개 변수가 필요 하므로 `OnTimeTick` 호출 `Invalidate` 1 분 마다 한 번만 합니다.

이 예제에서는 하나만 포함 되어 있습니다 **xamarin_background.png** 이미지; 그러나 수 하려는 사용자 지정 시계 화면에서 지원 되는 각 화면 밀도 대 한 다른 배경 이미지를 만듭니다.

#### <a name="ondraw"></a>OnDraw

업데이트 된 **OnDraw** 메서드는 아날로그 방식의 watch 화면의 다음 단계를 사용 하 여 그립니다.

1.  이제에서 유지 관리는 현재 시간을 가져옵니다는 `time` 개체입니다.

2.  그리기 화면 및 중심의 범위를 결정합니다.

3.  배경을 그릴 때 장치에 맞게 크기가 조정, 배경을 그립니다.

4.  12 그립니다 *틱* (시계 앞면에 시간에 해당) 시계 앞면 해결 합니다.

5.  각도, 회전 및 각 조사식 포인터에 대 한 길이 계산합니다.

6.  Watch 화면의 각 포인터를 그립니다. 시계 앰비언트 모드인 경우 초침 그리지 않습니다 note 합니다.


#### <a name="onpropertieschanged"></a>OnPropertiesChanged

이 메서드를 호출 하 게 `MyWatchFaceEngine` Wear 장치 (예: 낮은 비트 앰비언트 모드와 보호 굽기)의 속성에 대 한 합니다. `MyWatchFaceEngine`,이 메서드는만 낮은 비트 앰비언트 모드에 대 한 확인 (낮은 비트 앰비언트 모드로 화면 각 색에 대 한 비트 수가 지원).

이 메서드에 대 한 자세한 내용은 참조는 Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API 설명서입니다.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

이 메서드는 Wear 장치에 들어가거나 앰비언트 모드를 종료 되 면 호출 됩니다. 에 `MyWatchFaceEngine` 구현 시계 화면의 사용 하지 않도록 설정 앤티 앨리어싱을 앰비언트 모드일 때.

이 메서드에 대 한 자세한 내용은 참조는 Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API 설명서입니다.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

표시 또는 숨김이 메서드는 시계 될 때마다 호출 됩니다. `MyWatchFaceEngine`,이 메서드가 등록/등록 취소 (아래 설명 참조) 표시 여부 상태에 따라 표준 시간대 수신자입니다.

이 메서드에 대 한 자세한 내용은 참조는 Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API 설명서입니다.


### <a name="time-zone-feature"></a>표준 시간대 기능

새 **MyWatchFaceService.cs** 시간 영역 (예: 표준 시간대 간에 이동 하는 동안) 변경 될 때마다 현재 시간을 업데이트 하는 기능을 포함 합니다. 끝 **MyWatchFaceService.cs**영역 변경 시간 `BroadcastReceiver` 정의 된 표준 시간대 변경 텐트 개체를 처리 하는:

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

`RegisterTimezoneReceiver` 하 고 `UnregisterTimezoneReceiver` 메서드 호출을 `OnVisibilityChanged` 메서드.
`UnregisterTimezoneReceiver` 시계 화면의의 표시 여부 상태를 변경 된 경우 숨겨진 호출 됩니다. Watch 화면 다시 표시 되 면 `RegisterTimezoneReceiver` 호출 됩니다 (참조는 `OnVisibilityChanged` 메서드).

엔진 `RegisterTimezoneReceiver` 메서드가 시간대 받는 사람에 대 한 처리기를 선언 `Receive` 이벤트;이 처리기를 업데이트 합니다 `time` 시간대 교차할 때마다 새 시간을 사용 하 여 개체:

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

의도 필터를 만들고 표준 시간대 수신자에 게 등록:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` 메서드 시간대 수신기를 등록 취소 합니다.

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>향상 된 시계 화면의 실행

빌드하고 다시 Wear 장치에 앱을 배포 합니다. 시계 화면의 앞으로 조사식 얼굴 선택에서 선택 합니다. 조사식 선택에서 미리 보기는 왼쪽에 표시 됩니다 하 고 새 watch 화면 오른쪽에 표시 됩니다.

[![아날로그 watch 화면의](creating-a-watchface-images/13-analog-watchface.png "아날로그 얼굴 장치 선택기에 향상")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

이 스크린샷에서 초침 초당 한 번 이동 됩니다. Wear 장치에서이 코드를 실행할 때 시계 앰비언트 모드로 들어가면 초침 사라집니다.


## <a name="summary"></a>요약

이 연습에서는 사용자 지정 Android Wear 1.0 watchface는 구현 하 고 테스트 되었습니다. 합니다 `CanvasWatchFaceService` 및 `CanvasWatchFaceService.Engine` 클래스를 도입 되었으며 엔진 클래스의 필수 메서드는 간단한 디지털 시계 모드를 만들기 위해 구현 되었습니다. 이 구현이 아날로그 watch 화면을 만들려면 많은 기능을 사용 하 여 업데이트 된 및 가시성, 앰비언트 모드 및 장치 속성의 차이에 변경 내용을 처리 하도록 추가 메서드 구현 되었습니다. 마지막으로, 표준 시간대 브로드캐스트 수신기 시계 시간 표준 시간대를 교차할 때를 자동으로 업데이트 되도록 구현 되었습니다.


## <a name="related-links"></a>관련 링크

- [조사식 얼굴 만들기](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace 샘플](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
