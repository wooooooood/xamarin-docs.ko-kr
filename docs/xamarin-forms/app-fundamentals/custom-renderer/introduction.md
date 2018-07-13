---
title: 사용자 지정 렌더러 소개
description: 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명 합니다.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998006"
---
# <a name="introduction-to-custom-renderers"></a>사용자 지정 렌더러 소개

_사용자 지정 렌더러 Xamarin.Forms 컨트롤의 동작과 모양을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 이러한 작은 스타일 변경 또는 복잡 한 플랫폼 특정 레이아웃 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명 합니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md) 플랫폼 간 모바일 사용자 인터페이스를 설명 하는 공용 API를 제공 합니다. 각 페이지, 레이아웃 및 컨트롤 각 플랫폼에서 다르게 렌더링 됩니다 사용 하는 `Renderer` 클래스 (표현에 해당 하는 Xamarin.Forms), 네이티브 컨트롤을 다시 만드는 화면에서 정렬 및에 지정 된 동작을 추가 합니다 공유 코드입니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 다른 플랫폼;의 기본 동작을 허용 하는 동안 한 곳에서 제어를 사용자 지정 응용 프로그램 프로젝트에 지정된 된 형식에 대 한 사용자 지정 렌더러를 추가할 수 있습니다. 또는 다른 사용자 지정 렌더러를 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 다른 모양 및 느낌을 만들려면 각 응용 프로그램 프로젝트에 추가할 수 있습니다. 그러나 간단한 컨트롤 사용자 지정 하는 데 사용자 지정 렌더러 클래스를 구현 경우가 중량 응답 합니다. 효과이 프로세스를 간소화 하 고 작은 스타일 변경에 대 한 일반적으로 사용 됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

## <a name="examining-why-custom-renderers-are-necessary"></a>검토 이유는 사용자 지정 렌더러는 필요

Xamarin.Forms 컨트롤의 모양 변경, 사용자 지정 렌더러를 사용 하지 않고는 서브클래싱, 및 원래 컨트롤 대신 사용자 지정 컨트롤을 사용 하는 다음을 통해 사용자 지정 컨트롤을 만들어야 하는 2 단계 프로세스입니다. 다음 코드 예제에서는 하위 클래스의 예를 보여 줍니다.는 `Entry` 제어 합니다.

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` 컨트롤은는 `Entry` 위치를 제어 합니다 `BackgroundColor` 로 설정 되어 회색, 및 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤 요소에서 네임 스페이스 접두사를 사용 하 여 Xaml에서 참조할 수 있습니다. 다음 코드 예제에서는 하는 방법을 `MyEntry` 사용자 지정 컨트롤에서 사용할 수는 `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` 네임 스페이스 접두사 아무 이름이 나 가능 합니다. 그러나 합니다 `namespace` 고 `assembly` 값 사용자 지정 컨트롤의 세부 정보를 일치 해야 합니다. 네임 스페이스를 선언 하는 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

> [!NOTE]
> 정의 된 `xmlns` 공유 프로젝트 보다.NET Standard 라이브러리 프로젝트에서 훨씬 간단 합니다. .NET Standard 라이브러리를 결정 하는 것이 쉽습니다 있도록는 어셈블리로 컴파일되는 `assembly=CustomRenderer` 값 이어야 합니다. XAML 등 모든 공유 자산은 각 참조는 프로젝트, 즉 iOS, Android 및 UWP로 컴파일되는 공유 프로젝트를 사용 하는 경우 프로젝트 자체적인 *어셈블리 이름* 불가능에 쓸 `xmlns` 선언 각 응용 프로그램 마다 다를 수 값이 필요 하기 때문입니다. 공유 프로젝트에 대 한 XAML의 사용자 지정 컨트롤에는 모든 응용 프로그램 프로젝트를 동일한 어셈블리 이름을 사용 하 여 구성 해야 합니다.

`MyEntry` 다음 스크린샷과에서 같이 회색 배경으로 각 플랫폼에서 사용자 지정 컨트롤 다음 렌더링 됩니다.

![](introduction-images/screenshots.png "각 플랫폼에서 MyEntry 사용자 지정 컨트롤")

각 플랫폼에서 컨트롤의 배경색을 변경 컨트롤 서브클래싱 통해 순수 하 게 완성 되었습니다. 그러나이 기술은 플랫폼별 향상 된 기능 및 사용자 지정을 활용할 수 아니므로 가능에서 제한 됩니다. 필요한 경우 이러한 사용자 지정 렌더러를 구현 합니다.

## <a name="creating-a-custom-renderer-class"></a>사용자 지정 렌더러 클래스 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 네이티브 컨트롤을 렌더링 하는 렌더러 클래스의 서브 클래스를 만듭니다.
1. 컨트롤을 사용자 지정 하는 논리를 작성 및 네이티브 컨트롤을 렌더링 하는 메서드를 재정의 합니다. 종종는 `OnElementChanged` 메서드 사용은 해당 Xamarin.Forms 컨트롤이 생성 될 때 호출 되는 네이티브 컨트롤을 렌더링 합니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하는 선택 사항입니다. 사용자 지정 렌더러를 등록 하지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다. 그러나 사용자 지정 렌더러 필요한 각 플랫폼 프로젝트에서 렌더링 하는 경우는 [뷰](xref:Xamarin.Forms.View) 또는 [ViewCell](xref:Xamarin.Forms.ViewCell) 요소입니다.

이 시리즈의 항목에는 데모 및이 프로세스의 다양 한 Xamarin.Forms 요소에 대 한 설명을 제공 합니다.

## <a name="troubleshooting"></a>문제 해결

사용자 지정 컨트롤이 포함 된 추가 되었습니다 (예: 하지.NET 표준 라이브러리를 Mac/Visual Studio Xamarin.Forms 앱 프로젝트 템플릿에 대 한 Visual Studio에서 만든) 솔루션에는 예외는.NET Standard 라이브러리 프로젝트를 iOS에서 발생할 수 있습니다 때 사용자 지정 컨트롤에 액세스 하려고 합니다. 사용자 지정 컨트롤에 대 한 참조를 만들어 해결할 수 있는 경우이 문제는 `AppDelegate` 클래스:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

이렇게 하면 컴파일러에서 인식할 수는 `ClassInPCL` 것을 확인 하 여 형식입니다. 또는 합니다 `Preserve` 특성을 추가할 수는 `AppDelegate` 동일한 결과 달성 하기 위해 클래스:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

에 대 한 참조를 만들고이 `ClassInPCL` 런타임에 필수인 있는지를 나타내는 형식입니다. 자세한 내용은 [유지 코드](~/ios/deploy-test/linker.md)합니다.

## <a name="summary"></a>요약

이 문서는 사용자 지정 렌더러 소개를 제공 하 고 사용자 지정 렌더러를 만드는 과정을 간략히 설명 했습니다. 사용자 지정 렌더러 Xamarin.Forms 컨트롤의 동작과 모양을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 이러한 작은 스타일 변경 또는 복잡 한 플랫폼 특정 레이아웃 동작 사용자 지정에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
