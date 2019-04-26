---
title: UrhoSharp Android 지원
description: 이 문서에는 Android 관련 설정 및 기능 관련 정보 UrhoSharp 설명합니다. 지원 되는 아키텍처에 설명 하므로 특히 구성과 Urho, 및 Urho 포함 사용자 지정 시작 프로젝트를 만드는 방법.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: e7371fa85fd5955e9a0fd285adb32844001821b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61285177"
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android 지원

_Android 특정 설정 및 기능_

플랫폼 특정 기능을 활용 하려는 동안 Urho 이식 가능한 클래스 라이브러리 이며, 게임 논리에 대 한 다양 한 플랫폼에서 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에 일부 경우, .

페이지 아래에서 가정 `MyGame` 의 서브 클래스는 `Application` 클래스.

## <a name="architectures"></a>아키텍처

**지원 되는 아키텍처**: x86, armeabi, armeabi-v7a

## <a name="create-a-project"></a>프로젝트 만들기

Android 프로젝트를 만들고 UrhoSharp NuGet 패키지를 추가 합니다.

자산에 포함 된 데이터를 추가 합니다 **자산** 디렉터리 및 해야 모든 파일에 **AndroidAsset** 으로 **빌드 작업**합니다.

![설치 프로젝트](android-images/image-3.png "자산 디렉터리에 자산을 포함 하는 추가 데이터")

## <a name="configure-and-launching-urho"></a>구성 하 고 Urho 시작

문을 사용 하 여 추가 합니다 `Urho` 및 `Urho.Android` 네임 스페이스를 응용 프로그램을 시작할 수 있을 뿐만 아니라 Urho, 초기화에 대 한이 코드를 추가 합니다.

MyGame 클래스에서 구현 되는 게임을 실행 하는 가장 간단한 방법은 호출 하는 것

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

콘텐츠도 게임을 사용 하 여 전체 화면 활동을 열립니다.

## <a name="custom-embedding-of-urho"></a>사용자 지정 Urho 포함

있습니다 수 또는 것 Urho 전체 응용 프로그램 화면에서 가져와 사용 하려면 응용 프로그램의 구성 요소로 만들 수 있습니다는 `SurfaceView` 통해:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

몇 가지 이벤트에서 작업에 전달 UrhoSharp, 해야 예:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

에 대 한 동일한 작업을 수행 해야 합니다: `OnResume`, `OnPause`를 `OnLowMemory`, `OnDestroy`에 `DispatchKeyEvent` 및 `OnWindowFocusChanged`합니다.

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

