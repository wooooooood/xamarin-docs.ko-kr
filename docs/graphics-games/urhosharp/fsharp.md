---
title: UrhoSharp 프로그래밍F#
description: 이 문서에서는 Mac용 Visual Studio에서를 사용 하 여 F# 간단한 Hello 세계 UrhoSharp 응용 프로그램을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: d87749bd74cf2c478e96284060fed7386d10b853
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621013"
---
# <a name="programming-urhosharp-with-f"></a>F #을 사용 하 여 UrhoSharp 프로그래밍\#

UrhoSharp는 C# 프로그래머가 사용 하 F# 는 것과 동일한 라이브러리 및 개념을 사용 하 여 프로그래밍할 수 있습니다. [Urhosharp 사용](~/graphics-games/urhosharp/using.md) 문서는 urhosharp 엔진의 개요를 제공 하며이 문서 앞에서 읽어야 합니다.

C++ 전 세계에서 시작 된 많은 라이브러리와 마찬가지로, 많은 UrhoSharp 함수는 성공 또는 실패를 나타내는 부울 또는 정수를 반환 합니다. 를 사용 `|> ignore` 하 여 이러한 값을 무시 해야 합니다.

[샘플 프로그램](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 은의 F#urhosharp에 대 한 "Hello World"입니다.

## <a name="creating-an-empty-project"></a>빈 프로젝트 만들기

UrhoSharp F# 아직 사용할 수 있는 템플릿이 없으므로 사용자 고유의 urhosharp 프로젝트를 만들려면 [샘플](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) 로 시작 하거나 다음 단계를 수행 하면 됩니다.

1. Mac용 Visual Studio에서 새 **솔루션**을 만듭니다. **IOS > 앱 > 단일 뷰 앱** 을 선택 하 **F#** 고 구현 언어로를 선택 합니다. 
1. **주 storyboard** 파일을 삭제 합니다. **Info.plist** 파일을 열고 **iPhone/iPod 배포 정보** 창에서 `Main` **주 인터페이스** 드롭다운에서 문자열을 삭제 합니다.
1. **Viewcontroller. fs** 파일도 삭제 합니다.

## <a name="building-hello-world-in-urho"></a>Urho에서 Hello World 빌드

이제 게임의 클래스 정의를 시작할 준비가 되었습니다. 최소한의 `Urho.Application` 서브 클래스를 정의 하 고 해당 `Start` 메서드를 재정의 해야 합니다. 이 파일을 만들려면 F# 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **새 파일 추가 ...** 를 선택 하 여 프로젝트에 빈 F# 클래스를 추가 합니다. 새 파일은 프로젝트의 파일 목록 끝에 추가 되지만 **AppDelegate**에서 사용 *되기 전에* 표시 되도록 끌어야 합니다 (예).

1. Urho NuGet 패키지에 대 한 참조를 추가 합니다.
1. 기존 Urho 프로젝트에서 (large) 디렉터리 **CoreData/** 및 **데이터/** 를 프로젝트의 **리소스/** 디렉터리에 복사 합니다. F# 프로젝트에서 **Resources** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **기존 폴더 추가/추가** 를 사용 하 여 이러한 모든 파일을 프로젝트에 추가 합니다.

이제 프로젝트 구조는 다음과 같습니다.

![](fsharp-images/solutionpane.png "이제 프로젝트 구조는 다음과 같습니다.")

새로 만든 클래스를의 `Urho.Application` 하위 형식으로 정의 하 고 해당 `Start` 메서드를 재정의 합니다.

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

이 코드는 매우 간단 합니다. 이 메서드는 `Urho.Gui.Text` 클래스를 사용 하 여 특정 글꼴 및 색 크기의 중심 맞춤 문자열을 표시 합니다. 

그러나이 코드를 실행 하기 전에 UrhoSharp을 초기화 해야 합니다. 

AppDelegate 파일을 열고 메서드를 `FinishedLaunching` 다음과 같이 수정 합니다.

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

는 `ApplicationOptions.Default` 가로 모드 응용 프로그램에 대 한 기본 옵션을 제공 합니다. 이러한 `ApplicationOptions` `HelloWorld` 클래스를 `Application` 서브 클래스의 기본 생성자에 전달 합니다. 클래스를 정의할 때 줄 `inherit Application(o)` 에서 기본 클래스 생성자를 호출 합니다.

의 메서드는 `Run` 프로그램을 시작합니다.`Application` 이는로 `ignore`파이프 될 수 `int`있는를 반환 하는 것으로 정의 됩니다.

결과 프로그램은 다음 스크린샷 처럼 표시 됩니다.

![결과 프로그램의 스크린샷](fsharp-images/helloworldfsharp.png)

## <a name="related-links"></a>관련 링크

- [GitHub에서 찾아보기 (샘플)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
