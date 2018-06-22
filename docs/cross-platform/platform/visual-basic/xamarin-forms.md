---
title: Visual basic.net을 사용 하 여 Xamarin.Forms
description: Xamarin.Forms PCL 프로젝트 템플릿은 효과적으로 VB.NET를 사용 하 여 플랫폼 간 모바일 앱을 빌드할 수 있도록 하는 주 어셈블리에 대 한 Visual Basic을 사용 하도록 수정할 수 있습니다.
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b858e26de95d2abbc23917b1ed5a1de65105cd8d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917865"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual basic.net을 사용 하 여 Xamarin.Forms

Xamarin Visual Basic을 직접 지원 하지 않습니다-C# PCL Xamarin.Forms 솔루션을 만든 다음 Visual Basic을 사용한 일반적인 코드 PCL 프로젝트를 대체 하려면이 페이지의 지시를 따릅니다.

[![](xamarin-forms-images/hero-sml.png "PCL Xamarin.Forms 솔루션을 만든 다음 Visual Basic을 사용한 일반적인 코드 PCL 프로젝트를 대체")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual basic 프로그램에 Windows에서 Visual Studio를 사용 해야 합니다.

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic 연습을 Xamarin.Forms

Visual Basic을 사용 하는 간단한 Xamarin.Forms 프로젝트를 만들려면 다음이 단계를 수행 합니다.

1. 새 *Xamarin.Forms C#* PCL 이식 가능한 클래스 라이브러리 ()를 사용 하는 솔루션입니다.
로 이동 **파일 > 새 프로젝트** 및는 **새 프로젝트** 창으로 이동 **설치 됨 > 템플릿 > Visual C# > 크로스 플랫폼** 선택  **크로스 플랫폼 앱 (Xamarin.Forms 또는 네이티브) > Xamarin.Forms**합니다.

2. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트**합니다.

3. 선택 된 **Visual Basic > 클래스 라이브러리 (이식 가능)** 프로젝트 유형:

   [![](xamarin-forms-images/add-vb-2-sml.png "새로운 이식 가능한 클래스 라이브러리 프로젝트에 추가")](xamarin-forms-images/add-vb-2.png#lightbox)

4. 올바른 PCL 프로필을 구성 하려면 표시 된 것 처럼 플랫폼을 선택 (포함 해야 Xamarin.iOS 및 Xamarin.Android):

   ![](xamarin-forms-images/add-vb-3-sml.png "지원 하기 위해 플랫폼 선택")

5. Visual Basic 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **속성**, 다음 변경 된 **기본 네임 스페이스** 에 맞게 기존 C# 프로젝트:

   ![](xamarin-forms-images/add-vb-4s-sml.png "Visual Basic 루트 네임 스페이스 Xamarin.Forms 응용 프로그램 일치 확인")

6. 새 Visual Basic 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **Nuget 패키지 관리**, 설치 후 **Xamarin.Forms** 패키지 관리자 창을 닫습니다.

   [![](xamarin-forms-images/add-vb-4-sml.png "폼과 패키지 관리자 창 닫기")](xamarin-forms-images/add-vb-4.png#lightbox)

7. 기본 이름 바꾸기 **Class1** 파일 *및* 클래스를 `App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "응용 프로그램에 기본 Class1 파일을 클래스 이름 바꾸기")](xamarin-forms-images/add-vb-5.png#lightbox)

8. 에 다음 코드를 붙여는 **App.vb** 파일 Xamarin.Forms 응용 프로그램의 시작 지점이 됩니다. 포함 하십시오 `Imports Xamarin.Forms` 추가 `Inherits Application` 클래스:

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

9. 이제 새 Visual Basic 프로젝트에서 iOS 및 Android 프로젝트를 가리키도록 해야 합니다.
마우스 오른쪽 단추로 클릭는 **참조** iOS 및 Android 프로젝트를 열려면에서 노드는 **참조 관리자**합니다. 취소 눈금은 C# 이식 가능한 라이브러리와 눈금 VB 이식 가능한 라이브러리 (것을 잊지 마십시오 iOS 및 Android 프로젝트에이 작업을 수행).

   [![](xamarin-forms-images/add-vb-8-sml.png "이전 프로젝트 참조를 제거, Visual Basic 참조를 추가 합니다.")](xamarin-forms-images/add-vb-8.png#lightbox)

10. C# 이식 가능한 프로젝트를 삭제 합니다. 새로 추가 **.vb** 를 빌드하기 위한 파일 Xamarin.Forms 응용 프로그램을 종료 합니다. 새 템플릿을 `ContentPage`Visual Basic의 s는 다음과 같습니다.

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.Forms에는 Visual Basic의 제한 사항

설명 했 듯이 [휴대용 Visual Basic.NET 페이지](~/cross-platform/platform/visual-basic/index.md), Xamarin Visual Basic 언어를 지원 하지 않습니다. 즉, Visual Basic을 사용할 수 있습니다에 몇 가지 제한이 있습니다.

 - Visual Basic의 사용자 지정 렌더러를 쓸 수 없습니다, 기록해 야 합니다 C#에서 네이티브 플랫폼 프로젝트에서.

 - Visual Basic에서 종속성 서비스 구현의 쓸 수 없습니다, 기록해 야 합니다 C#에서 네이티브 플랫폼 프로젝트에서.

 - XAML 페이지는 Visual Basic 프로젝트에 포함할 수 없습니다-코드 숨김 생성기 C# 빌드만 수 있습니다. 별도, 참조, C# 이식 가능한 클래스 라이브러리에 XAML을 포함 하 고 데이터 바인딩을 사용 하 여 Visual Basic 모델을 통해 XAML 파일을 채울 수 (이 예에 포함 되어는 [샘플](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages)).

 - Xamarin Visual Basic.NET 언어를 지원 하지 않습니다.

## <a name="related-links"></a>관련 링크

- [XamarinFormsVB (샘플)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft)로 플랫폼 간 개발](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
