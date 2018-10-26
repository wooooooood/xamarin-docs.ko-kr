---
title: Xamarin.Forms ControlTemplate에서 바인딩
description: 쉽게 변경 될 컨트롤 템플릿의 컨트롤에에서 대 한 속성 값을 사용 하도록 설정 하면 public 속성에 데이터를 컨트롤 템플릿의 컨트롤에에서 바인딩할 템플릿 바인딩을 허용 합니다. 이 문서에서는 템플릿 바인딩을 사용 하 여 데이터 바인딩 컨트롤 템플릿에서 수행 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 13730dce5d4698085abe10cb93da5ba50b87ab01
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106431"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms ControlTemplate에서 바인딩

_쉽게 변경 될 컨트롤 템플릿의 컨트롤에에서 대 한 속성 값을 사용 하도록 설정 하면 public 속성에 데이터를 컨트롤 템플릿의 컨트롤에에서 바인딩할 템플릿 바인딩을 허용 합니다. 이 문서에서는 템플릿 바인딩을 사용 하 여 데이터 바인딩 컨트롤 템플릿에서 수행 하는 방법을 보여 줍니다._

A [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 바인딩 가능한 속성의 부모에 컨트롤 템플릿의 컨트롤의 속성을 바인딩하는 데는 *대상* 컨트롤 템플릿을 소유 하는 보기입니다. 표시 되는 텍스트를 정의 하는 대신에 예를 들어 [ `Label` ](xref:Xamarin.Forms.Label) 내에서 인스턴스를 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate), 템플릿 바인딩을 사용 하 여 바인딩할 수 없습니다는 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 속성을 표시할 텍스트를 정의 하는 바인딩 가능한 속성입니다.

[ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 기존 비슷합니다 [ `Binding` ](xref:Xamarin.Forms.Binding)한다는 점을 제외 하는 *원본* 의 `TemplateBinding` 부모에 항상 자동으로 설정 됩니다는 *대상* 컨트롤 템플릿을 소유 하는 보기입니다. 그러나 사용 하 여는 `TemplateBinding` 외부에 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 지원 되지 않습니다.

## <a name="creating-a-templatebinding-in-xaml"></a>TemplateBinding XAML에서 만들기

XAML에서 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 사용 하 여 만들어집니다 합니다 [ `TemplateBinding` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) 다음 코드 예제 에서처럼 태그 확장:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

설정 하지 않고 합니다 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 정적 텍스트 속성을 속성의 부모에 바인딩 가능한 속성에 바인딩할 템플릿 바인딩을 사용할 수는 *대상* 소유 하는 뷰는 [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). 그러나 템플릿 바인딩을 바인딩할 `Parent.HeaderText` 하 고 `Parent.FooterText`를 대신 `HeaderText` 및 `FooterText`합니다. 이 예제에서는 바인딩 가능한 속성의 상위 부모에서 정의 되기 때문에 이것이 합니다 *대상* 를 보려면 부모 대신 다음 코드 예제에서 설명한 것 처럼:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

합니다 *소스* 템플릿의 바인딩이 항상 자동으로 설정 됩니다의 부모를 *대상* 여기에 컨트롤 템플릿에 소유 하는 뷰를 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 인스턴스입니다. 바인딩에서 사용 하 여 템플릿을 [ `Parent` ](xref:Xamarin.Forms.Element.Parent) 속성의 부모 요소를 반환 하는 `ContentView` 인스턴스를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 인스턴스. 따라서 사용 하 여는 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 에 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 바인딩할 `Parent.HeaderText` 및 `Parent.FooterText` 에 정의 된 바인딩 가능한 속성을 찾습니다는 `ContentPage`,으로 다음 코드 예제에서 보여 줍니다.

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

이 인해 다음 스크린샷에 표시 된 모양:

![](template-binding-images/teal-theme.png "템플릿 바인딩을 사용 하 여 청록색 컨트롤 템플릿")

## <a name="creating-a-templatebinding-in-c35"></a>TemplateBinding C에서 만들기&#35;

C#을 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 사용 하 여 만들어집니다는 `TemplateBinding` 생성자에 다음 코드 예제에서 설명한 것 처럼:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

설정 하지 않고 합니다 [ `Label.Text` ](xref:Xamarin.Forms.Label.Text) 정적 텍스트 속성을 속성의 부모에 바인딩 가능한 속성에 바인딩할 템플릿 바인딩을 사용할 수는 *대상* 소유 하는 뷰는 [ `ControlTemplate`](xref:Xamarin.Forms.ControlTemplate). 템플릿 바인딩을 사용 하 여 만들를 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드를 지정 하는 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 두 번째 매개 변수로 인스턴스. 템플릿 바인딩이 바인딩되는 참고 `Parent.HeaderText` 및 `Parent.FooterText`부모의에 바인딩 가능한 속성 정의 되기 때문에, 합니다 *대상* 를 보려면 부모 대신 다음 코드 예제에서 설명한 것 처럼:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

바인딩 가능한 속성에 정의 된는 `ContentPage`앞부분에서 설명한 대로 합니다.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>BindableProperty ViewModel 속성에 바인딩

이전에 설명한 대로 [ `TemplateBinding` ](xref:Xamarin.Forms.TemplateBinding) 바인딩 가능한 속성의 부모에 컨트롤 템플릿의 컨트롤의 속성에 바인딩합니다 합니다 *대상* 컨트롤 템플릿을 소유 하는 보기입니다. 따라서 이러한 바인딩 가능한 속성 Viewmodel의 속성에 바인딩할 수 있습니다.

다음 코드 예제는 ViewModel에 두 속성을 정의합니다.

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

합니다 `HeaderText` 고 `FooterText` ViewModel 속성을 다음 XAML 코드 예제에 표시 된 대로 바인딩할 수 있습니다.

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText` 및 `FooterText` 바인딩 가능한 속성에 바인딩된를 `HomePageViewModel.HeaderText` 및 `HomePageViewModel.FooterText` 속성을 설정으로 인해를 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 인스턴스에 `HomePageViewModel` 클래스. 전반적으로이 인해 컨트롤 속성에는 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) 바인딩되 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 인스턴스를 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), 다시 바인딩하는 ViewModel 속성입니다.

해당하는 C# 코드가 다음 코드 예제에 표시됩니다.

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

바인딩할 수도 있습니다 보기 모델 속성에 직접 선언 하지 않아도 되도록 `BindableProperty`에 대 한 `HeaderText` 및 `FooterText` 에 `ContentPage`, 컨트롤 템플릿 Parent.BindingContext 바인딩하여. _PropertyName_ 예를 들어:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.BindingContext.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

ViewModels 데이터 바인딩에 대 한 자세한 내용은 참조 하세요. [에서 데이터에 대 한 바인딩을 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.

## <a name="summary"></a>요약

이 문서 컨트롤 템플릿에서 데이터 바인딩을 수행 하려면 템플릿 바인딩을 사용 하 여 보여 줍니다. 쉽게 변경 될 컨트롤 템플릿의 컨트롤에에서 대 한 속성 값을 사용 하도록 설정 하면 public 속성에 데이터를 컨트롤 템플릿의 컨트롤에에서 바인딩할 템플릿 바인딩을 허용 합니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [데이터 바인딩부터 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [템플릿 바인딩 (샘플) 사용 하 여 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [템플릿 바인딩 및 ViewModel (샘플)를 사용 하 여 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
