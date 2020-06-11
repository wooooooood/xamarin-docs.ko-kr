---
제목: "응용 프로그램에서 시스템 테마 변경 내용에 대응 Xamarin.Forms " 설명: " Xamarin.Forms 응용 프로그램이 onapptheme 유형 및 DynamicResource 태그 확장을 사용 하 여 운영 체제 테마 변경 내용에 응답할 수 있습니다."
assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D: xamarin: xamarin. 기술: xamarin 양식 작성자: davidbritch: dabritch. 날짜: 04/22/2020 안 함: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>응용 프로그램의 시스템 테마 변경 내용에 응답 Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

장치에는 일반적으로 운영 체제 수준에서 설정할 수 있는 다양 한 모양 기본 설정 집합을 참조 하는 밝은 테마 및 어두운 테마가 포함 됩니다. 응용 프로그램은 이러한 시스템 테마를 준수 하 고 시스템 테마가 변경 될 때 즉시 응답 해야 합니다.

시스템 테마는 장치 구성에 따라 다양 한 이유로 변경 될 수 있습니다. 여기에는 사용자가 명시적으로 변경 하는 시스템 테마, 하루 중 시간으로 변경 된 시스템 테마 및 낮은 광원 등의 환경적 요소로 인 한 변경 내용이 포함 됩니다.

Xamarin.Forms응용 프로그램은 `AppThemeColor` 클래스, `OnAppTheme<T>` 클래스 및 태그 확장을 사용 하 여 리소스를 정의 하 여 시스템 테마 변경에 응답할 수 있습니다 `OnAppTheme` . 이러한 리소스는 태그 확장과 함께 사용 해야 합니다 `DynamicResource` .

> [!IMPORTANT]
> 시스템 테마 변경에 대 한 응답은 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `AppTheme_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.

Xamarin.Forms시스템 테마 변경에 응답 하려면에 대해 다음과 같은 요구 사항을 충족 해야 합니다.

- Xamarin.Forms4.6 이상.
- iOS 13 이상
- Android 10 (API 29) 이상.
- UWP 빌드 14393 이상.

다음 스크린샷에는 iOS 및 Android에서 밝은 시스템 테마와 어두운 시스템 테마에 대 한 테마 페이지가 표시 됩니다.

[![IOS 및 Android](system-theme-changes-images/main-page-both-themes.png "테마가 적용 된 앱의 기본 페이지")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 기본 페이지") 
 에서 테마가 적용 된 앱의 기본 페이지 스크린샷 [ ![IOS 및 Android에서 테마가 적용 된 앱의 세부 정보 페이지 스크린샷](system-theme-changes-images/detail-page-both-themes.png "테마가 적용 된 앱의 세부 정보 페이지")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 세부 정보 페이지")

## <a name="define-and-consume-theme-resources"></a>테마 리소스 정의 및 사용

밝은 테마와 어두운 테마에 대 한 리소스는 `AppThemeColor` 클래스, `OnAppTheme<T>` 클래스 및 태그 확장을 사용 하 여 정의할 수 있습니다 `OnAppTheme` . 각 접근 방식에서는 현재 시스템 테마의 값에 따라 이러한 리소스가 자동으로 적용 됩니다. 또한 이러한 리소스를 사용 하는 개체는 앱이 실행 되는 동안 시스템 테마가 변경 되는 경우 자동으로 업데이트 됩니다.

### <a name="appthemecolor"></a>AppThemeColor

`AppThemeColor`클래스는 [`Color`](xref:Xamarin.Forms.Color) 밝은 시스템 테마와 어두운 시스템 테마에 대 한 리소스를 정의 하는 데 사용 됩니다. `AppThemeColor`리소스는에 정의 되어야 합니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) .

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

각 `AppThemeColor` 리소스 `x:Key` 에는의 설명 키를 제공 하는 특성이 있어야 합니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 및 속성의 값 `Light` 은 `Dark` 개체 여야 합니다 [`Color`](xref:Xamarin.Forms.Color) . 또한 속성을 사용 하 여 `Default` `Color` 기본적으로 사용 되는 개체에서 사용 하도록 설정할 수 있습니다.

`AppThemeColor`리소스는 인라인으로 사용 될 수 있습니다.

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

또는 `AppThemeColor` 암시적 또는 명시적 개체를 사용 하 여 리소스를 사용할 수 있습니다 [`Style`](xref:Xamarin.Forms.Style) .

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor`리소스는 태그 확장과 함께 사용 해야 합니다 `DynamicResource` . 이렇게 하면 시스템 테마가 변경 될 때 사용 하는 개체의 모양이 업데이트 됩니다.

