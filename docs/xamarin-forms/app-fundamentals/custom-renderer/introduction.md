---
title: 사용자 지정 렌더러 소개
description: 사용자 지정 렌더러 모양 및 Xamarin.Forms 컨트롤의 동작을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 작은 스타일 변경 내용 또는 정교한 플랫폼 특정 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러 만들기 위한 프로세스를 보여 줍니다.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 43b021b158bbb815ab8d27c393f54e0775599940
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="introduction-to-custom-renderers"></a>사용자 지정 렌더러 소개

_사용자 지정 렌더러 모양 및 Xamarin.Forms 컨트롤의 동작을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 작은 스타일 변경 내용 또는 정교한 플랫폼 특정 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다. 이 문서에서는 사용자 지정 렌더러를 소개 하 고 사용자 지정 렌더러 만들기 위한 프로세스를 보여 줍니다._

Xamarin.Forms [페이지, 레이아웃 및 컨트롤](~/xamarin-forms/user-interface/controls/index.md) 플랫폼 간 모바일 사용자 인터페이스를 설명 하기 위해 공용 API를 제공 합니다. 각 페이지, 레이아웃 및 컨트롤은 다른 방식으로 렌더링 각 플랫폼에서 사용 하 여는 `Renderer` 화면에서 정렬 하 고에 지정 된 동작을 추가 하는 다시 (Xamarin.Forms 표현에 해당), 네이티브 컨트롤을 만드는 클래스는 공유 코드입니다.

개발자는 컨트롤의 모양 및/또는 동작을 사용자 지정하기 위해 자신 만의 사용자 지정 `Renderer` 클래스를 구현할 수 있습니다. 지정된 된 형식에 대 한 사용자 지정 렌더러; 다른 플랫폼에서 기본 동작을 허용 하는 동안 한 곳에서 컨트롤을 사용자 지정할 수 하나의 응용 프로그램 프로젝트에 추가할 수 있습니다. 또는 다른 사용자 지정 렌더러를 iOS, Android 및 유니버설 Windows 플랫폼 (UWP)에서 다른 모양과 느낌을 만들어 각 응용 프로그램 프로젝트에 추가할 수 있습니다. 그러나 단순 컨트롤 사용자 지정을 수행 하는 사용자 지정 렌더러 클래스를 구현 방식은 규모 응답 합니다. 효과이 프로세스를 간소화 하 고 작은 스타일 변경 내용에 대해 일반적으로 사용 됩니다. 자세한 내용은 [효과](~/xamarin-forms/app-fundamentals/effects/index.md)를 참조하세요.

## <a name="examining-why-custom-renderers-are-necessary"></a>검사 하는 중 이유 사용자 지정 렌더러는 필요한 경우

