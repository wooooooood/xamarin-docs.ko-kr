---
제목: " Xamarin.Forms 텍스트 스타일" 설명: "이 문서에서는 응용 프로그램에서 텍스트의 스타일을 지정 하는 방법을 설명 Xamarin.Forms 합니다. 스타일은 한 번 정의 하 여 여러 뷰에서 사용할 수 있지만 스타일은 한 형식의 뷰에서만 사용할 수 있습니다. "
assetid: 57C0CFD6-A568-46B8-ADA1-BF25681893CF: xamarin-forms author: davidbritch: dabritch:: 05/22/2017-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-text-styles"></a>Xamarin.Forms텍스트 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.ios에서 텍스트 스타일 지정_

스타일을 사용 하 여 레이블, 항목 및 편집기의 모양을 조정할 수 있습니다. 스타일은 한 번 정의 하 여 여러 뷰에서 사용할 수 있지만 스타일은 한 형식의 뷰에서만 사용할 수 있습니다.
스타일에는를 지정 하 `Key` 고 특정 컨트롤의 속성을 사용 하 여 선택적으로 적용할 수 있습니다 `Style` .

## <a name="built-in-styles"></a>기본 제공 스타일

Xamarin.Forms에는 일반적인 시나리오에 대 한 여러 가지 [기본 제공](xref:Xamarin.Forms.Device.Styles) 스타일이 포함 되어 있습니다.

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

기본 제공 스타일 중 하나를 적용 하려면 태그 확장을 사용 `DynamicResource` 하 여 스타일을 지정 합니다.

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C #에서 기본 제공 스타일은 다음에서 선택 됩니다 `Device.Styles` .

```csharp
label.Style = Device.Styles.TitleStyle;
```

![장치 스타일 예제](styles-images/builtinstyles.png)

## <a name="custom-styles"></a>사용자 지정 스타일

스타일은 setter 및 setter로 구성 되며 속성 및 속성이 설정 될 값으로 구성 됩니다.

C #에서 크기가 30 인 빨강 텍스트가 있는 레이블에 대 한 사용자 지정 스타일은 다음과 같이 정의 됩니다.

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

XAML에서:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

리소스 (모든 스타일 포함)는 `ContentPage.Resources` 보다 친숙 한 요소의 형제 내에서 정의 됩니다 `ContentPage.Content` .

![사용자 지정 스타일 예제](styles-images/customstyle.png)

## <a name="applying-styles"></a>스타일 적용

스타일을 만든 후에는와 일치 하는 모든 뷰에 적용할 수 있습니다 `TargetType` .

XAML에서 사용자 지정 스타일은 원하는 스타일을 참조 하는 `Style` 태그 확장과 함께 속성을 제공 하 여 뷰에 적용 됩니다 `StaticResource` .

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

C #에서 스타일은 뷰에 직접 적용 하거나 페이지의에 추가 하 고 검색할 수 있습니다 `ResourceDictionary` . 직접 추가 하려면:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

페이지의를 추가 하 고 검색 하려면 `ResourceDictionary` :

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

기본 제공 스타일은 내게 필요한 옵션 설정에 응답 해야 하기 때문에 다르게 적용 됩니다. XAML에서 기본 제공 스타일을 적용 하려면 `DynamicResource` 태그 확장이 사용 됩니다.

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

C #에서 기본 제공 스타일은 다음에서 선택 됩니다 `Device.Styles` .

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>접근성

기본 제공 스타일은 접근성 기본 설정을 보다 쉽게 적용할 수 있도록 하기 위해 존재 합니다. 기본 제공 스타일을 사용 하는 경우 사용자가 접근성 기본 설정을 적절 하 게 설정 하면 글꼴 크기가 자동으로 증가 합니다.

내게 필요한 옵션 설정을 사용 하도록 설정 하 고 사용 하지 않도록 설정 하 여 기본 제공 스타일로 스타일 지정 된 보기의 동일한 페이지에 대 한 다음 예를 살펴보겠습니다.

사용 안 함:

![내게 필요한 옵션을 사용 하지 않는 장치 스타일](styles-images/pre-access.png)

사용:

![내게 필요한 옵션 기능이 설정 된 장치 스타일](styles-images/post-access.png)

내게 필요한 옵션을 보장 하기 위해 기본 제공 스타일을 앱 내에서 텍스트 관련 스타일의 기반으로 사용 하 고 스타일을 일관 되 게 사용 하 고 있는지 확인 합니다. 일반적으로 스타일을 확장 하 고 사용 하는 방법에 대 한 자세한 내용은 [스타일](~/xamarin-forms/user-interface/styles/index.md) 을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [을 사용 하 여 Mobile Apps 만들기 Xamarin.Forms , 12 장](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [스타일](~/xamarin-forms/user-interface/styles/index.md)
- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Style](xref:Xamarin.Forms.Style)
