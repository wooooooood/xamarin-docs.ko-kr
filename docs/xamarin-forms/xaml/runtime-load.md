---
title: Xamarin.Forms에서 런타임에 XAML 로드
description: XAML 로드 하 고 LoadFromXaml 확장 메서드를 사용 하 여 런타임에 구문 분석할 수 있습니다.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: ce8ba32a1a6a1f69033615558c7ebf15d41e70fe
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61178048"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Xamarin.Forms에서 런타임에 XAML 로드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)

합니다 [ `Xamarin.Forms.Xaml` ](xref:Xamarin.Forms.Xaml) 네임 스페이스는 두 개의 포함 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 를 로드 하려면 사용할 수 있으며 런타임에 XAML을 구문 분석 하는 확장 메서드.

## <a name="background"></a>배경

Xamarin.Forms XAML 클래스를 생성 합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 직접 메서드는 없습니다. 이 클래스는 XAML에 대 한 코드 숨김 파일 때문에 발생 합니다 `InitializeComponent` 해당 생성자에서 메서드:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio는 XAML 파일을 포함 하는 프로젝트가 빌드될 때 생성 하려면 XAML 파일 구문 분석을 C# 코드 파일 (예를 들어 **MainPage.xaml.g.cs**)의 정의 포함 하는 `InitializeComponent` 메서드:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

합니다 `InitializeComponent` 메서드 호출을 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) .NET Standard 라이브러리에서 XAML 파일 (또는 해당 컴파일된 이진 파일)를 추출 하는 방법입니다. 압축을 푼 다음 모든 XAML 파일에 정의 된 개체를 초기화, 부모-자식 관계에서 모두 함께 연결, XAML 파일에서 설정 하는 이벤트에는 코드에 정의 된 이벤트 처리기를 연결 하 고 결과 트리의 내용으로 개체를 설정 합니다 페이지입니다.

## <a name="loading-xaml-at-runtime"></a>런타임 시 XAML 로드

합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드는 `public`, 따라서를 로드 하려면 Xamarin.Forms 응용 프로그램에서 호출할 수 있습니다 하 고 런타임에 XAML을 구문 분석 합니다. 이 XAML에서 필요한 보기를 만들고 응용 프로그램에서 표시할 웹 서비스에서 XAML을 다운로드 하는 응용 프로그램과 같은 시나리오를 허용 합니다.

> [!WARNING]
> 런타임에 XAML을 로드는 상당한 성능 비용으로 있으며 일반적으로 피해 야 합니다.

다음 코드 예제에서는 단순한 사용을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

이 예제는 [ `Button` ](xref:Xamarin.Forms.Button) 사용의 인스턴스를 만듭니다 [ `Text` ](xref:Xamarin.Forms.Button.Text) 에 정의 된 속성 값을 설정 하는 XAML에서는 `string`. 합니다 `Button` 에 추가 되는 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) 페이지에 대 한 XAML에 정의 된 합니다.

> [!NOTE]
> 합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 확장 메서드를 사용 하면 제네릭 형식 인수를 지정 해야 합니다. 그러나 그것이 형식 인수를 지정할 필요가 거의 없습니다 것에서 작동 하는 인스턴스의 형식에서 유추 합니다.

합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드를 사용 하 여 다음 예제에서는 값 모든 XAML 확장에 사용할 수는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 를 이동한 후:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>요소에 액세스

사용 하 여 런타임에 XAML을 로드 합니다 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드는 런타임 개체 이름을 지정 하는 XAML 요소에 대 한 강력한 형식의 액세스를 허용 하지 않습니다 (사용 하 여 `x:Name`). 그러나 이러한 XAML 요소를 검색할 수 있습니다 사용 하는 [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 메서드를 한 다음 필요에 따라 액세스:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

이 예에 대 한 XAML [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 팽창 됩니다. 이 XAML이 포함 되어 있습니다를 [ `Label` ](xref:Xamarin.Forms.Label) 라는 `monkeyName`를 사용 하 여 검색 되는 [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 메서드를 사용할 수 있기 전에 [ `Text` ](xref:Xamarin.Forms.Label.Text) 속성이 설정 됩니다.

## <a name="related-links"></a>관련 링크

- [LoadRuntimeXAML (샘플)](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)
