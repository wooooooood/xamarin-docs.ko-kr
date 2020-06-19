---
title: Xamarin.FormsImageButton
description: ImageButton은 이미지를 표시 하 고 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 또는 클릭에 응답 합니다.
ms.prod: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7d81c0ce4dc2a46a840a34cc9084c8f2388a0169
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137646"
---
# <a name="xamarinforms-imagebutton"></a>Xamarin.FormsImageButton

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_ImageButton은 이미지를 표시 하 고 응용 프로그램이 특정 작업을 수행 하도록 지시 하는 탭 또는 클릭에 응답 합니다._

`ImageButton`뷰는 뷰와 뷰를 결합 하 여 해당 [`Button`](xref:Xamarin.Forms.Button) [`Image`](xref:Xamarin.Forms.Image) 내용이 이미지인 단추를 만듭니다. 사용자가 `ImageButton` 손가락으로를 누르거나 마우스로 클릭 하 여 응용 프로그램에서 특정 작업을 수행 하도록 지시할 수 있습니다. 그러나 `Button` 뷰와 달리 보기에는 `ImageButton` 텍스트와 텍스트 모양의 개념이 없습니다.

> [!NOTE]
> [`Button`](xref:Xamarin.Forms.Button)뷰에서 [`Image`](xref:Xamarin.Forms.Button.Image) 이미지를 표시할 수 있는 속성을 정의 하는 동안 `Button` 이 속성은 텍스트 옆에 작은 아이콘을 표시할 때 사용 됩니다 `Button` .

이 가이드의 코드 예제는 [양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)에서 가져온 것입니다.

## <a name="setting-the-image-source"></a>이미지 원본 설정

`ImageButton``Source`이미지 소스를 파일, URI, 리소스 또는 스트림으로 사용 하 여 단추에 표시할 이미지에 설정 해야 하는 속성을 정의 합니다. 다른 원본에서 이미지를 로드 하는 방법에 대 한 자세한 내용은 [의 Xamarin.Forms 이미지 ](images.md)를 참조 하세요.

다음 예제에서는 XAML로를 인스턴스화하는 방법을 보여 줍니다 `ImageButton` .

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

`Source`속성은에 표시 되는 이미지를 지정 합니다 `ImageButton` . 이 예제에서는 각 플랫폼 프로젝트에서 로드 되는 로컬 파일로 설정 되어 다음과 같은 스크린샷을 생성 합니다.

