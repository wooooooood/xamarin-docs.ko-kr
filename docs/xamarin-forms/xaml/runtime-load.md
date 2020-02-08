---
title: Xamarin.ios에서 런타임에 XAML 로드
description: LoadFromXaml 확장 메서드를 사용 하 여 런타임에 XAML을 로드 하 고 구문 분석할 수 있습니다.
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: 71f510cd37d4bed2a5a6077ed63f748ce9894725
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045067"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Xamarin.ios에서 런타임에 XAML 로드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

[`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) 네임 스페이스에는 런타임에 XAML을 로드 하 고 구문 분석 하는 데 사용할 수 있는 두 가지 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 확장 메서드가 포함 되어 있습니다.

## <a name="background"></a>배경

Xamarin.ios XAML 클래스가 생성 될 때 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드를 간접적으로 호출 합니다. 이는 XAML 클래스의 코드 숨김이 생성자에서 `InitializeComponent` 메서드를 호출 하기 때문에 발생 합니다.

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio에서 XAML 파일을 포함 하는 프로젝트를 빌드하는 경우 XAML 파일을 구문 분석 C# 하 여 `InitializeComponent` 메서드의 정의가 포함 된 코드 파일 (예: **MainPage.xaml.g.cs**)을 생성 합니다.

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent` 메서드는 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드를 호출 하 여 .NET Standard 라이브러리에서 XAML 파일 (또는 컴파일된 이진)을 추출 합니다. 추출 후에는 XAML 파일에 정의 된 모든 개체를 초기화 하 고, 모두 부모-자식 관계에서 연결 하 고, 코드에 정의 된 이벤트 처리기를 XAML 파일에 설정 된 이벤트에 연결 하 고, 개체의 결과 트리를의 콘텐츠로 설정 합니다. 페이지.

## <a name="loading-xaml-at-runtime"></a>런타임에 XAML 로드

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드는 `public`되므로 xamarin.ios 응용 프로그램에서 호출 하 여 런타임에 XAML을 로드 하 고 구문 분석할 수 있습니다. 이를 통해 웹 서비스에서 XAML을 다운로드 하 고 XAML에서 필요한 뷰를 만든 다음 응용 프로그램에 표시 하는 등의 시나리오를 수행할 수 있습니다.

> [!WARNING]
> 런타임에 XAML을 로드 하는 데는 상당한 성능 비용이 발생 하므로 일반적으로 피해 야 합니다.

다음 코드 예제에서는 간단한 사용을 보여 줍니다.

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

이 예제에서는 `string`에 정의 된 XAML에서 [`Text`](xref:Xamarin.Forms.Button.Text) 속성 값을 설정 하 여 [`Button`](xref:Xamarin.Forms.Button) 인스턴스를 만듭니다. 그런 다음 해당 페이지에 대 한 XAML에 정의 된 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 에 `Button` 추가 됩니다.

> [!NOTE]
> [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 확장 메서드를 사용 하 여 제네릭 형식 인수를 지정할 수 있습니다. 그러나 형식 인수를 지정 하는 것은 거의 필요 하지 않습니다 .이 인수는 작동 하는 인스턴스의 형식에서 유추 됩니다.

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드는 XAML을 확장 하는 데 사용할 수 있습니다. 다음 예제에서는 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 않아서 한 다음 탐색 합니다.

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>요소 액세스

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드를 사용 하 여 런타임에 xaml을 로드 하면 `x:Name`를 사용 하 여 지정 된 런타임 개체 이름을 가진 xaml 요소에 대해 강력한 형식의 액세스를 허용 하지 않습니다. 그러나 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 메서드를 사용 하 여 이러한 XAML 요소를 검색 한 다음 필요에 따라 액세스할 수 있습니다.

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

이 예제에서 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 의 XAML은 팽창입니다. 이 XAML에는 [`Text`](xref:Xamarin.Forms.Label.Text) 속성을 설정 하기 전에 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 메서드를 사용 하 여 검색 되는 `monkeyName`라는 [`Label`](xref:Xamarin.Forms.Label) 포함 되어 있습니다.

## <a name="related-links"></a>관련 링크

- [LoadRuntimeXAML (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
