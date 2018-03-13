---
title: "Watch 화면 만들기"
description: "이 가이드에서는 Android 착용에 대 한 사용자 지정 조사식 얼굴 서비스를 구현 하는 방법을 설명 합니다. 더 많은 코드를 아날로그 스타일 watch 화면을 만들려면 다음을 디지털 조사식 얼굴 서비스를 훼손는 구축 하기 위한 단계별 지침이 제공 됩니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fb3a2a9e60bda2a99a719bf75d23c29d42a94bdb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-watch-face"></a>Watch 화면 만들기

_이 가이드에서는 Android 착용에 대 한 사용자 지정 조사식 얼굴 서비스를 구현 하는 방법을 설명 합니다. 더 많은 코드를 아날로그 스타일 watch 화면을 만들려면 다음을 디지털 조사식 얼굴 서비스를 훼손는 구축 하기 위한 단계별 지침이 제공 됩니다._

## <a name="overview"></a>개요 

이 연습에서는 기본 조사식 얼굴 서비스는 사용자 지정 Android 착용 watch 화면 만들기의 필수 항목을 설명 하기 위해 생성 됩니다. 초기 조사식 얼굴 서비스에는 시간 및 분 단위로 현재 시간을 표시 하는 간단한 디지털 조사식 표시 됩니다. 

