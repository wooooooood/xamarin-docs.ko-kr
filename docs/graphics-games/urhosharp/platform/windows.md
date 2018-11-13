---
title: UrhoSharp Windows 지원
description: 이 문서에서는 UrhoSharp에 대 한 Windows 지원을 설명 합니다. 프로젝트 만들기, 구성 및 Urho 시작, WPF와 통합 및 UWP와 통합 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 471029375d8a61a6c48d94a66d7836807e0da22f
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526808"
---
# <a name="urhosharp-windows-support"></a>UrhoSharp Windows 지원

플랫폼 특정 기능을 활용 하려는 동안 Urho 이식 가능한 클래스 라이브러리 이며, 게임 논리에 대 한 다양 한 플랫폼에서 사용할 동일한 API를 사용 하면 여전히 초기화 해야 Urho 플랫폼 특정 드라이버에 일부 경우, .

페이지 아래에서 가정 `MyGame` 의 서브 클래스는 `Application` 클래스.

**지원 되는 아키텍처:** 64 비트 Windows입니다.

이 사용 하는 방법을 보여 주는 전체 예제를 볼 수는 [샘플](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>독립 실행형 프로젝트

### <a name="creating-a-project"></a>프로젝트 만들기

콘솔 프로젝트를 만들고 Urho NuGet 참조를 찾을 수 있습니다 자산 (데이터 디렉터리를 포함 하는 디렉터리) 있는지 확인 합니다.

### <a name="configuring-and-launching-urho"></a>구성 및 Urho 시작

응용 프로그램을 시작 하려면이 수행 합니다.

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>예제

[전체 예제](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>WPF로 통합

### <a name="creating-a-project"></a>프로젝트 만들기

WPF 프로젝트를 만들고 Urho NuGet 참조를 찾을 수 있습니다 자산 (데이터 디렉터리를 포함 하는 디렉터리) 있는지 확인 합니다.

### <a name="configuring-and-launching-urho-from-wpf"></a>구성 및 WPF에서 Urho 시작

서브 클래스를 만든 `Window` 및 다음과 같은 자산을 구성 합니다.

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>예제

[전체 예제](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>UWP와 통합

### <a name="creating-a-project"></a>프로젝트 만들기

UWP 프로젝트를 만들고 Urho NuGet 참조 찾을 수 있는지 자산 (데이터 디렉터리를 포함 하는 디렉터리) 있는지 확인 합니다.

### <a name="configuring-and-launching-urho-from-uwp"></a>구성 및 Urho UWP에서 시작

서브 클래스를 만든 `Window` 및 다음과 같은 자산을 구성 합니다.

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

### <a name="example"></a>예제

[전체 예제](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windowsforms"></a>Windows.Forms와 통합

### <a name="creating-a-project"></a>프로젝트 만들기

Windows.Forms 프로젝트를 만들고 Urho NuGet 참조를 찾을 수 있습니다 자산 (데이터 디렉터리를 포함 하는 디렉터리) 있는지 확인 합니다.

### <a name="configuring-and-launching-urho-from-windowsforms"></a>구성 및 Urho Windows.Forms에서 시작

폼에서 Urho 시작, 참조 [전체 샘플](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
