---
title: 런타임에 XAML 로드Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d750aa84a48ad4c8015a619d819134cefc63c3d9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139375"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>런타임에 XAML 로드Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

[`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml)네임 스페이스에는 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 런타임에 XAML을 로드 하 고 구문 분석 하는 데 사용할 수 있는 두 가지 확장 메서드가 포함 되어 있습니다.

## <a name="background"></a>배경

Xamarin.FormsXAML 클래스를 생성할 때 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 메서드를 간접적으로 호출 합니다. 이는 XAML 클래스의 코드 숨김이 생성자에서 메서드를 호출 하기 때문에 발생 합니다 `InitializeComponent` .

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio에서 XAML 파일이 포함 된 프로젝트를 빌드하는 경우 XAML 파일을 구문 분석 하 여 메서드의 정의가 포함 된 c # 코드 파일 (예: **MainPage.xaml.g.cs**)을 생성 합니다 `InitializeComponent` .

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent`메서드는 메서드를 호출 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 하 여 .NET Standard 라이브러리에서 XAML 파일 (또는 컴파일된 이진)을 추출 합니다. 추출 후에는 XAML 파일에 정의 된 모든 개체를 초기화 하 고,이를 모두 부모-자식 관계에서 연결 하 고, 코드에 정의 된 이벤트 처리기를 XAML 파일에 설정 된 이벤트에 연결 하 고, 개체의 결과 트리를 페이지의 콘텐츠로 설정 합니다.

## <a name="loading-xaml-at-runtime"></a>런타임에 XAML 로드

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)메서드는 이므로 `public` Xamarin.Forms 런타임에 응용 프로그램에서 호출 하 여 XAML을 로드 하 고 구문 분석할 수 있습니다. 이를 통해 웹 서비스에서 XAML을 다운로드 하 고 XAML에서 필요한 뷰를 만든 다음 응용 프로그램에 표시 하는 등의 시나리오를 수행할 수 있습니다.

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

이 예제에서는 [`Button`](xref:Xamarin.Forms.Button) [`Text`](xref:Xamarin.Forms.Button.Text) 에 정의 된 XAML에서 속성 값을 설정 하 여 인스턴스를 만듭니다 `string` . `Button`그런 다음이 [`StackLayout`](xref:Xamarin.Forms.StackLayout) 페이지의 XAML에 정의 된에 추가 됩니다.

> [!NOTE]
> [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)확장 메서드를 사용 하면 제네릭 형식 인수를 지정할 수 있습니다. 그러나 형식 인수를 지정 하는 것은 거의 필요 하지 않습니다 .이 인수는 작동 하는 인스턴스의 형식에서 유추 됩니다.

메서드를 사용 하 여 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 모든 XAML을 확장 하 고 다음 예제를 않아서 하 여 탐색할 수 있습니다 [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>요소 액세스

메서드를 사용 하 여 런타임에 XAML을 로드 하는 경우에는를 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 사용 하 여 지정 된 런타임 개체 이름을 가진 xaml 요소에 대해 강력한 형식의 액세스를 허용 하지 않습니다 `x:Name` . 그러나 이러한 XAML 요소는 메서드를 사용 하 여 검색 한 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 다음 필요에 따라 액세스할 수 있습니다.

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

이 예제에서에 대 한 XAML은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 팽창입니다. 이 XAML은 [`Label`](xref:Xamarin.Forms.Label) 속성을 `monkeyName` [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) 설정 하기 전에 메서드를 사용 하 여 검색 되는 라는를 포함 [`Text`](xref:Xamarin.Forms.Label.Text) 합니다.

## <a name="related-links"></a>관련 링크

- [LoadRuntimeXAML (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
