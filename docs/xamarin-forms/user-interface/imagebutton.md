---
title: Xamarin.Forms ImageButton
description: Imagebutton이 이미지를 표시 하 고 탭 또는 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: f97cd3030b865b53b82845ff8941e3f0a10f0320
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050142"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.Forms ImageButton

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)

_Imagebutton이 이미지를 표시 하 고 탭 또는 특정 작업을 수행 하는 응용 프로그램을 지시 하는 클릭에 응답 합니다._

`ImageButton` 결합 보기는 [ `Button` ](xref:Xamarin.Forms.Button) 보기 및 [ `Image` ](xref:Xamarin.Forms.Image) 내용이 단추를 만들려면 뷰는 이미지입니다. 사용자가 `ImageButton` 손가락을 사용 하 여 특정 작업을 수행 하 고 응용 프로그램이를 마우스로 클릭 하거나 탭 할 합니다. 달리 합니다 `Button` 보기는 `ImageButton` 뷰에 텍스트 및 텍스트 모양에 대 한 개념이 없습니다.

> [!NOTE]
> 하는 동안 합니다 [ `Button` ](xref:Xamarin.Forms.Button) 뷰 정의 [ `Image` ](xref:Xamarin.Forms.Button.Image) 속성에서 이미지를 표시할 수 있도록를 `Button`,이 속성은 작은 아이콘을 표시할 때 사용 하기 위한 다음에 `Button` 텍스트입니다.

이 가이드의 코드 예제에서 수행 되는 [FormsGallery 샘플](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)합니다.

## <a name="setting-the-image-source"></a>이미지 원본 설정

`ImageButton` 정의 `Source` 이미지 원본 파일, URI, 리소스 또는 스트림을 사용 하 여 단추에 표시할 이미지를 설정 해야 하는 속성입니다. 다양 한 원본에서 이미지를 로드 하는 방법에 대 한 자세한 내용은 참조 하세요. [xamarin.forms에서 이미지](images.md)합니다.

다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ImageButton` XAML에서:

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

합니다 `Source` 속성에 표시 되는 이미지를 지정 합니다 `ImageButton`합니다. 이 예제에서는 다음 스크린샷에서 결과, 각 플랫폼 프로젝트에서 로드 되는 로컬 파일에 설정 됩니다.

[![기본 ImageButton](imagebutton-images/BasicImageButton.png "기본 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "기본 ImageButton")

기본적으로 `ImageButton` 사각형이 사용 하 여 it 둥근 모퉁이 제공할 수 있지만 `CornerRadius` 속성. 에 대 한 자세한 내용은 `ImageButton` 모양을 참조 [ImageButton 모양을](#imagebutton-appearance)합니다.

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

`ImageButton` 정의 된 `Clicked` 사용자가 누를 때 실행 되는 이벤트를 `ImageButton` 손가락이 나 마우스 포인터를 사용 하 여 합니다. 이벤트의 화면에서 손가락이 나 마우스 단추를 놓으면 발생 합니다 `ImageButton`합니다. 합니다 `ImageButton` 있어야 해당 `IsEnabled` 속성으로 설정 `true` 탭에 응답할 합니다.

다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ImageButton` XAML 핸들에 해당 `Clicked` 이벤트:

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

합니다 `Clicked` 이벤트를 명명 된 이벤트 처리기로 `OnImageButtonClicked` 코드 숨김 파일에 있는:

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

경우는 `ImageButton` 를 탭 할는 `OnImageButtonClicked` 메서드를 실행 합니다. 합니다 `sender` 인수가 `ImageButton` 이 이벤트를 담당 합니다. 에 액세스 하는 데 사용할 수 있습니다는 `ImageButton` 개체 또는 여러 구별할 수 `ImageButton` 공유 하는 동일한 개체 `Clicked` 이벤트입니다.

이 특정 `Clicked` 처리기는 카운터를 증가 하 고 카운터 값 표시를 [ `Label` ](xref:Xamarin.Forms.Label):

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

경우에 따라 응용 프로그램은 특정 상태의 경우 특정 `ImageButton` 클릭은 올바른 작업이 아닙니다. 이러한 경우에는 `ImageButton` 해야 사용할 수 없게 설정 하 여 해당 `IsEnabled` 속성을 `false`입니다.

## <a name="using-the-command-interface"></a>인터페이스를 사용 하 여

응용 프로그램에 응답할 가능성이 `ImageButton` 처리 없이 탭은 `Clicked` 이벤트입니다. 합니다 `ImageButton` 이라는 다른 알림 메커니즘을 구현 합니다 _명령_ 또는 _명령_ 인터페이스입니다. 이 두 속성으로 구성 됩니다.

