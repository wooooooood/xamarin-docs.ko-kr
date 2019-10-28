---
title: Visual Basic.NET를 사용 하는 Xamarin 양식
description: VB.NET를 사용 하 여 플랫폼 간 모바일 앱을 효율적으로 빌드할 수 있도록 하는 주 어셈블리에 대 한 Visual Basic를 사용 하도록 Xamarin Forms 프로젝트 템플릿을 수정할 수 있습니다.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: conceptdev
ms.author: crdun
ms.date: 04/24/2019
ms.openlocfilehash: ed7e1d65ed361a94ce72a724d797309b40ef8b6c
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959153"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET를 사용 하는 Xamarin 양식

Xamarin은 Visual Basic를 직접 지원 하지 않습니다 .이 페이지의 지침에 따라 Xamarin.ios C# 솔루션을 만든 다음 .NET Standard C# 프로젝트를 Visual Basic 바꿉니다.

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)

[Xamarin.ios 솔루션![만든 다음 .NET Standard 프로젝트를 Visual Basic으로 바꿉니다.](xamarin-forms-images/hero-sml.png)](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic를 사용 하 여 프로그램을 시작 하려면 Windows의 Visual Studio를 사용 해야 합니다.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic 연습을 사용한 Xamarin 양식

Visual Basic를 사용 하는 간단한 Xamarin.ios 프로젝트를 만들려면 다음 단계를 수행 합니다.

1. Visual Studio 2019에서 **새 프로젝트 만들기**를 선택 합니다.

2. **새 프로젝트 만들기** 창에서 **xamarin.ios** 를 입력 하 여 목록을 필터링 하 고 **모바일 앱 (xamarin.ios)** 을 선택 하 고 **다음**을 누릅니다.

    [Xamarin. Forms 앱에 대 한![필터](xamarin-forms-images/02-sml.png)](xamarin-forms-images/02.png#lightbox)

3. 다음 화면에서 프로젝트의 이름을 입력 하 고 **만들기**를 누릅니다.

4. **빈** 템플릿을 선택 하 고 **확인을**누릅니다.

    [![빈 Xamarin.ios 템플릿](xamarin-forms-images/04-sml.png)](xamarin-forms-images/04.png#lightbox)

    그러면 Visual Studio에서를 사용 하 여 C#xamarin.ios 솔루션을 만듭니다. 다음 단계는 Visual Basic 사용 하도록 솔루션을 수정 합니다.

5. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트** ...를 선택 합니다.

6. **Visual Basic 라이브러리** 를 입력 하 여 프로젝트 옵션을 필터링 하 고 Visual Basic 아이콘을 사용 하 여 **클래스 라이브러리 (.NET Standard)** 옵션을 선택 합니다.

    [Visual Basic 라이브러리의![필터](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

7. 다음 화면에서 프로젝트의 이름을 입력 하 고 **만들기**를 누릅니다.

8. Visual Basic 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택한 다음 **기본 네임 스페이스** 를 기존 C# 프로젝트와 일치 하도록 변경 합니다.

    [![Visual Basic 루트 네임 스페이스가 Xamarin.ios 앱과 일치 하는지 확인 합니다.](xamarin-forms-images/07a-sml.png)](xamarin-forms-images/07a.png#lightbox)

9. 새 Visual Basic 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리**를 선택한 다음 **xamarin.ios** 를 설치 하 고 패키지 관리자 창을 닫습니다.

    [폼을![하 고 패키지 관리자 창을 닫습니다.](xamarin-forms-images/07b-sml.png)](xamarin-forms-images/07b.png#lightbox)

10. 기본 **Class1** 파일의 이름을 **App .vb**로 바꿉니다.

    [기본 Class1 파일 및 클래스를 앱으로 이름 바꾸기![](xamarin-forms-images/08.png)](xamarin-forms-images/08.png#lightbox)

11. 다음 코드를 **응용 프로그램 .vb** 파일에 붙여넣어 xamarin.ios 앱의 시작 지점이 됩니다.

    ```vb
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

12. Android 및 iOS 프로젝트를 업데이트 하 여 템플릿에서 만든 C# 프로젝트가 아닌 새 Visual Basic 프로젝트를 참조 하도록 합니다.
Android 및 iOS 프로젝트에서 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 여 **참조 관리자**를 엽니다. C# 라이브러리 및 Visual Basic 라이브러리의 틱을 해제 합니다 (Android 및 iOS 프로젝트 모두에 대해이 작업을 수행 하는 것은 잊지 않음).

    [이전 프로젝트 참조를 제거![Visual Basic 참조를 추가 합니다.](xamarin-forms-images/10-sml.png)](xamarin-forms-images/10.png#lightbox)

13. 프로젝트를 C# 삭제 합니다. 새 **.vb** 파일을 추가 하 여 xamarin.ios 응용 프로그램을 빌드합니다. Visual Basic 새 `ContentPage`s에 대 한 템플릿은 다음과 같습니다.

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.ios의 Visual Basic 제한 사항

[이식 가능한 Visual Basic.NET 페이지](~/cross-platform/platform/visual-basic/index.md)에서 설명한 대로 Xamarin은 Visual Basic 언어를 지원 하지 않습니다. 즉, Visual Basic를 사용할 수 있는 위치에 몇 가지 제한 사항이 있습니다.

- XAML 페이지는 Visual Basic 프로젝트에 포함 될 수 없습니다. 코드에 포함 된 생성기만 빌드할 C#수 있습니다. 별도의 참조 된 C# 이식 가능한 클래스 라이브러리에 xaml을 포함 하 고 데이터 바인딩을 사용 하 여 Visual Basic 모델을 통해 xaml 파일을 채울 수 있습니다 (이에 대 한 예제는 [샘플](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)에 포함 됨).

- 사용자 지정 렌더러는 Visual Basic 작성할 수 없으며 네이티브 플랫폼 프로젝트 C# 에서 작성 해야 합니다.

- 종속성 서비스 구현은 Visual Basic 작성할 수 없으며 네이티브 플랫폼 프로젝트 C# 에 작성 되어야 합니다.

## <a name="related-links"></a>관련 링크

- [XamarinFormsVB (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Framework를 사용한 크로스 플랫폼 개발](https://docs.microsoft.com/dotnet/standard/cross-platform/)
