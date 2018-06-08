---
title: ControlTemplate에서 바인딩
description: 템플릿 바인딩을 쉽게 변경할 수에 대 한 컨트롤 템플릿에 컨트롤에서 속성 값을 사용 하도록 설정 데이터에 컨트롤 템플릿의 컨트롤 공용 속성에 바인딩할 수 있도록 합니다. 이 문서에 컨트롤 서식 파일에서 데이터 바인딩을 수행 하려면 템플릿 바인딩을 사용 하 여 보여줍니다.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b2904d06d0982fb30e9a989f03f22b726b9772e
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847178"
---
# <a name="binding-from-a-controltemplate"></a>ControlTemplate에서 바인딩

_템플릿 바인딩을 쉽게 변경할 수에 대 한 컨트롤 템플릿에 컨트롤에서 속성 값을 사용 하도록 설정 데이터에 컨트롤 템플릿의 컨트롤 공용 속성에 바인딩할 수 있도록 합니다. 이 문서에 컨트롤 서식 파일에서 데이터 바인딩을 수행 하려면 템플릿 바인딩을 사용 하 여 보여줍니다._

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 컨트롤 서식 파일에 바인딩 가능한 속성의 부모에 컨트롤의 속성을 바인딩하는 데 사용 되는 *대상* 컨트롤 서식 파일을 소유 하는 보기입니다. 예를 들어 하 여 표시 되는 텍스트를 정의 하는 대신 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 내 인스턴스는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), 템플릿 바인딩을 사용 하 여 바인딩할 수는 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 속성을 표시할 텍스트를 정의 하는 바인딩 가능한 속성입니다.

A [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 기존의 유사 [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/)한다는 점을 제외 하 고 *소스* 의 `TemplateBinding` 의 부모에 항상 자동으로 설정 됩니다는 *대상* 컨트롤 서식 파일을 소유 하는 보기입니다. 그러나를 사용 하는 것에 주의 `TemplateBinding` 외부에 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 지원 되지 않습니다.

## <a name="creating-a-templatebinding-in-xaml"></a>XAML에서 TemplateBinding 만들기

Xaml에서는 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 사용 하 여 만들어집니다는 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) 태그 확장, 다음 코드 예제에서와 같이:

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

설정 하지 않고는 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 정적 텍스트로 속성, 속성 수 템플릿 바인딩을 사용 하 여 바인딩 가능한 속성의 부모에 바인딩할는 *대상* 담당 하는 보기는 [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). 그러나 템플릿 바인딩을 바인딩할 메모 `Parent.HeaderText` 및 `Parent.FooterText`, 대신 `HeaderText` 및 `FooterText`합니다. 이 예제에서는 바인딩 가능한 속성의 최상위 항목에 정의 된 때문에 이것이 *대상* 를 보려면 부모 하는 대신 다음 코드 예제에서와 같이:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*소스* 서식 파일의 바인딩은 항상으로 자동 설정의 부모는 *대상* 여기 있는 컨트롤 템플릿을 소유 하는 보기는 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 인스턴스입니다. 바인딩에서 사용 하는 서식 파일은 [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) 의 부모 요소를 반환 하는 `ContentView` 인스턴스에 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 인스턴스. 따라서 사용 하는 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 에 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 바인딩할 `Parent.HeaderText` 및 `Parent.FooterText` 에 정의 된 바인딩 가능한 속성을 찾습니다는 `ContentPage`,으로 다음 코드 예제에서 확인할 수 있습니다.

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

다음 스크린샷에 표시 된 모양 결과이 됩니다.

![](template-binding-images/teal-theme.png "템플릿 바인딩을 사용 하 여 청록색 컨트롤 템플릿")

## <a name="creating-a-templatebinding-in-c35"></a>C에서 TemplateBinding 만들기&#35;

C#에서는 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 사용 하 여 만든는 `TemplateBinding` 생성자에 다음 코드 예제에서와 같이:

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

설정 하지 않고는 [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) 정적 텍스트로 속성, 속성 수 템플릿 바인딩을 사용 하 여 바인딩 가능한 속성의 부모에 바인딩할는 *대상* 담당 하는 보기는 [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). 템플릿 바인딩을 사용 하 여 만들는 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) 메서드를 지정 하는 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 두 번째 매개 변수로 인스턴스. 템플릿 바인딩에 바인딩되는 참고 `Parent.HeaderText` 및 `Parent.FooterText`바인딩 가능한 속성의 최상위 항목에 정의 되어 있으므로는 *대상* 를 보려면 부모 하는 대신 다음 코드 예제에서와 같이:

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

바인딩 가능한 속성에 정의 된는 `ContentPage`이전의 개략적인 설명 대로, 합니다.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>BindableProperty ViewModel 속성에 바인딩

듯이 [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) 컨트롤 템플릿 컨트롤의 속성을 바인딩할 수 있는 속성의 부모에 바인딩하는 *대상* 컨트롤 서식 파일을 소유 하는 보기입니다. 차례로 Viewmodel의 속성에 바인딩할 수 있는 이러한 속성을 바인딩할 수 있습니다.

다음 코드 예제는 ViewModel에 두 개의 속성을 정의합니다.

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` 및 `FooterText` ViewModel 속성을 다음 XAML 코드 예제와 같이 바인딩할 수 있습니다.

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

`HeaderText` 및 `FooterText` 바인딩 가능한 속성에 바인딩된는 `HomePageViewModel.HeaderText` 및 `HomePageViewModel.FooterText` 속성 설정으로 인해는 [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 인스턴스에 `HomePageViewModel` 클래스입니다. 전반적으로,이 인해 컨트롤 속성에는 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) 바인딩될 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) 인스턴스를 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)에 다시 바인딩 ViewModel 속성입니다.

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

Viewmodel 데이터 바인딩에 대 한 자세한 내용은 참조 하십시오. [에서 데이터에 대 한 바인딩을 MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)합니다.

## <a name="summary"></a>요약

이 문서 컨트롤 서식 파일에서 데이터 바인딩을 수행 하려면 템플릿 바인딩을 사용 하 여 설명 합니다. 템플릿 바인딩을 쉽게 변경할 수에 대 한 컨트롤 템플릿에 컨트롤에서 속성 값을 사용 하도록 설정 데이터에 컨트롤 템플릿의 컨트롤 공용 속성에 바인딩할 수 있도록 합니다.



## <a name="related-links"></a>관련 링크

- [데이터 바인딩 기본 사항](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [MVVM에 대 한 데이터 바인딩을에서](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [템플릿 바인딩 (샘플)와 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [템플릿 바인딩 ViewModel (샘플)와 간단한 테마](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
