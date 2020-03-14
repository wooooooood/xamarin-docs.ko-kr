---
title: Xamarin.Forms ImageButton
description: Imagebutton이 이미지를 표시 하 고 탭 또는 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 7c6647a0299b5ece3caaaa1d322ec1a0efac3557
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305716"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_ImageButton은 이미지를 표시 하 고 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 또는 클릭에 응답 합니다._

`ImageButton` 뷰는 [`Button`](xref:Xamarin.Forms.Button) 뷰와 [`Image`](xref:Xamarin.Forms.Image) 보기를 결합 하 여 해당 콘텐츠가 이미지인 단추를 만듭니다. 사용자가 손가락으로 `ImageButton`를 누르거나 마우스로 클릭 하 여 응용 프로그램에서 특정 작업을 수행 하도록 지시할 수 있습니다. 그러나 `Button` 뷰와 달리 `ImageButton` 보기에는 텍스트와 텍스트 모양의 개념이 없습니다.

> [!NOTE]
> [`Button`](xref:Xamarin.Forms.Button) 뷰에서는 `Button`에 이미지를 표시할 수 있는 [`Image`](xref:Xamarin.Forms.Button.Image) 속성을 정의 하지만이 속성은 `Button` 텍스트 옆에 작은 아이콘을 표시할 때 사용 됩니다.

이 가이드의 코드 예제는 [양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)에서 가져온 것입니다.

## <a name="setting-the-image-source"></a>이미지 원본 설정

`ImageButton`는 이미지 소스가 파일, URI, 리소스 또는 스트림이 되도록 단추에 표시할 이미지에 설정 해야 하는 `Source` 속성을 정의 합니다. 다른 원본에서 이미지를 로드 하는 방법에 대 한 자세한 내용은 [xamarin.ios의 이미지](images.md)를 참조 하세요.

다음 예제에서는 XAML로 `ImageButton`를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Source` 속성은 `ImageButton`에 표시 되는 이미지를 지정 합니다. 이 예제에서는 다음 스크린샷에서 결과, 각 플랫폼 프로젝트에서 로드 되는 로컬 파일에 설정 됩니다.

[![기본 ImageButton](imagebutton-images/BasicImageButton.png "기본 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "기본 ImageButton")

기본적으로 `ImageButton`는 사각형 이지만 `CornerRadius` 속성을 사용 하 여 모퉁이가 둥근 모퉁이를 제공할 수 있습니다. `ImageButton` 모양에 대 한 자세한 내용은 [ImageButton 모양새](#imagebutton-appearance)를 참조 하세요.

> [!NOTE]
> `ImageButton`는 애니메이션 GIF를 로드할 수 있지만 GIF의 첫 번째 프레임만 표시 됩니다.

다음 예제에는 완전히 이지만 이전 하는 XAML 예와 기능적으로 동일 하 게 연결 되는 페이지를 만드는 방법을 보여 줍니다 C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## <a name="handling-imagebutton-clicks"></a>가 ImageButton 처리

`ImageButton` 사용자가 손가락 또는 마우스 포인터로 `ImageButton`를 누를 때 발생 하는 `Clicked` 이벤트를 정의 합니다. 이 이벤트는 `ImageButton`화면에서 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 탭에 응답 하려면 `ImageButton` `IsEnabled` 속성을 `true`으로 설정 해야 합니다.

다음 예제에서는 XAML로 `ImageButton`을 인스턴스화하고 `Clicked` 이벤트를 처리 하는 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Clicked` 이벤트는 코드에 있는 파일에 있는 `OnImageButtonClicked` 라는 이벤트 처리기로 설정 됩니다.

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

`ImageButton` 탭 하면 `OnImageButtonClicked` 메서드가 실행 됩니다. `sender` 인수는이 이벤트를 담당 하는 `ImageButton`입니다. 이를 사용 하 여 `ImageButton` 개체에 액세스 하거나 동일한 `Clicked` 이벤트를 공유 하는 여러 `ImageButton` 개체를 구분할 수 있습니다.

이 특정 `Clicked` 처리기는 카운터를 증가 시키고 카운터 값을 [`Label`](xref:Xamarin.Forms.Label)에 표시 합니다.

