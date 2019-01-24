---
title: Xamarin.Forms ControlTemplate에서 바인딩
description: 템플릿 바인딩을 사용하면 컨트롤 템플릿의 컨트롤을 공용 속성에 데이터 바인딩할 수 있기 때문에 컨트롤 템플릿의 컨트롤에 대한 속성 값을 쉽게 변경할 수 있습니다. 이 문서에서는 템플릿 바인딩을 사용하여 컨트롤 템플릿에서 데이터 바인딩을 수행하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 49f66164c707f91f298b2e5cb09b35f1767186cf
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051583"
---
# <a name="binding-from-a-xamarinforms-controltemplate"></a>Xamarin.Forms ControlTemplate에서 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)

_템플릿 바인딩을 사용하면 컨트롤 템플릿의 컨트롤을 공용 속성에 데이터 바인딩할 수 있기 때문에 컨트롤 템플릿의 컨트롤에 대한 속성 값을 쉽게 변경할 수 있습니다. 이 문서에서는 템플릿 바인딩을 사용하여 컨트롤 템플릿에서 데이터 바인딩을 수행하는 방법을 보여줍니다._

[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)은 컨트롤 템플릿을 소유한 *대상* 뷰의 부모의 바인딩 가능한 속성에 컨트롤 템플릿의 컨트롤 속성을 바인딩하는 데 사용됩니다. 예를 들어 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 내부의 [`Label`](xref:Xamarin.Forms.Label) 인스턴스에 따라 표시된 텍스트를 정의하는 대신 템플릿 바인딩을 사용하여 [`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성을 표시될 텍스트를 정의하는 바인딩 가능한 속성에 바인딩할 수 있습니다.

[`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)은 `TemplateBinding`의 *원본*이 항상 컨트롤 템플릿을 소유하는 *대상* 뷰의 부모로 자동 설정되는 것을 제외하고는 기존 [`Binding`](xref:Xamarin.Forms.Binding)과 비슷합니다. 그러나 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) 외부의 `TemplateBinding`을 사용하는 것은 지원되지 않습니다.

## <a name="creating-a-templatebinding-in-xaml"></a>XAML에서 TemplateBinding 만들기

다음 코드 예제에 설명된 대로 XAML에서 [`TemplateBinding`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) 태그 확장을 사용하여 [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)을 만듭니다.

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

[`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성을 정적 텍스트로 설정하기보다 해당 속성은 템플릿 바인딩을 사용하여 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)을 소유하는 *대상* 뷰의 부모의 바인딩 가능한 속성에 바인딩할 수 있습니다. 그러나 템플릿 바인딩은 `HeaderText` 및 `FooterText`보다는 `Parent.HeaderText` 및 `Parent.FooterText`에 바인딩됩니다. 다음 코드 예제에 설명된 것처럼 이 예제에서는 바인딩 가능한 속성을 부모보다는 *대상* 뷰의 상위 부모에서 정의하기 때문입니다.

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

템플릿 바인딩의 *원본*은 항상 컨트롤 템플릿을 소유하는 *대상* 뷰의 부모로 자동 설정되며 여기서는 [`ContentView`](xref:Xamarin.Forms.ContentView) 인스턴스입니다. 템플릿 바인딩은 [`Parent`](xref:Xamarin.Forms.Element.Parent) 속성을 사용하여 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 인스턴스인 `ContentView` 인스턴스의 부모 요소를 반환합니다. 따라서 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)의 [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)을 사용하여 `Parent.HeaderText`에 바인딩하고 `Parent.FooterText`는 다음 코드 예제에 표시된 것처럼 `ContentPage`에서 정의된 바인딩 가능한 속성을 찾습니다.

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

이로 인해 결국 다음 스크린샷에 표시된 모양이 됩니다.

![](template-binding-images/teal-theme.png "템플릿 바인딩을 사용하는 청록색 컨트롤 템플릿")

## <a name="creating-a-templatebinding-in-c35"></a>C&#35;에서 TemplateBinding 만들기

다음 코드 예제에 표시된 것처럼 `TemplateBinding` 생성자를 사용하여 [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)을 만듭니다.

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

[`Label.Text`](xref:Xamarin.Forms.Label.Text) 속성을 정적 텍스트로 설정하기보다 해당 속성은 템플릿 바인딩을 사용하여 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)을 소유하는 *대상* 뷰의 부모의 바인딩 가능한 속성에 바인딩할 수 있습니다. [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) 메서드를 두 번째 매개 변수로 [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding) 인스턴스를 지정하여 템플릿 바인딩을 만듭니다. 다음 코드 예제에 설명된 것처럼 바인딩 가능한 속성은 부모보다는 *대상* 뷰의 상위 부모에서 정의되므로 템플릿 바인딩은 `Parent.HeaderText` 및 `Parent.FooterText`에 바인딩됩니다.

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

바인딩 가능한 속성은 앞에서 설명한 대로 `ContentPage`에서 정의됩니다.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>ViewModel 속성에 BindableProperty 바인딩

앞서 설명한 대로 [`TemplateBinding`](xref:Xamarin.Forms.TemplateBinding)은 컨트롤 템플릿을 소유한 *대상* 뷰의 부모의 바인딩 가능한 속성에 컨트롤 템플릿의 컨트롤 속성을 바인딩하는 데 사용됩니다. 결국 이러한 바인딩 가능한 속성은 ViewModels의 속성에 바인딩될 수 있습니다.

다음 코드 예제에서는 ViewModel에서 두 속성을 정의합니다.

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` 및 `FooterText` ViewModel 속성은 다음 XAML 코드 예제에 표시된 대로 바인딩될 수 있습니다.

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

`HeaderText` 및 `FooterText` 바인딩 가능한 속성은 `HomePageViewModel` 클래스 인스턴스로 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)를 설정했기 때문에 `HomePageViewModel.HeaderText` 및 `HomePageViewModel.FooterText` 속성에 바인딩됩니다. 전반적으로 이로 인해 결국 ViewModel 속성에 바인딩하는 [`ContentPage`](xref:Xamarin.Forms.ContentPage)의 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 인스턴스에 바인딩된 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)의 컨트롤 속성이 됩니다.

동등한 C# 코드는 다음 코드 예제와 같습니다.

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

뷰 모델에 직접 바인딩할 수도 있으므로 Parent.BindingContext._PropertyName_ 등에 컨트롤 템플릿을 바인딩하여 `ContentPage`에서 `HeaderText` 및 `FooterText`에 대한 `BindableProperty`을 선언하지 않아도 됩니다.

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

ViewModels에 데이터 바인딩에 대한 자세한 내용은 [데이터 바인딩에서 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 템플릿 바인딩을 사용하여 컨트롤 템플릿에서 데이터 바인딩을 수행하는 방법을 보여줍니다. 템플릿 바인딩을 사용하면 컨트롤 템플릿의 컨트롤을 공용 속성에 데이터 바인딩할 수 있기 때문에 컨트롤 템플릿의 컨트롤에 대한 속성 값을 쉽게 변경할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [데이터 바인딩에서 MVVM까지](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [템플릿 바인딩(샘플)을 사용한 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [템플릿 바인딩 및 ViewModel(샘플)을 사용한 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](xref:Xamarin.Forms.TemplateBinding)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentView](xref:Xamarin.Forms.ContentView)
