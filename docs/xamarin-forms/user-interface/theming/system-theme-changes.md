---
title: 응용 프로그램의 시스템 테마 변경 내용에 응답 Xamarin.Forms
description: Xamarin.Forms응용 프로그램은 OnAppTheme 유형과 DynamicResource 태그 확장을 사용 하 여 운영 체제 테마 변경 내용에 응답할 수 있습니다.
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 28bcbed3a03a2abbec42a619062579419a3063a4
ms.sourcegitcommit: 8a18471b3d96f3f726b66f9bc50a829f1c122f29
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84988201"
---
# <a name="respond-to-system-theme-changes-in-xamarinforms-applications"></a>응용 프로그램의 시스템 테마 변경 내용에 응답 Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

장치에는 일반적으로 운영 체제 수준에서 설정할 수 있는 다양 한 모양 기본 설정 집합을 참조 하는 밝은 테마 및 어두운 테마가 포함 됩니다. 응용 프로그램은 이러한 시스템 테마를 준수 하 고 시스템 테마가 변경 될 때 즉시 응답 해야 합니다.

시스템 테마는 장치 구성에 따라 다양 한 이유로 변경 될 수 있습니다. 여기에는 사용자가 명시적으로 변경 하는 시스템 테마, 하루 중 시간으로 변경 된 시스템 테마 및 낮은 광원 등의 환경적 요소로 인 한 변경 내용이 포함 됩니다.

Xamarin.Forms응용 프로그램은 `AppThemeBinding` 태그 확장 및 `SetAppThemeColor` 및 확장 메서드를 사용 하 여 리소스를 사용 하 여 시스템 테마 변경에 응답할 수 있습니다 `SetOnAppTheme<T>` .

> [!IMPORTANT]
> 시스템 테마 변경에 대 한 응답은 현재 실험적 이며 플래그를 설정 하는 방법 으로만 사용할 수 있습니다 `AppTheme_Experimental` . 자세한 내용은 [실험적 플래그](~/xamarin-forms/internals/experimental-flags.md)를 참조 하세요.

Xamarin.Forms시스템 테마 변경에 응답 하려면에 대해 다음과 같은 요구 사항을 충족 해야 합니다.

- Xamarin.Forms4.6.0.967 이상
- iOS 13 이상
- Android 10 (API 29) 이상.
- UWP 빌드 14393 이상.

다음 스크린샷에는 iOS 및 Android에서 밝은 시스템 테마와 어두운 시스템 테마에 대 한 테마 페이지가 표시 됩니다.