[![기본 ImageButton 클릭](imagebutton-images/ImageButton.png "기본 ImageButton 클릭")](imagebutton-images/ImageButton-Large.png#lightbox "기본 ImageButton 클릭")

다음 예제에는 완전히 이지만 이전 하는 XAML 예와 기능적으로 동일 하 게 연결 되는 페이지를 만드는 방법을 보여 줍니다 C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## <a name="disabling-the-imagebutton"></a>Imagebutton이 사용 하지 않도록 설정

때로는 특정 `ImageButton` 클릭이 올바른 작업이 아닌 특정 상태에 있는 응용 프로그램이 있을 수 있습니다. 이러한 경우 `IsEnabled` 속성을 `false`로 설정 하 여 `ImageButton`를 사용 하지 않도록 설정 해야 합니다.

## <a name="using-the-command-interface"></a>인터페이스를 사용 하 여

응용 프로그램이 `Clicked` 이벤트를 처리 하지 않고 `ImageButton` 탭에 응답할 수 있습니다. `ImageButton`는 _명령 또는 명령_ 인터페이스 라는 대체 알림 메커니즘을 _commanding_ 구현 합니다. 이 두 속성으로 구성 됩니다.

- [`System.Windows.Input`](xref:System.Windows.Input) 네임 스페이스에 정의 된 인터페이스인 [`ICommand`](xref:System.Windows.Input.ICommand)형식의 `Command`입니다.
- [`Object`](xref:System.Object)형식의 `CommandParameter` 속성입니다.

이 방법은 모델-뷰-ViewModel (MVVM) 아키텍처를 구현 하는 경우에 특히 데이터 바인딩 관련 하 여 및에 적합 합니다.

명령 인터페이스를 사용 하는 방법에 대 한 자세한 내용은 [단추](button.md) 가이드에서 [명령 인터페이스 사용](button.md#using-the-command-interface) 을 참조 하세요.

## <a name="pressing-and-releasing-the-imagebutton"></a>Imagebutton이 눌렀다 놓아

`Clicked` 이벤트 외에도 `ImageButton` `Pressed` 및 `Released` 이벤트를 정의 합니다. `Pressed` 이벤트는 `ImageButton`에서 손가락을 누를 때 발생 하거나 포인터가 `ImageButton`위에 배치 된 상태에서 마우스 단추를 누르면 발생 합니다. `Released` 이벤트는 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 일반적으로 `Clicked` 이벤트는 `Released` 이벤트와 동시에 발생 하지만 손가락 또는 마우스 포인터를 해제 하기 전에 `ImageButton` 화면에서 벗어나면 `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

이러한 이벤트에 대 한 자세한 내용은 [단추](button.md) 가이드에서 [단추 누르기 및 해제](button.md#pressing-and-releasing-the-button) 를 참조 하세요.

## <a name="imagebutton-appearance"></a>ImageButton 모양

[`View`](xref:Xamarin.Forms.View) 클래스에서 상속 `ImageButton`는 속성 외에도 `ImageButton`는 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- `Aspect` 표시 영역에 맞게 이미지 크기를 조정 하는 방법입니다.
- `BorderColor` `ImageButton`주변 영역의 색입니다.
- `BorderWidth` 테두리의 너비입니다.
- `CornerRadius`은 `ImageButton`의 모퉁이 반경입니다.

`Aspect` 속성은 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거형의 멤버 중 하나로 설정할 수 있습니다.

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -이미지를 완전히 확장 하 고 정확 하 게 `ImageButton`를 채웁니다. 이 이미지 왜곡 되지 될 수 있습니다.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -가로 세로 비율을 유지 하면서 `ImageButton`를 채우도록 이미지를 클리핑 합니다.
- 이미지의 너비 또는 높이에 따라 위쪽/아래쪽 또는 옆쪽에 공백이 추가 된 상태로 전체 이미지가 `ImageButton`에 letterboxes 이미지를 [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) 합니다 (필요한 경우). [`Aspect`](xref:Xamarin.Forms.Aspect) 열거형의 기본값입니다.

> [!NOTE]
> `ImageButton` 클래스에는 `ImageButton`의 레이아웃 동작을 제어 하는 [`Margin`](xref:Xamarin.Forms.View.Margin) 및 `Padding` 속성도 있습니다. 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

## <a name="imagebutton-visual-states"></a>ImageButton 시각적 상태

`ImageButton`에는 사용 하도록 설정 된 경우 사용자가 이동할 때 `ImageButton`에 대 한 시각적 변경을 시작 하는 데 사용할 수 있는 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) 있습니다.

다음 XAML 예제에서는 `Pressed` 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다.

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

`Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) `ImageButton`를 누를 때 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성이 기본값 1에서 0.8로 변경 되도록 지정 합니다. `Normal` `VisualState`는 `ImageButton` 정상 상태 이면 해당 `Scale` 속성이 1로 설정 되도록 지정 합니다. 따라서 전반적인 효과는 `ImageButton`을 누를 때 약간 더 작은 것으로 재조정, `ImageButton` 해제 될 때 기본 크기로 재조정 됩니다.

시각적 상태에 대 한 자세한 내용은 [Xamarin.ios 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
