---
title: 'F #으로 UrhoSharp 프로그래밍'
description: '이 문서에서는 간단한 hello world UrhoSharp 응용 프로그램을 사용 하 여 F #에서 Visual Studio for mac을 만드는 방법 설명'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 430c4eca7c6dbd7107692246b70ff93bafa44d01
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241836"
---
# <a name="programming-urhosharp-with-f"></a>F #으로 UrhoSharp 프로그래밍

UrhoSharp 동일한 라이브러리 및 C# 프로그래머가 사용 되는 개념을 사용 하 여 F #을 사용 하 여 프로그래밍할 수 있습니다. 합니다 [UrhoSharp 사용 하 여](~/graphics-games/urhosharp/using.md) 문서 UrhoSharp 엔진의 개요를 제공 하 고이 문서에서는 하기 전에 읽어야 합니다.

C + + 세계에서 발생 하는 여러 라이브러리와 같은 많은 UrhoSharp 함수 부울 또는 성공 또는 실패를 나타내는 정수를 반환 합니다. 사용 해야 `|> ignore` 이러한 값을 무시 합니다.

합니다 [샘플 프로그램](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) UrhoSharp F #에서 "Hello World"입니다.

## <a name="creating-an-empty-project"></a>빈 프로젝트 만들기

UrhoSharp에 대 한 F # 템플릿이 없습니다 아직 고유한 UrhoSharp 프로젝트를 만들 수 있으므로 사용할 수 있습니다 하거나 시작 합니다 [샘플](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 이러한 단계를 따르세요.

1. Mac 용 Visual Studio에서 새를 만들 **솔루션**합니다. 선택할 **iOS > 앱 > 단일 뷰 앱** 선택한 **F #** 구현 언어입니다. 
1. 삭제 된 **Main.storyboard** 파일입니다. 열기는 **Info.plist** 파일 및를 **iPhone / iPod 배포 정보** 창 삭제를 `Main` 문자열을 **주 인터페이스** 드롭다운 합니다.
1. 삭제 된 **ViewController.fs** 파일도 있습니다.

## <a name="building-hello-world-in-urho"></a>Urho의 Hello World 작성

이제 게임의 클래스 정의 시작할 준비가 되었습니다. 최소한의 서브 클래스를 정의 해야 합니다 `Urho.Application` 시키고 해당 `Start` 메서드. 이 파일을 만들려면 F # 프로젝트를 마우스 오른쪽 단추로 클릭, 선택 **새 파일 추가...**  프로젝트에 빈 F # 클래스를 추가 합니다. 새 파일을 프로젝트에서 파일 목록의 끝에 추가 되지만 나타나도록 끌어 놓아야 *하기 전에* 에서 사용 됩니다 **AppDelegate.fs**합니다.

1. Urho NuGet 패키지에 대 한 참조를 추가 합니다.
1. 기존 Urho 프로젝트에서 (큼) 디렉터리를 복사 **CoreData /** 하 고 **데이터 /** 프로젝트의 **리소스 /** 디렉터리입니다. F # 프로젝트에서 마우스 오른쪽 단추로 클릭는 **리소스** 폴더 및 사용 **추가 / 기존 폴더 추가** 이러한 파일의 모든 프로젝트에 추가 합니다.

프로젝트 구조 다음과 같아집니다.

![](fsharp-images/solutionpane.png "프로젝트 구조 다음과 같아집니다.")

새로 만든 클래스의 하위 유형으로 정의할 `Urho.Application` 시키고 해당 `Start` 메서드:

```fsharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

코드는 매우 간단 합니다. 사용 된 `Urho.Gui.Text` 특정 글꼴 및 색 크기를 사용 하 여 가운데 맞춤 문자열을 표시 하는 클래스입니다. 

그러나이 코드를 실행 하기 전에 UrhoSharp 초기화 되어야 합니다. 

AppDelegate.fs 파일을 열고 수정 합니다 `FinishedLaunching` 같이 메서드:

```fsharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

`ApplicationOptions.Default` 를 가로 모드로 응용 프로그램에 대 한 기본 옵션을 제공 합니다. 전달 `ApplicationOptions` 에 대 한 기본 생성자에 `Application` 하위 클래스입니다 (정의할 때 사용자에 게 유의 합니다 `HelloWorld` 클래스에서 줄 `inherit Application(o)` 기본 클래스 생성자를 호출). 

`Run` 메서드의 여 `Application` 프로그램을 시작 합니다. 반환으로 정의 되어는 `int`에 파이프 될 수 있는 `ignore`합니다. 

결과 프로그램이 같습니다.

![](fsharp-images/helloworldfsharp.png "프로그램 결과와 같아야 합니다.")








## <a name="related-links"></a>관련 링크

- [(샘플) GitHub에서 찾아보기](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