[![IOS 및 Android](system-theme-changes-images/main-page-both-themes.png "테마가 적용 된 앱의 기본 페이지")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 기본 페이지") 
 에서 테마가 적용 된 앱의 기본 페이지 스크린샷 [ ![IOS 및 Android에서 테마가 적용 된 앱의 세부 정보 페이지 스크린샷](system-theme-changes-images/detail-page-both-themes.png "테마가 적용 된 앱의 세부 정보 페이지")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "테마가 적용 된 앱의 세부 정보 페이지")

## <a name="define-and-consume-theme-resources"></a>테마 리소스 정의 및 사용

밝은 테마와 어두운 테마에 대 한 리소스는 `AppThemeBinding` 태그 확장과 `SetAppThemeColor` 및 확장 메서드와 함께 사용 될 수 있습니다 `SetOnAppTheme<T>` . 이러한 접근 방식을 사용 하면 현재 시스템 테마의 값에 따라 리소스가 자동으로 적용 됩니다. 또한 이러한 리소스를 사용 하는 개체는 앱이 실행 되는 동안 시스템 테마가 변경 되는 경우 자동으로 업데이트 됩니다.

### <a name="appthemebinding-markup-extension"></a>AppThemeBinding 태그 확장

`AppThemeBinding`태그 확장을 사용 하면 현재 시스템 테마에 따라 이미지나 색 등의 리소스를 사용할 수 있습니다.

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{AppThemeBinding Light=Green, Dark=Red}" />
        <Image Source="{AppThemeBinding Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 장치에서 밝은 테마를 사용 하 고 장치에서 짙은 테마를 사용 하는 경우 빨간색으로 설정 된 첫 번째의 텍스트 색을 녹색으로 설정 합니다. 마찬가지로는 [`Image`](xref:Xamarin.Forms.Image) 현재 시스템 테마에 따라 다른 이미지 파일을 표시 합니다.

또한에 정의 된 리소스는 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 태그 확장과 함께 사용 될 수 있습니다 `StaticResource` .

```xaml
<ContentPage ...>
    <ContentPage.Resources>

        <!-- Light colors -->
        <Color x:Key="LightPrimaryColor">WhiteSmoke</Color>
        <Color x:Key="LightSecondaryColor">Black</Color>

        <!-- Dark colors -->
        <Color x:Key="DarkPrimaryColor">Teal</Color>
        <Color x:Key="DarkSecondaryColor">White</Color>

        <Style x:Key="ButtonStyle"
               TargetType="Button">
            <Setter Property="BackgroundColor"
                    Value="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}" />
            <Setter Property="TextColor"
                    Value="{AppThemeBinding Light={StaticResource LightSecondaryColor}, Dark={StaticResource DarkSecondaryColor}}" />
        </Style>

    </ContentPage.Resources>

    <Grid BackgroundColor="{AppThemeBinding Light={StaticResource LightPrimaryColor}, Dark={StaticResource DarkPrimaryColor}}">
      <Button Text="MORE INFO"
              Style="{StaticResource ButtonStyle}" />
    </Grid>    
</ContentPage>    
```

이 예제에서 및 스타일의 배경색은 [`Grid`](xref:Xamarin.Forms.Grid) [`Button`](xref:Xamarin.Forms.Button) 장치에서 밝은 테마를 사용 하는지 또는 어두운 테마를 사용 하는지에 따라 달라 집니다.

태그 확장에 대 한 자세한 내용은 `AppThemeBinding` [AppThemeBinding 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)을 참조 하세요.

### <a name="extension-methods"></a>확장 메서드

Xamarin.Forms`SetAppThemeColor` `SetOnAppTheme<T>` [`VisualElement`](xref:Xamarin.Forms.VisualElement) 개체가 시스템 테마 변경 내용에 응답할 수 있도록 하는 및 확장 메서드를 포함 합니다.

`SetAppThemeColor`메서드를 사용 하면 [`Color`](xref:Xamarin.Forms.Color) 현재 시스템 테마를 기반으로 대상 속성에 대해 설정 되는 개체를 지정할 수 있습니다.

```csharp
Label label = new Label();
label.SetAppThemeColor(Label.TextColorProperty, Color.Green, Color.Red);
```

이 예제에서는 [`Label`](xref:Xamarin.Forms.Label) 장치에서 밝은 테마를 사용 하 고 장치에서 짙은 테마를 사용 하는 경우 빨간색으로 설정 된의 텍스트 색을 녹색으로 설정 합니다.

`SetOnAppTheme<T>`메서드를 사용 하면 `T` 현재 시스템 테마를 기반으로 대상 속성에 설정 되는 형식의 개체를 지정할 수 있습니다.

```csharp
Image image = new Image();
image.SetOnAppTheme<FileImageSource>(Image.SourceProperty, "lightlogo.png", "darklogo.png");
```

이 예제에서는 [`Image`](xref:Xamarin.Forms.Image) `lightlogo.png` 장치가 밝은 테마를 사용 하는 경우와 `darklogo.png` 장치에서 짙은 테마를 사용 하는 경우를 표시 합니다.

## <a name="detect-the-current-system-theme"></a>현재 시스템 테마를 검색 합니다.

속성의 값을 가져와 현재 시스템 테마를 검색할 수 있습니다 `Application.RequestedTheme` .

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

`RequestedTheme`속성은 열거형 멤버를 반환 합니다 `OSAppTheme` . `OSAppTheme` 열거형은 다음 멤버를 정의합니다.

- `Unspecified`-장치가 지정 되지 않은 테마를 사용 하 고 있음을 나타냅니다.
- `Light`-장치에서 밝은 테마를 사용 하 고 있음을 나타냅니다.
- `Dark`-장치에서 짙은 테마를 사용 하 고 있음을 나타냅니다.

## <a name="set-the-current-user-theme"></a>현재 사용자 테마 설정

응용 프로그램에서 사용 하는 테마는 `Application.UserAppTheme` `OSAppTheme` 현재 작동 중인 시스템 테마에 관계 없이 형식의 속성을 사용 하 여 설정할 수 있습니다.

```csharp
Application.Current.UserAppTheme = OSAppTheme.Dark;
```

이 예제에서 응용 프로그램은 현재 작동 중인 시스템 테마에 관계 없이 시스템 어둡게 모드에 대해 정의 된 테마를 사용 하도록 설정 됩니다.

> [!NOTE]
> 속성을로 설정 하 여 `UserAppTheme` `OSAppTheme.Unspecified` 운영 체제 테마를 기본값으로 설정 합니다.

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
- [AppThemeBinding 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#appthemebinding-markup-extension)
- [리소스 사전](~/xamarin-forms/xaml/resource-dictionaries.md)
- [XAML 스타일을 사용하여 Xamarin.Forms 앱 스타일 지정](~/xamarin-forms/user-interface/styles/xaml/index.md)