[![디지털 watch 화면](creating-a-watchface-images/01-initial-face.png "초기 디지털 시계 화면의의 예제 스크린 샷")](creating-a-watchface-images/01-initial-face.png#lightbox)

이 디지털 watch 화면을 개발 하 고 테스트 한 후 더 많은 코드를 더 많이로 업그레이드 하는 정교한 아날로그 watch 화면 3 개의 자동으로 추가 됩니다. 

[![아날로그 watch 화면](creating-a-watchface-images/02-example-watchface.png "최종 아날로그 시계 화면의의 예제 스크린 샷")](creating-a-watchface-images/02-example-watchface.png#lightbox)

조사식 얼굴 서비스 번들로 제공 되며 마모 응용 프로그램의 일부로 설치 됩니다. 다음 예에서 `MainActivity` 며 포함 마모 응용 프로그램 템플릿에서 코드 조사식 얼굴 서비스를 패키지 하 고 응용 프로그램의 일부로 스마트 조사식에 배포할 수 있도록 합니다. 실제로이 앱은 디버깅 및 테스트에 대 한 조사식 얼굴 서비스 마모 장치 (또는 에뮬레이터)에 로드를 가져오기 위한 수단으로 순수 하 게 역할 합니다. 

## <a name="requirements"></a>요구 사항

조사식 얼굴 서비스를 구현 하려면 다음이 필요 합니다.

-   Android 5.0 (API 수준 21) 마모 장치 또는 에뮬레이터에 이상.

-   [Xamarin Android 착용 지원 라이브러리](https://www.nuget.org/packages/Xamarin.Android.Wear) Xamarin.Android 프로젝트에 추가 해야 합니다. 

아니지만, Android 5.0 최소 API 수준 Android 5.1 조사식 얼굴 서비스 구현에 대 한 나중 것이 좋습니다. Android Android 5.1 (API 22)를 실행 하는 장치 착용 하거나 높을수록는 장치가 절전 모드일 때 화면에 표시 되는 내용 제어 하려면 마모 앱을 허용 *앰비언트* 모드입니다. 장치 절전을 벗어날 때 *앰비언트* 모드 중인 *대화형* 모드입니다. 이러한 모드에 대 한 자세한 내용은 [유지 Your App 표시](https://developer.android.com/training/wearables/apps/always-on.html)합니다. 


## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 쓰는 유형 Android 프로젝트 만들기 **WatchFace** (새 Xamarin.Android 프로젝트 만들기에 대 한 자세한 내용은 참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-vs-sml.png "새 프로젝트 대화 상자에서 쓰는 유형 응용 프로그램 선택")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![새 프로젝트 대화 상자](creating-a-watchface-images/03-wear-project-xs-sml.png "새 프로젝트 대화 상자에서 쓰는 유형 응용 프로그램 선택")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


패키지 이름으로 설정 `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![패키지의 이름 설정을](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface에 패키지 이름을 설정")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![패키지의 이름 설정을](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface에 패키지 이름을 설정")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

또한, 아래로 스크롤하여 사용 하도록 설정 된 **인터넷** 및 **WAKE_LOCK** 사용 권한: 

[![필요한 권한](creating-a-watchface-images/05-required-permissions-vs.png "인터넷을 사용 하도록 설정 및 WAKE_LOCK 사용 권한")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

최소 Android 버전을 설정 **Android 5.1 (API 수준 22)**합니다. 또한 가능 하 게는 **인터넷** 및 **WakeLock** 사용 권한:

[![필요한 권한](creating-a-watchface-images/05-required-permissions-xs.png "인터넷을 사용 하도록 설정 및 WakeLock 사용 권한")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

다음으로 다운로드 [preview.png](creating-a-watchface-images/preview.png) &ndash; 에 추가 됩니다는 **drawables** 이 연습의 뒷부분에서 폴더입니다.


## <a name="add-the-xamarin-android-wear-package"></a>Xamarin Android 마모 패키지 추가

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

NuGet 패키지 관리자를 시작 (Visual Studio에서 마우스 오른쪽 단추로 클릭 **참조** 에 **솔루션 탐색기** 선택 **NuGet 패키지 관리...** ). 안정적인 최신 버전의 프로젝트 업데이트 **Xamarin.Android.Wear**: 

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear 패키지 추가")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

다음으로 **Xamarin.Android.Support.v13** 은 설치, 제거:

[![NuGet 패키지 관리자 제거](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 제거")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

NuGet 패키지 관리자를 시작 (Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **패키지** 에 **솔루션 창** 선택 **패키지 추가...** ). 안정적인 최신 버전의 프로젝트 업데이트 **Xamarin.Android.Wear**: 

[![NuGet 패키지 관리자 추가](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear 패키지 추가")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


빌드 및 마모 장치 또는 에뮬레이터에서 앱을 실행 (이 작업을 수행 하는 방법에 대 한 자세한 내용은 참조는 [시작](~/android/wear/get-started/index.md) 가이드). 마모 장치에서 다음 응용 프로그램 화면이 표시 되어야 합니다.

[![앱 스크린 샷을](creating-a-watchface-images/08-app-screen.png "마모 장치에 응용 프로그램 화면")](creating-a-watchface-images/08-app-screen.png#lightbox)

이 시점에서 기본 마모 앱 되므로 필요는 없습니다 조사식 얼굴 기능 아직 조사식 얼굴 서비스 구현을 제공 하지 않습니다. 이 서비스는 다음 추가 됩니다. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android 마모 구현 조사식 통해 얼굴은 `CanvasWatchFaceService` 클래스입니다. `CanvasWatchFaceService` 파생 된 `WatchFaceService`에서 파생 되는 자체 `WallpaperService` 다음 다이어그램에 나와 있는 것 처럼: 

[![상속 다이어그램](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService 상속 다이어그램")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` 중첩 된 포함 `CanvasWatchFaceService.Engine`; 인스턴스화하여는 `CanvasWatchFaceService.Engine` 시계 화면의 그리기의 실제 작업을 수행 하는 개체입니다. `CanvasWatchFaceService.Engine` 파생 된 `WallpaperService.Engine` 는 위의 다이어그램에 나와 있는 것 처럼 합니다. 

이 다이어그램에 표시 되지은 `Canvas` 하 `CanvasWatchFaceService` watch 화면을 그리는 데 사용 하 여 &ndash; 이 `Canvas` 를 통해 전달 되는 `OnDraw` 아래 설명 된 대로 메서드. 

다음 섹션에서는 사용자 정의 보기 글꼴 서비스는 다음이 단계를 수행 하 여 만들어집니다. 

1.  라는 클래스를 정의 `MyWatchFaceService` 에서 파생 된 `CanvasWatchFaceService`합니다. 

2.  내에서 `MyWatchFaceService`, 라는 중첩 된 클래스를 만들고 `MyWatchFaceEngine` 에서 파생 된 `CanvasWatchFaceService.Engine`합니다. 

3.  `MyWatchFaceService`, 구현는 `CreateEngine` 인스턴스화하는 메서드가 `MyWatchFaceEngine` 하 고 반환 합니다.

4.  `MyWatchFaceEngine`, 구현에서 `OnCreate` 조사식 면 스타일을 만들고 다른 초기화 작업을 수행 하는 메서드. 

5.  구현 된 `OnDraw` 방식의 `MyWatchFaceEngine`합니다. 이 메서드는 watch 화면을 그려야 할 때마다 (예: *무효화*). `OnDraw` 시간, 분 및 두 번째 손에 같은 조사식 얼굴 요소 그립니다 (및 다시 그립니다) 메서드입니다. 

6.  구현 된 `OnTimeTick` 방식의 `MyWatchFaceEngine`합니다. 
    `OnTimeTick` 날짜/시간 변경 될 때 또는 (앰비언트와 대화형 모드)에 1 분 마다 적어도 한 번이 하 라고 합니다. 

에 대 한 자세한 내용은 `CanvasWatchFaceService`, Android 참조 [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API 설명서입니다.
마찬가지로, [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) 시계 화면의의 실제 구현에 설명 합니다.


### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService 추가

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

라는 새 파일 추가 **MyWatchFaceService.cs** (Visual Studio에서 마우스 오른쪽 단추로 클릭 **WatchFace** 에 **솔루션 탐색기**, 클릭 **추가 > 새 항목...** 를 선택 하 고 **클래스**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

라는 새 파일 추가 **MyWatchFaceService.cs** (Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭는 **WatchFace** 프로젝트를 클릭 하 여 **추가 > 새 파일...** 를 선택 하 고 **빈 클래스**). 

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

`MyWatchFaceService` (에서 파생 된 `CanvasWatchFaceService`)은 시계 화면의 "주 프로그램"입니다. `MyWatchFaceService` 단 하나의 메서드를 구현 `OnCreateEngine`, 인스턴스화합니다을 반환 하는 `MyWatchFaceEngine` 개체 (`MyWatchFaceEngine` 에서 파생 된 `CanvasWatchFaceService.Engine`). 인스턴스화된 `MyWatchFaceEngine` 으로 개체 반환 되어야 합니다는 `WallpaperService.Engine`합니다. 캡슐화 `MyWatchFaceService` 개체 생성자에 전달 합니다. 

`MyWatchFaceEngine` 실제 조사식 얼굴 구현 &ndash; 시계 화면의 그릴 수 있는 코드가 포함 되어 있습니다. 또한 화면 변경 (앰비언트/대화형 모드를 해제 하면, 등 화면입니다.)와 같은 시스템 이벤트를 처리 합니다. 

 
### <a name="implement-the-engine-oncreate-method"></a>엔진 OnCreate 메서드 구현

`OnCreate` 메서드 시계 화면의 초기화 합니다. 다음 필드를 추가 `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

이 `Paint` 개체 watch 화면에 현재 시간을 그리는 데 사용 됩니다. 다음 메서드를 다음으로 추가 `MyWatchFaceEngine`: 

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

`OnCreate` 잠시 후 라고 `MyWatchFaceEngine` 시작 됩니다. 설정의 `WatchFaceStyle` (컨트롤 마모 장치 사용자와 상호 작용 하는 방법) 하 고 인스턴스화합니다는 `Paint` 시간을 표시 하는 사용할 개체입니다. 

에 대 한 호출 `SetWatchFaceStyle` 다음 작업을 수행 합니다.

1.  집합 *미리 보기 모드* 를 `PeekModeShort`, 때문에 알림을 표시에 카드 작은 "보기"로 표시 되도록 합니다. 

2.  배경 표시 여부 설정 하는 `Interruptive`, 표시할 간단 하 게만 있으므로 알림 나타냅니다 peek 카드의 배경을 이르게 됩니다.

3.  사용자 지정 시계 화면의 대신 시간을 표시할 수 있도록 시계 화면의 그려지는에서 기본 시스템 UI 시간을 사용 하지 않습니다.

이러한 오류 코드 및 기타 조사식 글꼴 스타일 옵션에 대 한 자세한 내용은 참조는 Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API 설명서입니다.

후 `SetWatchFaceStyle` 완료 되 면 `OnCreate` 인스턴스화하는 `Paint` 개체 (`hoursPaint`)을 흰색으로 그 텍스트 크기가 48 픽셀의 색을 설정 하 고 ([TextSize](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) 픽셀 단위로 지정 해야 합니다). 


### <a name="implement-the-engine-ondraw-method"></a>엔진 OnDraw 메서드를 구현 합니다.

`OnDraw` 메서드는 아마도 가장 중요 한 `CanvasWatchFaceService.Engine` 메서드 &ndash; 이것은 실제로 그리기 자릿수 같은 얼굴 요소 감시와 클록 얼굴 손에 메서드입니다. 다음 예제에서는 시계 화면의에 시간 문자열을 그립니다.
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

Android 호출 하는 경우 `OnDraw`, 전달 된 `Canvas` 인스턴스 및 경계 면을 그릴 수 있습니다. 위의 코드 예에서 `DateTime` 시간 동안에서 현재 시간 및 분 (12 시간 형식)으로 계산 하는 데 사용 됩니다. 결과 시간 문자열을 사용 하 여 캔버스에 그리는 `Canvas.DrawText` 메서드. 문자열 70 픽셀 위에 나타납니다 왼쪽된 가장자리와 80 픽셀에서 아래로 위쪽 가장자리에서. 

에 대 한 자세한 내용은 `OnDraw` 메서드를 Android 참조 [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) API 설명서입니다.


### <a name="implement-the-engine-ontimetick-method"></a>엔진 OnTimeTick 메서드 구현

Android 주기적으로 호출 된 `OnTimeTick` watch 화면으로 표시 된 시간을 업데이트 하는 메서드. (모두 앰비언트 모드와 대화형 모드에서), 분 당 최소 한 번 또는 날짜/시간 또는 표준 시간대 변경 된 경우에 호출 됩니다. 다음 메서드를 추가 `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

이 구현 `OnTimeTick` 단순히 호출 `Invalidate`합니다. `Invalidate` 메서드 일정 `OnDraw` watch 화면을 다시 그리게 합니다. 

에 대 한 자세한 내용은 `OnTimeTick` 메서드를 Android 참조 [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API 설명서입니다.

 
## <a name="register-the-canvaswatchfaceservice"></a>CanvasWatchFaceService 등록

`MyWatchFaceService` 에 등록 되어야 합니다는 **AndroidManifest.xml** 연결 된 마모 응용 프로그램입니다. 이렇게 하려면 다음 XML을 추가 하는 `<application>` 섹션: 

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

이 XML은 다음을 수행합니다.

1.  설정의 `android.permission.BIND_WALLPAPER` 권한. 장치에서 시스템 배경 무늬를 변경 하기 위한 조사식 얼굴 서비스 권한을 제공 하는이 사용 권한은 합니다. 이 사용 권한을 설정 해야 하는 `<service>` 섹션 아닌 외부 `<application>` 섹션. 

2.  정의 `watch_face` 리소스입니다. 이 리소스는 선언 하는 XML 파일을 짧은 `wallpaper` 리소스 (다음 섹션에서이 파일이 생성 됩니다). 

3.  라는 이미지를 그릴 수 있는 선언한 `preview` 를 조사식 선택기 선택 화면으로 표시 됩니다. 

4.  에 포함 되어는 `intent-filter` Android 알 수 있도록 `MyWatchFaceSevice` watch 화면을 표시 합니다. 

기본에 대 한 코드를 완료 하는 `WatchFace` 예제입니다. 다음 단계는 데 필요한 리소스를 추가 하는 것입니다.

 
## <a name="add-resource-files"></a>리소스 파일 추가

조사식 서비스를 실행 하기 전에 추가 해야는 **watch_face** 리소스 및 미리 보기 이미지입니다. 에 새 XML 파일을 먼저 만듭니다 **Resources/xml/watch_face.xml** 해당 내용을 다음 XML로 바꿉니다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

이 파일의 빌드 작업을 설정 **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![빌드 작업](creating-a-watchface-images/10-android-resource-vs.png "빌드 AndroidResource 하는 작업 집합")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![빌드 작업](creating-a-watchface-images/10-android-resource-xs.png "빌드 AndroidResource 하는 작업 집합")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

이 리소스 파일은 간단한 정의 `wallpaper` watch 화면에 사용 될 요소입니다. 

아직 수행 하는 경우 다운로드 [preview.png](creating-a-watchface-images/preview.png)합니다. 설치에서 **Resources/drawable/preview.png**합니다. 이 파일을 추가 해야는 `WatchFace` 프로젝트. 이 미리 보기 이미지가 마모 장치에서 조사식 글꼴 선택에서 사용자에 게 표시 됩니다. 사용자 고유의 watch 화면에 대 한 미리 보기 이미지를 만들려면 실행 되는 동안 시계 화면의의 스크린샷을 취할 수 있습니다. (스크린샷을 마모 장치에서 시작 하는 방법에 대 한 자세한 내용은 [스크린 샷을 라인](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>실습

빌드하고 마모 장치에 앱을 배포 합니다. 이전 처럼 표시 마모 응용 프로그램 화면이 표시 됩니다. 새 watch 화면을 사용 하도록 설정 하려면 다음을 수행 합니다. 

1.  조사식 화면 배경이 표시 될 때까지 오른쪽을 살짝 밉니다. 

2.  터치 하 고 2 초에 대 한 화면의 배경을 아무 곳 이나 있으면 됩니다.

3.  다양 한 조사식 얼굴을 탐색 하는 오른쪽을 왼쪽에서 살짝 밉니다. 

4.  선택 된 **Xamarin 샘플** 시청 얼굴 (오른쪽에 표시): 

    [![Watchface 선택기](creating-a-watchface-images/11-watchface-picker.png "살짝 Xamarin 샘플 watch 화면을 찾을 수")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  탭의 **Xamarin 샘플** 얼굴 선택을 시청 합니다. 

이렇게 하면 지금까지 구현 하는 사용자 정의 보기 글꼴 서비스를 사용 하는 마모 장치의 시계 화면의 변경: 

[![디지털 watch 화면](creating-a-watchface-images/12-digital-watchface.png "마모 장치에서 실행 하는 사용자 지정 디지털 조사식")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

응용 프로그램 구현도 따라서 최소 때문에 이것이 비교적 조잡 watch 화면 (예를 들어 조사식 얼굴 배경 포함 되지 않습니다 및를 호출 하지 않습니다 `Paint` 앤티앨리어싱 모양을 개선 하는 메서드). 그러나 사용자 지정 watch 화면을 만드는 데 필요한 핵심 기능을 구현지 않습니다. 

다음 섹션에서이 watch 화면 보다 정교한 구현에 업그레이드 됩니다. 

 
## <a name="upgrading-the-watch-face"></a>시계 화면의 업그레이드

이 연습의 나머지 부분에서 `MyWatchFaceService` 아날로그 스타일 watch 화면을 표시 하려면 업그레이드 되 고 더 많은 기능을 지원 하도록 확장 되었습니다. 업그레이드 된 watch 화면을 만들려면 다음과 같은 기능이 추가 됩니다. 

1.  아날로그 시간, 분 및 두 번째 포인터와 시간을 나타냅니다.

2.  표시 유형 변경에 반응합니다.

3.  앰비언트 모드와 대화형 모드 간의 변경 사항에 응답합니다. 

4.  내부 마모 장치 속성을 읽습니다.

5.  자동으로 때 시간대 변경이 발생 한 시간을 업데이트 합니다. 

아래 코드 변경 내용을 구현 하기 전에 다운로드 [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), 압축을 푼 및 압축 푼된.png 파일을 이동할 **리소스/그릴** (이전 덮어쓰기 **preview.png**). 새.png 파일을 추가 `WatchFace` 프로젝트.


### <a name="update-engine-features"></a>엔진 기능 업데이트

다음 단계는 업그레이드 **MyWatchFaceService.cs** 을 아날로그 watch 화면을 그린 다음이 새 기능을 지원 되는 구현에 있습니다. 내용을 대체 **MyWatchFaceService.cs** 의 조사식 얼굴 코드의 아날로그 버전과 [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (있습니다 수 잘라내어이 소스에 기존  **MyWatchFaceService.cs**). 

이 버전의 **MyWatchFaceService.cs** 기존 메서드를 더 많은 코드를 추가 하 고 기능을 추가 하는 추가 재정의 된 메서드가 포함 되어 있습니다. 다음 섹션에서는 다양 한 소스 코드 안내를 제공합니다.

#### <a name="oncreate"></a>OnCreate

업데이트 된 **OnCreate** 메서드 조사식 면 스타일을 이전과 마찬가지로 구성 하지만 몇 가지 추가 단계를 포함 합니다. 

1.  배경 이미지 설정 하는 **xamarin_background** 에 상주 하는 리소스 **Resources/drawable-hdpi/xamarin_background.png**합니다. 

2.  초기화 `Paint` 손 시간, 분 손 모양 및 두 번째 손 모양 그리기에 대 한 개체입니다.

3.  초기화 한 `Paint` 시계 화면의 가장자리 주위 시간 틱 그리기에 대 한 개체입니다. 

4.  호출 하는 타이머를 만듭니다는 `Invalidate` (다시 그리기) 메서드를 두 번째 손 모양 아이콘이 1 초 마다 그려야 합니다. 이 타이머 변수가 필요 하기 때문에 `OnTimeTick` 호출 `Invalidate` 매 분 마다 한 번만 합니다.

이 예제를 포함 한 **xamarin_background.png** 이미지입니다; 그러나 사용자 지정 시계 화면에서 지 원하는 각 화면 밀도 대 한 배경 이미지를 만드는 경우가 있습니다. 

#### <a name="ondraw"></a>OnDraw

업데이트 된 **OnDraw** 메서드는 아날로그 스타일 watch 화면에서 다음 단계를 사용 하 여 그립니다. 

1.  이제에 유지 관리는 현재 시간을 가져옵니다는 `time` 개체입니다. 

2.  그리기 화면 및 중심을 관통의 경계를 결정합니다.

3.  배경을 그릴 때 장치에 맞게 크기가 조정 배경을 그립니다.

4.  12 그립니다 *틱* (클록 얼굴 시간에 해당)는 클록의 면 주위 합니다. 

5.  각도, 회전 및 각 조사식 포인터에 대 한 길이 계산합니다.

6.  조사식 화면에 각 포인터를 그립니다. Note 시계 앰비언트 모드에 있으면 두 번째 손 모양 아이콘이 그리지 않습니다. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

알리기 위해이 메서드는 `MyWatchFaceEngine` 마모 장치 (예: 낮은 비트 앰비언트 모드와 보호에 대 한 진행) 속성에 대 한 합니다. `MyWatchFaceEngine`,이 메서드는만 낮은 비트 앰비언트 모드에 대 한 확인 (낮은 비트 앰비언트 모드로 화면 각 색에 대 한 비트 수가 지원). 

이 메서드에 대 한 자세한 내용은 참조는 Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API 설명서입니다.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

이 메서드는 마모 장치 들어오거나 앰비언트 모드 종료 될 때 호출 됩니다. 에 `MyWatchFaceEngine` 구현 시계 화면의 비활성화 앤티앨리어싱 앰비언트 모드일 때. 

이 메서드에 대 한 자세한 내용은 참조는 Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API 설명서입니다.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

표시 / 숨김이 메서드는 시계 될 때마다 호출 됩니다. `MyWatchFaceEngine`,이 메서드가 등록/등록 취소 (아래 설명 참조)의 표시 여부 상태에 따라 시간대 수신기입니다. 

이 메서드에 대 한 자세한 내용은 참조는 Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API 설명서입니다.

 
### <a name="time-zone-feature"></a>표준 시간대 기능

새 **MyWatchFaceService.cs** 시간 영역 (예: 표준 시간대 간에 이동 하는 동안) 변경 될 때마다 현재 시간을 업데이트 하는 기능 포함 합니다. 끝 부분 **MyWatchFaceService.cs**, 영역 변경 시간 `BroadcastReceiver` 정의 된 표준 시간대 변경 의도 개체를 처리 하는: 

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

`RegisterTimezoneReceiver` 및 `UnregisterTimezoneReceiver` 메서드 호출의 `OnVisibilityChanged` 메서드. 
`UnregisterTimezoneReceiver` 시계 화면의의 표시 여부 상태를 변경 되 면 숨겨진 호출 됩니다. Watch 화면 다시 표시 되 면 `RegisterTimezoneReceiver` 라고 (참조는 `OnVisibilityChanged` 메서드). 

엔진은 `RegisterTimezoneReceiver` 이 표준 시간대 수신기에 대 한 처리기를 선언 하는 메서드 `Receive` 이벤트;이 처리기를 업데이트는 `time` 시간대 교차할 때마다 새 시간을 사용 하는 개체: 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

의도 필터 만들어지고 시간대 수신기에 대 한 등록:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` 메서드 시간대 수신기의 등록을 취소 합니다.

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>향상 된 시계 화면의 실행

빌드하고 마모 장치에 앱을 다시 배포 합니다. 앞으로 조사식 글꼴 선택에서 시계 화면의 선택 합니다. 왼쪽에 표시 되는 감시 선택에서 미리 보기 및 새 시계 화면의 오른쪽에 표시 됩니다.

[![아날로그 watch 화면](creating-a-watchface-images/13-analog-watchface.png "아날로그 글꼴 선택에서와 장치에 향상")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

이 화면에 두 번째 손 모양 아이콘이 초당 한 번 이동 됩니다. 마모 장치에서이 코드를 실행 하면 시계 앰비언트 모드로 설정 되 면 두 번째 손 모양 아이콘이 사라집니다.

 
## <a name="summary"></a>요약

이 연습에서는 사용자 지정 Android 착용 watchface 구현 하 고 테스트 되었습니다. `CanvasWatchFaceService` 및 `CanvasWatchFaceService.Engine` 클래스를 도입 되었으며 엔진 클래스의 필수 메서드 간단한 디지털 watch 화면을 만들기 위해 구현 되었습니다. 이 구현은는 아날로그 watch 화면을 만들려면 더 많은 기능으로 업데이트 하 고 추가 방법 표시 여부, 앰비언트 모드 및 장치 속성의 차이에 변경 내용을 처리 하도록 구현 된 합니다. 마지막으로, 표준 시간대 브로드캐스트 수신기 시계는 시간 표준 시간대 교차할 때를 자동으로 업데이트 되도록 구현 되었습니다. 


## <a name="related-links"></a>관련 링크

- [조사식 면 만들기](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace 샘플](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