- `Command` 형식의 [ `ICommand` ](xref:System.Windows.Input.ICommand)에 정의 된 인터페이스는 [ `System.Windows.Input` ](xref:System.Windows.Input) 네임 스페이스입니다.
- `CommandParameter` 형식의 속성 [ `Object` ](xref:System.Object)합니다.

이 방법은 모델-뷰-ViewModel (MVVM) 아키텍처를 구현 하는 경우에 특히 데이터 바인딩 관련 하 여 및에 적합 합니다.

인터페이스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [인터페이스를 사용 하 여](button.md#using-the-command-interface) 에 [단추](button.md) 가이드입니다.

## <a name="pressing-and-releasing-the-imagebutton"></a>Imagebutton이 눌렀다 놓아

외에 합니다 `Clicked` 이벤트 `ImageButton` 도 정의 `Pressed` 및 `Released` 이벤트입니다. 합니다 `Pressed` 이벤트 손가락을 누를 때 발생을 `ImageButton`, 또는 위에 포인터를 사용 하 여 마우스 단추를 누를 `ImageButton`합니다. `Released` 이벤트 손가락이 나 마우스 단추를 놓을 때 발생 합니다. 일반적으로 `Clicked` 이벤트와 동일한 시간에도 발생 합니다 `Released` 손가락이 나 마우스 포인터의 화면에서 슬라이드 하는 경우 있지만 이벤트는 `ImageButton` 반환 되기 전에 `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

이러한 이벤트에 대 한 자세한 내용은 참조 하세요. [키를 눌러 및 단추에서 손을 떼기](button.md#pressing-and-releasing-the-button) 에 [단추](button.md) 가이드입니다.

## <a name="imagebutton-appearance"></a>ImageButton 모양

속성 외에 `ImageButton` 에서 상속 되는 [ `View` ](xref:Xamarin.Forms.View) 클래스 `ImageButton` 도 해당 모양에 영향을 주는 몇 가지 속성을 정의 합니다.:

- `Aspect` 이미지는 표시 영역에 맞게 크기가 조정 하는 방법입니다.
- `BorderColor` 주위의 영역 색인을 `ImageButton`입니다.
- `BorderWidth` 테두리의 너비가입니다.
- `CornerRadius` 모서리 반지름을 `ImageButton`입니다.

합니다 `Aspect` 속성의 멤버 중 하나로 설정할 수 있습니다 합니다 [ `Aspect` ](xref:Xamarin.Forms.Aspect) 열거형:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -이미지를 완전히 정확 하 게 입력 확장 된 `ImageButton`합니다. 이 이미지 왜곡 되지 될 수 있습니다.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -을 채우도록 이미지를 자릅니다는 `ImageButton` 가로 세로 비율을 유지 하면서 합니다.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -레터 박스 (필요한 경우) 이미지 전체 이미지에 맞도록는 `ImageButton`, 위쪽/아래쪽 또는 이미지를 가로 또는 세로 인지에 따라 양쪽에 추가 공백이 있는 합니다. 기본 값을 [ `Aspect` ](xref:Xamarin.Forms.Aspect) 열거형입니다.

> [!NOTE]
> `ImageButton` 클래스에 [ `Margin` ](xref:Xamarin.Forms.View.Margin) 하 고 `Padding` 레이아웃 동작을 제어 하는 속성을 `ImageButton`합니다. 자세한 내용은 [여백 및 안쪽 여백](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)합니다.

## <a name="imagebutton-visual-states"></a>ImageButton 시각적 상태

`ImageButton` 에 `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 시각적으로 변화를 시작 하려면 사용할 수 있는 `ImageButton` 설정 된 사용자가 누를 때.

다음 XAML 예제에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다는 `Pressed` 상태:

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

`Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) 되도록 지정 합니다 `ImageButton` 를 누르면 해당 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 1 0.8은 사용자의 기본 값에서 속성을 변경할 수는. 합니다 `Normal` `VisualState` 되도록 지정 합니다 `ImageButton` 정상 상태의 해당 `Scale` 속성이 1로 설정 됩니다. 따라서 전체적인 효과 때를 `ImageButton` 는 약간 더 작은 및 시기를 재조정는 누른는 `ImageButton` 는 기본 크기를 재조정은 해제 합니다.

시각적 상태에 대 한 자세한 내용은 참조 하세요. [은 Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)합니다.

## <a name="related-links"></a>관련 링크

- [FormsGallery 샘플](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