### <a name="onappthemelttgt"></a>OnAppTheme &lt; T&gt;

`OnAppTheme<T>`클래스는 밝은 시스템 테마와 어두운 시스템 테마에 대 한 모든 형식의 리소스를 정의 하는 데 사용 됩니다. `OnAppTheme<T>`[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) `T` 특성 값으로 지정 된 인수를 사용 하 여 리소스를에 정의 해야 합니다 `x:TypeArguments` .

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

각 `OnAppTheme<T>` 리소스 `x:Key` 에는의 설명 키를 제공 하는 특성이 있어야 합니다 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . 및 속성의 값 `Light` 은 `Dark` 특성으로 정의 된 형식의 개체 여야 합니다 `x:TypeArguments` . 또한 `Default` 속성을 `T` 사용 하 여 기본적으로 사용 되는 개체에서 사용 되는 형식의 개체로 설정할 수 있습니다.

`OnAppTheme<T>`리소스는 인라인으로 사용 될 수 있습니다.

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

또는 `OnAppTheme<T>` 암시적 또는 명시적 개체를 사용 하 여 리소스를 사용할 수 있습니다 [`Style`](xref:Xamarin.Forms.Style) .

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>`리소스는 태그 확장과 함께 사용 해야 합니다 `DynamicResource` . 이렇게 하면 시스템 테마가 변경 될 때 사용 하는 개체의 모양이 업데이트 됩니다.

### <a name="onapptheme-markup-extension"></a>OnAppTheme 태그 확장

`OnAppTheme`태그 확장을 사용 하면 현재 시스템 테마에 따라 이미지나 색과 같은 소비할 리소스를 지정할 수 있습니다. 클래스와 동일한 기능을 제공 `OnAppTheme<T>` 하지만 보다 간결한 표현을 제공 합니다.

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 장치에서 밝은 테마를 사용 하 고 장치에서 짙은 테마를 사용 하는 경우 빨간색으로 설정 된 첫 번째의 텍스트 색을 녹색으로 설정 합니다. 마찬가지로는 [`Image`](xref:Xamarin.Forms.Image) 현재 시스템 테마에 따라 다른 이미지 파일을 표시 합니다.

태그 확장에 대 한 자세한 내용은 `OnAppTheme` [Onapptheme 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)을 참조 하세요.

## <a name="detect-the-current-system-theme"></a>현재 시스템 테마를 검색 합니다.

속성의 값을 가져와 현재 시스템 테마를 검색할 수 있습니다 `Application.RequestedTheme` .

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

`RequestedTheme`속성은 열거형 멤버를 반환 합니다 `OSAppTheme` . `OSAppTheme` 열거형은 다음 멤버를 정의합니다.

- `Unspecified`-장치가 지정 되지 않은 테마를 사용 하 고 있음을 나타냅니다.
- `Light`-장치에서 밝은 테마를 사용 하 고 있음을 나타냅니다.
- `Dark`-장치에서 짙은 테마를 사용 하 고 있음을 나타냅니다.

## <a name="react-to-theme-changes"></a>테마 변경에 대응

장치를 구성 하는 방법에 따라 다양 한 이유로 장치의 시스템 테마가 변경 될 수 있습니다. Xamarin.Forms이벤트를 처리 하 여 시스템 테마가 변경 될 때 앱에 알릴 수 있습니다 `Application.RequestedThemeChanged` .

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

이벤트와 함께 제공 되는 개체에는 `AppThemeChangedEventArgs` `RequestedThemeChanged` 형식의 이라는 단일 속성이 있습니다 `RequestedTheme` `OSAppTheme` . 이 속성을 검사 하 여 요청 된 시스템 테마를 검색할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [SystemThemes (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [OnAppTheme 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [의 동적 스타일Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
