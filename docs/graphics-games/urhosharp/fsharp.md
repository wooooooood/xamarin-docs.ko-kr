---
title: 으로 UrhoSharp 프로그래밍F#
description: 이 문서에서는 간단한 hello world UrhoSharp 응용 프로그램을 만드는 방법을 설명 F# mac 용 Visual Studio에서
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6269a7f2fa097136f492657d0ba7c6a1f056c38c
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832327"
---
# <a name="programming-urhosharp-with-f"></a>으로 UrhoSharp 프로그래밍F#

UrhoSharp 사용 하 여 프로그래밍할 수 있습니다 F# 라이브러리와에서 사용 하는 개념을 사용 하 여 C# 프로그래머입니다. 합니다 [UrhoSharp 사용 하 여](~/graphics-games/urhosharp/using.md) 문서 UrhoSharp 엔진의 개요를 제공 하 고이 문서에서는 하기 전에 읽어야 합니다.

등에서 발생 하는 많은 라이브러리는 C++ 세계 많은 UrhoSharp 함수 부울 또는 성공 또는 실패를 나타내는 정수를 반환 합니다. 사용 해야 `|> ignore` 이러한 값을 무시 합니다.

합니다 [샘플 프로그램](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 는 "Hello World"에서 UrhoSharp입니다 F#합니다.

## <a name="creating-an-empty-project"></a>빈 프로젝트 만들기

가지 없습니다 F# 템플릿 UrhoSharp 아직 사용할 수 있으므로 프로젝트를 만드는 고유한 UrhoSharp 사용 하 여 시작할 수는 [샘플](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 이러한 단계를 따르세요.

1. Mac 용 Visual Studio에서 새를 만들 **솔루션**합니다. 선택할 **iOS > 앱 > 단일 뷰 앱** 선택한 **F#** 구현 언어입니다. 
1. 삭제 된 **Main.storyboard** 파일입니다. 열기는 **Info.plist** 파일 및를 **iPhone / iPod 배포 정보** 창 삭제를 `Main` 문자열을 **주 인터페이스** 드롭다운 합니다.
1. 삭제 된 **ViewController.fs** 파일도 있습니다.

## <a name="building-hello-world-in-urho"></a>Urho의 Hello World 작성

이제 게임의 클래스 정의 시작할 준비가 되었습니다. 최소한의 서브 클래스를 정의 해야 합니다 `Urho.Application` 시키고 해당 `Start` 메서드. 이 파일을 만들려면 마우스 오른쪽 단추로 클릭 하면 F# 프로젝트를 선택 **새 파일 추가...**  빈을 추가 하 고 F# 프로젝트에 클래스입니다. 새 파일을 프로젝트에서 파일 목록의 끝에 추가 되지만 나타나도록 끌어 놓아야 *하기 전에* 에서 사용 됩니다 **AppDelegate.fs**합니다.

1. Urho NuGet 패키지에 대 한 참조를 추가 합니다.
1. 기존 Urho 프로젝트에서 (큼) 디렉터리를 복사 **CoreData /** 하 고 **데이터 /** 프로젝트의 **리소스 /** 디렉터리입니다. 에 F# 프로젝트를 마우스 오른쪽 단추로 클릭는 **리소스** 폴더 및 사용 하 여 **추가 / 기존 폴더 추가** 프로젝트에 모든이 파일을 추가할 합니다.

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

결과 프로그램은이 스크린샷과 같이 표시 됩니다.

![결과 프로그램의 스크린 샷](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>관련 링크

- [(샘플) GitHub에서 찾아보기](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