Xamarin.Forms 컨트롤의 모양을 변경, 사용자 지정 렌더러를 사용 하지 않고은 서브클래싱을 하 고 다음 원래 컨트롤 대신 사용자 지정 컨트롤을 사용을 통해 사용자 지정 컨트롤을 만들어야 하는 2 단계 프로세스입니다. 다음 코드 예제에서는 서브클래싱의 예가 나와 `Entry` 제어:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` 컨트롤은 한 `Entry` 위치를 제어는 `BackgroundColor` 로 설정 되어, 회색으로 표시 되며 해당 위치에 대 한 네임 스페이스를 선언 하 고 컨트롤 요소에서 네임 스페이스 접두사를 사용 하 여 Xaml에서 참조할 수 있습니다. 다음 코드 예제는 방법을 `MyEntry` 사용자 지정 컨트롤에서 사용할 수는 `ContentPage`:

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

`local` 네임 스페이스 접두사는 모두 사용할 수 있습니다. 그러나는 `namespace` 및 `assembly` 값에는 사용자 지정 컨트롤의 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

> [!NOTE]
> 정의 `xmlns` 공유 프로젝트 보다 PCLs에서 훨씬 간단 합니다. 쉽게 작업을 결정 하는 PCL는 어셈블리로 컴파일되는 `assembly=CustomRenderer` 값 이어야 합니다. (XAML) 모든 공유 자산 iOS, Android 및 UWP 의미는 참조 프로젝트의 각로 컴파일되는 공유 프로젝트를 사용할 경우 프로젝트에는 자신의 *어셈블리 이름을* 수 없으면를 쓸 수 `xmlns` 선언 하지 않아도 각 응용 프로그램에 대 한 값 필요 하기 때문입니다. 공유 프로젝트에 대 한 XAML에서 사용자 지정 컨트롤에는 모든 응용 프로그램 프로젝트를 동일한 어셈블리 이름으로 구성할 필요 합니다.

`MyEntry` 사용자 지정 컨트롤 다음 스크린샷에 표시 된 것 처럼 그런 다음 회색 배경으로 각 플랫폼에서 렌더링 됩니다.

![](introduction-images/screenshots.png "각 플랫폼에서 MyEntry 사용자 지정 컨트롤")

각 플랫폼에서 컨트롤의 배경색을 변경은 순수 하 게 컨트롤을 서브클래싱하 통해 달성 되었습니다. 그러나이 방법은 플랫폼 특정 향상 기능 및 사용자 지정을 활용할 수 없으면으로 얻을 수 있습니다 어떻게 제한적입니다. 필요할 때 사용자 지정 렌더러를 구현 해야 합니다.

## <a name="creating-a-custom-renderer-class"></a>사용자 지정 렌더러 클래스 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 기본 컨트롤을 렌더링 하는 렌더러 클래스의 서브 클래스를 만듭니다.
1. 네이티브 컨트롤을 렌더링 하는 메서드를 재정의 하 고 컨트롤을 사용자 지정 하는 논리를 작성 합니다. 종종는 `OnElementChanged` 메서드를 사용 하면 해당 Xamarin.Forms 컨트롤이 만들어질 때 호출 되는 네이티브 컨트롤을 렌더링 합니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소에 대 한 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하기 선택 사항입니다. 사용자 지정 렌더러 등록 되지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다. 그러나 사용자 지정 렌더러 필요한 각 플랫폼 프로젝트에 렌더링 하는 경우는 [보기](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 또는 [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) 요소입니다.

이 시리즈의 항목에는 데모 및이 프로세스의 다양 한 Xamarin.Forms 요소에 대 한 설명을 제공 합니다.

## <a name="troubleshooting"></a>문제 해결

사용자 지정 컨트롤 (즉, 하지 PCL Mac/Visual Studio Xamarin.Forms 응용 프로그램 프로젝트 템플릿에 대 한 Visual Studio에서 만든) 솔루션에 추가 된 PCL 프로젝트에 포함 되 면 사용자 지정 컨트롤에 액세스 하려고 할 때 iOS에서 예외가 발생할 수 있습니다. 사용자 지정 컨트롤에 대 한 참조를 만들어이 해결할 수 있습니다이 문제가 발생 한 경우는 `AppDelegate` 클래스:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

이렇게 하면 강제로 인식할 수 컴파일러는 `ClassInPCL` 것을 확인 하 여 형식입니다. 또는 `Preserve` 특성에 추가할 수는 `AppDelegate` 동일한 결과를 얻기 위해 클래스:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

에 대 한 참조를 만들고이 `ClassInPCL` 필요는 런타임 시를 나타내는 형식입니다. 자세한 내용은 참조 [유지 코드](~/ios/deploy-test/linker.md)합니다.

## <a name="summary"></a>요약

이 문서에서 사용자 지정 렌더러의 경우에 대 한 소개를 제공 하 고 사용자 지정 렌더러 만들기 위한 프로세스를 간략히 설명 했습니다. 사용자 지정 렌더러 모양 및 Xamarin.Forms 컨트롤의 동작을 사용자 지정 하기 위한 강력한 도구를 제공 합니다. 작은 스타일 변경 내용 또는 정교한 플랫폼 특정 레이아웃 및 동작 사용자 지정에 사용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [효과](~/xamarin-forms/app-fundamentals/effects/index.md)