[![기본 ImageButton](imagebutton-images/BasicImageButton.png "기본 ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "기본 ImageButton")

기본적으로는 `ImageButton` 사각형 이지만 속성을 사용 하 여 모퉁이가 둥근 모퉁이를 제공할 수 있습니다 `CornerRadius` . 모양에 대 한 자세한 내용은 `ImageButton` [ImageButton 모양새](#imagebutton-appearance)를 참조 하세요.

> [!NOTE]
> 는 `ImageButton` 애니메이션 gif를 로드할 수 있지만 gif의 첫 번째 프레임만 표시 합니다.

다음 예제에서는 이전 XAML 예제와 기능적으로 동일한 페이지를 만드는 방법을 보여 줍니다.

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

## <a name="handling-imagebutton-clicks"></a>ImageButton 클릭 처리

`ImageButton``Clicked`사용자가 `ImageButton` 손가락 또는 마우스 포인터로를 누를 때 발생 하는 이벤트를 정의 합니다. 이 이벤트는 화면에서 손가락 또는 마우스 단추를 놓을 때 발생 `ImageButton` 합니다. `ImageButton` `IsEnabled` 탭에 응답 하려면의 속성이로 설정 되어 있어야 합니다 `true` .

다음 예제에서는 XAML에서을 인스턴스화하고 해당 이벤트를 처리 하는 방법을 보여 줍니다 `ImageButton` `Clicked` .

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

`Clicked`이벤트는 `OnImageButtonClicked` 코드 숨김이 파일에 있는 라는 이벤트 처리기로 설정 됩니다.

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

`ImageButton`을 탭하면 `OnImageButtonClicked` 메서드가 실행됩니다. `sender`인수는 `ImageButton` 이 이벤트를 담당 합니다. 이를 사용 하 여 개체에 액세스 `ImageButton` 하거나 동일한 이벤트를 공유 하는 여러 개체를 구분할 수 있습니다 `ImageButton` `Clicked` .

이 특정 `Clicked` 처리기는 카운터를 증가 시키고 카운터 값을에 표시 합니다 [`Label`](xref:Xamarin.Forms.Label) .

[![기본 ImageButton 클릭](imagebutton-images/ImageButton.png "기본 ImageButton 클릭")](imagebutton-images/ImageButton-Large.png#lightbox "기본 ImageButton 클릭")

다음 예제에서는 이전 XAML 예제와 기능적으로 동일한 페이지를 만드는 방법을 보여 줍니다.

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

## <a name="disabling-the-imagebutton"></a>ImageButton 사용 안 함

때로는 특정 `ImageButton` 클릭이 올바른 작업이 아닌 특정 상태에 있는 응용 프로그램이 있을 수 있습니다. 이러한 경우에는 `ImageButton` 속성을로 설정 하 여을 사용 하지 않도록 설정 해야 합니다 `IsEnabled` `false` .

## <a name="using-the-command-interface"></a>명령 인터페이스 사용

응용 프로그램이 `ImageButton` 이벤트를 처리 하지 않고 탭에 응답할 수 있습니다 `Clicked` . 는 `ImageButton` _명령 또는 명령_ 인터페이스 라는 대체 알림 메커니즘을 _commanding_ 구현 합니다. 이는 두 가지 속성으로 구성 됩니다.

- `Command`[`ICommand`](xref:System.Windows.Input.ICommand)네임 스페이스에 정의 된 인터페이스인 형식의입니다 [`System.Windows.Input`](xref:System.Windows.Input) .
- `CommandParameter`형식의 속성 [`Object`](xref:System.Object) 입니다.

이 방법은 데이터 바인딩과의 연결에 적합 하며 특히 MVVM (모델-뷰-ViewModel) 아키텍처를 구현 하는 경우에 적합 합니다.

명령 인터페이스를 사용 하는 방법에 대 한 자세한 내용은 [단추](button.md) 가이드에서 [명령 인터페이스 사용](button.md#using-the-command-interface) 을 참조 하세요.

## <a name="pressing-and-releasing-the-imagebutton"></a>ImageButton 누르기 및 해제

`Clicked` 이벤트 외에도 `ImageButton`은 `Pressed` 및 `Released` 이벤트도 정의합니다. `Pressed`이 이벤트는 손가락에서을 누를 때 발생 `ImageButton` 하거나 포인터가 위에 배치 된 상태에서 마우스 단추를 누르면 발생 합니다 `ImageButton` . `Released`이 이벤트는 손가락 또는 마우스 단추를 놓을 때 발생 합니다. 일반적으로 이벤트는 `Clicked` 이벤트와 동시에 발생 `Released` 하지만 손가락 또는 마우스 포인터가의 표면에서 벗어나면 `ImageButton` `Clicked` 이벤트가 발생 하지 않을 수 있습니다.

이러한 이벤트에 대 한 자세한 내용은 [단추](button.md) 가이드에서 [단추 누르기 및 해제](button.md#pressing-and-releasing-the-button) 를 참조 하세요.

## <a name="imagebutton-appearance"></a>ImageButton 모양

클래스에서 상속 되는 속성 외에 `ImageButton` [`View`](xref:Xamarin.Forms.View) 도는 `ImageButton` 모양에 영향을 주는 몇 가지 속성을 정의 합니다.

- `Aspect`표시 영역에 맞게 이미지 크기를 조정 하는 방법입니다.
- `BorderColor`주위의 영역 색입니다 `ImageButton` .
- `BorderWidth`테두리의 너비입니다.
- `CornerRadius`는의 모퉁이 반경입니다 `ImageButton` .

`Aspect`속성은 열거형의 멤버 중 하나로 설정할 수 있습니다 [`Aspect`](xref:Xamarin.Forms.Aspect) .

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)-이미지를 완전히 확장 하 고 정확 하 게 채웁니다 `ImageButton` . 이로 인해 이미지가 왜곡 될 수 있습니다.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)- `ImageButton` 가로 세로 비율을 유지 하면서를 채우도록 이미지를 클리핑 합니다.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)-이미지 (필요한 경우)를 letterboxes 하 여 전체 이미지가에 맞도록 하 고 `ImageButton` 이미지의 너비 또는 높이에 따라 위쪽/아래쪽 또는 옆쪽에 공백이 추가 됩니다. 이 값은 열거형의 기본값입니다 [`Aspect`](xref:Xamarin.Forms.Aspect) .

> [!NOTE]
> `ImageButton`또한 클래스에 [`Margin`](xref:Xamarin.Forms.View.Margin) `Padding` 는의 레이아웃 동작을 제어 하는 및 속성이 있습니다 `ImageButton` . 자세한 내용은 [여백 및 패딩](~/xamarin-forms/user-interface/layouts/margin-and-padding.md)을 참조하세요.

## <a name="imagebutton-visual-states"></a>ImageButton 시각적 상태

`ImageButton`에는 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) `ImageButton` 사용자가 사용 하도록 설정 된 경우 사용자가 누를 때의 시각적 변경을 시작 하는 데 사용할 수 있는가 있습니다.

다음 XAML 예제에서는 상태에 대 한 시각적 상태를 정의 하는 방법을 보여 줍니다 `Pressed` .

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

는를 `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) `ImageButton` 누르면 해당 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성이 기본값 1에서 0.8로 변경 되도록 지정 합니다. 는 `Normal` `VisualState` `ImageButton` 가 정상 상태 이면 해당 `Scale` 속성은 1로 설정 되도록 지정 합니다. 따라서 전반적인 효과는를 `ImageButton` 누를 때 약간 더 재조정, `ImageButton` 가 릴리스되면이는 기본 크기로 재조정는 것입니다.

시각적 상태에 대 한 자세한 내용은 [ Xamarin.Forms 시각적 상태 관리자](~/xamarin-forms/user-interface/visual-state-manager.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [양식 갤러리 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
