---
title: 'F #을 사용한 프로그래밍 UrhoSharp'
description: '이 문서를 사용 하 여 F # Visual Studio에서 Mac.에 대 한 간단한 hello world UrhoSharp 응용 프로그램을 만드는 방법에 설명'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 64d69de70d6bc6f23b9907b498622b00c42b6f50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783274"
---
# <a name="programming-urhosharp-with-f"></a>F #을 사용한 프로그래밍 UrhoSharp

UrhoSharp 동일한 라이브러리 및 C# 프로그래머에 사용 되는 개념을 사용 하 여 F #으로 프로그래밍할 수 있습니다. [를 사용 하 여 UrhoSharp](~/graphics-games/urhosharp/using.md) 문서 UrhoSharp 엔진의 개요를 제공 하 고이 문서의 하기 전에 읽어야 합니다.

C + + 환경에서 발생 하는 여러 라이브러리와 마찬가지로 많은 UrhoSharp 함수 부울 이나 성공 또는 실패를 나타내는 정수를 반환 합니다. 사용 해야 `|> ignore` 이러한 값을 무시 하도록 합니다.

[샘플 프로그램](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) UrhoSharp에서 F #에 "Hello World"입니다.

## <a name="creating-an-empty-project"></a>빈 프로젝트 만들기

UrhoSharp에 대 한 F # 템플릿이 아직와 start 수 자신의 UrhoSharp 프로젝트를 만들 수 있으므로 사용할 수는 [샘플](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 하거나이 단계를 수행 합니다.

1. Mac 용 Visual Studio에서 만드는 새 **솔루션**합니다. 선택 **iOS > 앱 > 단일 앱 보기** 선택 **F #** 구현 언어로 합니다. 
1. 삭제 된 **Main.storyboard** 파일입니다. 열기는 **Info.plist** 파일 및는 **iPhone / iPod 배포 정보** 창에서 삭제는 `Main` 문자열에 **주 인터페이스** 드롭다운입니다.
1. 삭제 된 **ViewController.fs** 파일도 있습니다.

## <a name="building-hello-world-in-urho"></a>Urho의 빌딩 Hello World

이제 게임의 클래스 정의 시작할 준비가 되었습니다. 여기에 최소한의 서브 클래스를 정의 해야 합니다 `Urho.Application` 재정의 하 고 해당 `Start` 메서드. 이 파일을 만들려면 F # 프로젝트를 마우스 오른쪽 단추로 클릭, 선택 **새 파일 추가 중...**  프로젝트에 빈 F # 클래스를 추가 합니다. 새 파일 프로젝트의 파일 목록 끝에 추가 되지만 표시 되도록 끌어서 놓아야 *전에* 에 사용 되는 **AppDelegate.fs**합니다.

1. Urho NuGet 패키지에 대 한 참조를 추가 합니다.
1. 기존 Urho 프로젝트에서 (대량) 디렉터리를 복사 **대 한 CoreData /** 및 **데이터 /** 프로젝트로 **리소스 /** 디렉터리입니다. F # 프로젝트에서 마우스 오른쪽 단추로 클릭는 **리소스** 폴더를 사용 하 여 **추가 / 기존 폴더 추가** 이러한 파일의 모든 프로젝트에 추가 합니다.

이제 프로젝트 구조가 같습니다.

![](fsharp-images/solutionpane.png "이제 같은 모양이 프로젝트 구조")

새로 만든 클래스의 하위 유형으로 정의 `Urho.Application` 재정의 하 고 해당 `Start` 메서드:

```csharp
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

코드는 매우 단순 합니다. 사용 하 여는 `Urho.Gui.Text` 특정 글꼴 및 색 크기 가운데 맞춤 문자열을 표시 하는 클래스입니다. 

그러나이 코드를 실행 하려면 먼저 UrhoSharp 초기화 되어야 합니다. 

AppDelegate.fs 파일을 열고 수정 된 `FinishedLaunching` 메서드를 다음과 같이 합니다.

```csharp
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

`ApplicationOptions.Default` 가로 모드 응용 프로그램에 대 한 기본 옵션을 제공 합니다. 이러한 전달 `ApplicationOptions` 에 대 한 기본 생성자 프로그램 `Application` 하위 클래스 (정의할 때 사용자에 게 유의 `HelloWorld` 클래스, 줄 `inherit Application(o)` 기본 클래스 생성자를 호출). 

`Run` 방식의 프로그램 `Application` 프로그램을 시작 합니다. 반환으로 정의 되어 있는 `int`에 파이프 될 수 있습니다 `ignore`합니다. 

결과 프로그램 같이 표시 됩니다.

![](fsharp-images/helloworldfsharp.png "결과 프로그램 같아야 합니다.")








## <a name="related-links"></a>관련 링크

- [GitHub (샘플)에서 찾아보기](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
