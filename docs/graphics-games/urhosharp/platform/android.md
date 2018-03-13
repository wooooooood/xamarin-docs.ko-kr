---
title: "UrhoSharp Android 지원"
description: "Android 특정 설정 및 UrhoSharp에 대 한 기능입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: f99b8d2d9f779bc0cf14d76c110d9769ec49ad53
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android 지원

_Android 특정 설정 및 기능_

플랫폼 특정 기능을 활용 하려는 Urho 이식 가능한 클래스 라이브러리는 고 게임 논리를 다양 한 플랫폼 전반에 걸쳐 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에서 한 경우에 따라, 장치 .

아래의 페이지 가정 `MyGame` 의 서브 클래스는 `Application` 클래스입니다.

## <a name="architectures"></a>아키텍처

**지원 되는 아키텍처**: x86, armeabi, armeabi v7a

## <a name="create-a-project"></a>프로젝트 만들기

Android 프로젝트를 만들고 UrhoSharp NuGet 패키지를 추가 합니다.

추가 하는 자산을 포함 하는 데이터는 **자산** 모든 파일은 디렉터리 및 있는지 확인 하십시오 **AndroidAsset** 로 **빌드 작업**합니다.

![설치 프로젝트](android-images/image-3.png "자산 디렉터리에 자산을 포함 하는 추가 데이터")

## <a name="configure-and-launching-urho"></a>구성 및 Urho 시작

에 대 한 문을 사용 하 여 추가 `Urho` 및 `Urho.Android` 네임 스페이스를 응용 프로그램을 시작 뿐만 아니라 Urho, 초기화에 대 한이 코드를 추가 합니다.

호출 하는 가장 간단한 방법은 MyGame 클래스에서 구현 되는 게임을 실행 하는 것

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

콘텐츠로 게임 전체 화면 활동을 열립니다.

## <a name="custom-embedding-of-urho"></a>Urho의 사용자 지정 포함

있습니다 수 또는를 가지도록 Urho을 전체 응용 프로그램 화면 하 고 응용 프로그램의 구성 요소를 사용 하 여 만들 수 있습니다는 `SurfaceView` 통해:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

몇 가지 이벤트에서 전달 하면 작업 하도록 UrhoSharp, 해야 예::

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

에 대 한 동일한 작업을 수행 해야 합니다: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` 및 `OnWindowFocusChanged`합니다.

게임을 시작 하는 일반적인 작업을 보여 줍니다.

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

