---
title: 사용자 지정 렌더러 소개
description: 이 문서에서는 사용자 지정 렌더러를 소개하고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명합니다.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: ad2868a82f662f45066a6111a1dd3bd2aacad671
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70771875"
---
# <a name="introduction-to-custom-renderers"></a>사용자 지정 렌더러 소개

_사용자 지정 렌더러는 Xamarin.Forms 컨트롤의 모양과 동작을 사용자 지정하는 강력한 방법을 제공합니다. 작은 스타일 변경 또는 정교한 플랫폼별 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개하고 사용자 지정 렌더러를 만드는 과정을 간략하게 설명합니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md)은 플랫폼 간 모바일 사용자 인터페이스를 설명하는 공용 API를 제공합니다. 각 페이지, 레이아웃 및 컨트롤은 차례로 네이티브 컨트롤을 만들고(Xamarin.Forms 표현에 해당), 화면에 정렬하고, 공유 코드에 지정한 동작을 추가하는 `Renderer` 클래스를 사용하여 각 플랫폼에서 다르게 렌더링됩니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 다른 플랫폼에서 기본 동작을 허용하는 동안 한 장소에서 컨트롤을 사용자 지정하려면 지정된 형식의 사용자 지정 렌더러를 애플리케이션 프로젝트에 추가할 수 있습니다. 또는 iOS, Android 및 UWP(유니버설 Windows 플랫폼)에서 다른 모양 및 느낌을 만들려면 다른 사용자 지정 렌더러를 각 애플리케이션 프로젝트에 추가할 수 있습니다. 그러나 간단한 컨트롤 사용자 지정을 수행하는 사용자 지정 렌더러 클래스를 구현하는 것은 종종 중량 응답입니다. 효과는 이 프로세스를 간소화하고 일반적으로 작은 스타일 변경에 사용됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

## <a name="examining-why-custom-renderers-are-necessary"></a>사용자 지정 렌더러가 필요한 이유 검토

사용자 지정 렌더러를 사용하지 않고 Xamarin.Forms 컨트롤의 모양 변경은 서브클래싱을 통해 사용자 지정 컨트롤을 만드는 방법 및 원래 컨트롤 대신 사용자 지정 컨트롤을 사용하는 방법이 포함되는 2단계 프로세스입니다. 다음 코드 예제에서는 `Entry` 컨트롤을 서브클래싱하는 예를 보여 줍니다.

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` 컨트롤은 `BackgroundColor`가 회색으로 설정되며, 해당 위치의 네임스페이스를 선언하고 컨트롤 요소의 네임스페이스 접두사를 사용하여 Xaml에서 참조할 수 있는 `Entry` 컨트롤입니다. 다음 코드 예제에서는 `ContentPage`에서 `MyEntry` 사용자 지정 컨트롤을 사용하는 방법을 보여줍니다.

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

`local` 네임스페이스 접두사는 어떤 것으로든 지정할 수 있습니다. 그러나 `namespace` 및 `assembly` 값은 사용자 지정 컨트롤의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 컨트롤을 참조하는 데 사용됩니다.

> [!NOTE]
> `xmlns`의 정의는 공유 프로젝트 보다 .NET Standard 라이브러리 프로젝트에서 훨씬 간단합니다. .NET Standard 라이브러리를 어셈블리로 컴파일하므로 `assembly=CustomRenderer` 값을 결정하기가 쉽습니다. 공유 프로젝트를 사용하는 경우 모든 공유 자산(XAML 포함)은 각 참조 프로젝트로 컴파일됩니다. 즉, iOS, Android 및 UWP 프로젝트에 고유한 *어셈블리 이름*이 있는 경우 값이 각 애플리케이션에 대해 서로 달라야 하므로 `xmlns` 선언을 작성할 수 없습니다. 공유 프로젝트에 대한 XAML의 사용자 지정 컨트롤에서는 모든 애플리케이션 프로젝트가 동일한 어셈블리 이름으로 구성되어야 합니다.

다음 스크린샷에 표시된 것처럼 `MyEntry` 사용자 지정 컨트롤은 회색 배경의 각 플랫폼에서 렌더링됩니다.

![](introduction-images/screenshots.png "각 플랫폼의 MyEntry 사용자 지정 컨트롤")

각 플랫폼에서 컨트롤의 배경 색상은 컨트롤 서브클래싱을 통해서만 변경할 수 있습니다. 그러나 플랫폼별 향상 및 사용자 지정을 활용할 수 없으므로 이 기술은 완수할 수 있는 작업에 제약이 있습니다. 필요한 경우 사용자 지정 렌더러를 구현해야 합니다.

## <a name="creating-a-custom-renderer-class"></a>사용자 지정 렌더러 클래스 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 네이티브 컨트롤을 렌더링하는 렌더러 클래스의 서브클래스를 만듭니다.
1. 네이티브 컨트롤을 렌더링하고 컨트롤을 사용자 지정하기 위한 논리를 작성하는 메서드를 재정의합니다. `OnElementChanged` 메서드는 해당 Xamarin.Forms 컨트롤이 생성될 때 호출되는 네이티브 컨트롤을 렌더링하는 데 사용됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 컨트롤을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 랜더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소의 경우 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 컨트롤의 기본 클래스에 대한 기본 렌더러가 사용됩니다. 그러나 [보기](xref:Xamarin.Forms.View) 요소 또는 [ViewCell](xref:Xamarin.Forms.ViewCell) 요소를 렌더링하는 경우 각 플랫폼 프로젝트에 사용자 지정 렌더러가 필요합니다.

이 시리즈의 항목에서는 다양한 Xamarin.Forms 요소에 대한 이 프로세스의 설명 및 데모를 제공합니다.

## <a name="troubleshooting"></a>문제 해결

솔루션에 추가된 .NET Standard 라이브러리 프로젝트(예: Mac용 Visual Studio/Visual Studio Xamarin.Forms 앱 프로젝트에서 만든 .NET Standard 라이브러리가 아님)에 사용자 지정 컨트롤이 포함된 경우 사용자 지정 컨트롤에 액세스 하려고 할 때 iOS에서 예외가 발생할 수 있습니다. 이 문제가 발생한 경우 `AppDelegate` 클래스에서 사용자 지정 컨트롤에 대한 참조를 만들어 해결할 수 있습니다.

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

이렇게 하면 컴파일러가 문제를 해결하여 `ClassInPCL` 형식을 강제로 인식합니다. 또는 `Preserve` 특성을 `AppDelegate` 클래스에 추가하여 동일한 결과를 얻을 수 있습니다.

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

이렇게 하면 `ClassInPCL` 형식에 대한 참조를 만들고 런타임 시 필요한지를 나타냅니다. 자세한 내용은 [코드 유지](~/ios/deploy-test/linker.md)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 사용자 지정 렌더러를 소개했으며 사용자 지정 렌더러를 만드는 과정을 간략하게 설명했습니다. 사용자 지정 렌더러는 Xamarin.Forms 컨트롤의 모양과 동작을 사용자 지정하는 강력한 방법을 제공합니다. 작은 스타일 변경 또는 정교한 플랫폼별 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
