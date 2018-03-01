---
title: "글꼴"
description: "Xamarin.Forms에 글꼴 설정"
ms.topic: article
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 22a0b0f5df5a44f2409a59b26eb841b97c920d8b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="fonts"></a>글꼴

이 문서에서는 텍스트를 표시 하는 컨트롤에 Xamarin.Forms 글꼴 특성 (가중치, 크기 등)을 지정할 수 있습니다 어떻게을 설명 합니다. 글꼴 정보 수 [코드에 지정 된](#Setting_Font_in_Code) 또는 [Xaml에 지정 된](#Setting_Font_in_Xaml)합니다.
사용 하 여 이기도 한 [사용자 지정 글꼴](#Using_a_Custom_Font)합니다.

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>코드에서 글꼴을 설정합니다.

텍스트를 표시 하는 모든 컨트롤의 세 가지 글꼴 관련 속성을 사용 합니다.

- **FontFamily** &ndash; 는 `string` 글꼴 이름입니다.
- **FontSize** &ndash; 으로 글꼴 크기는 `double`합니다.
- **FontAttributes** &ndash; 등의 스타일 정보를 지정 하는 문자열 *기울임꼴* 및 **b o l d** (사용 하는 `FontAttributes` C# 열거형)입니다.

이 코드에는 레이블을 만들고 글꼴 크기 및 표시 하는 가중치를 지정 하는 방법을 보여 줍니다.

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>글꼴 크기

`FontSize` 속성 double 값으로 예를 들어 설정할 수 있습니다.

```csharp
label.FontSize = 24;
```

사용할 수도 있습니다는 `NamedSize` 에 4 개의 기본 제공 옵션이; 열거형 Xamarin.Forms는 각 플랫폼에 대 한 최적의 크기를 선택합니다.

-  **Micro**
-  **Small**
-  **보통**
-  **큰**


`NamedSize` 열거형 될 수 있습니다 아무 곳에 나 사용을 `FontSize` 를 사용 하 여 지정할 수 있습니다는 `Device.GetNamedSize` 를 값으로 변환 하는 메서드는 `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>글꼴 특성

글꼴 스타일와 같은 **굵게** 및 *기울임꼴* 에 설정 될 수는 `FontAttributes` 속성입니다. 다음 값은 현재 지원 됩니다.

-  **없음**
-  **굵게**
-  **Italic**

`FontAttribute` 열거형을 다음과 같이 사용할 수 있습니다 (단일 특성을 지정할 수 있습니다 또는 `OR` 함께):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

일부 Xamarin.Forms 컨트롤 (예: `Label`)도 사용 하 여 문자열 내에서 다른 글꼴 특성을 지원는 `FormattedString` 클래스입니다. A `FormattedString` 하나 이상의 이루어져 `Span`s, 각각 고유한 서식 특성을 가질 수 있습니다.

`Span` 클래스에는 다음 특성이 있습니다.

* **텍스트** &ndash; 표시할 값을
* **FontFamily** &ndash; 글꼴 이름
* **FontSize** &ndash; 글꼴 크기
* **FontAttributes** &ndash; 스타일 정보 같은 *기울임꼴* 및 **굵게 표시**
* **ForegroundColor** &ndash; 텍스트 색
* **BackgroundColor** &ndash; 배경색

만들기 및 표시의 예로 `FormattedString` 아래에 나와 &ndash; 레이블의 할당 되는 `FormattedText` 속성 및 not는 `Text` 속성입니다.

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>플랫폼 마다 글꼴 정보 설정

또는 `Device.RuntimePlatform` 이 코드에서와 같이 각 플랫폼에서 다른 글꼴 이름을 설정 하려면 속성을 사용할 수 있습니다.

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS에 대 한 글꼴 정보의 좋은 소스는 [iosfonts.com](http://iosfonts.com)합니다.

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Xaml에서 글꼴을 설정합니다.

모든 해당 표시 텍스트를 제어 하는 Xamarin.Forms는 `Font` Xaml에서 설정할 수 있는 속성입니다. 이 예에서 표시 된 것과 같이 명명 된 크기 열거형 값을 사용 하도록 Xaml에서 글꼴을 설정 하는 가장 간단한 방법은입니다.

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

에 대 한 기본 제공 변환기는는 `Font` 속성을 사용 하는 Xaml에서 문자열 값으로 표시할 수 있는 모든 글꼴 설정 합니다. 다음 예제에서는 Xaml의 글꼴 특성 및 크기를 지정 하는 방법을 보여 줍니다.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

다중 지정 하려면 `Font` 설정, 단일 글꼴 특성 문자열에 필요한 설정을 결합 합니다. 글꼴 특성 문자열으로 서식을 지정 하는 `"[font-face],[attributes],[size]"`합니다. 매개 변수 순서는 중요, 모든 매개 변수는 선택 사항 및 여러 `attributes` 예를 들어 지정할 수 있습니다.

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

`FormattedString` 여기에 표시 된 대로 클래스 XAML에서 사용할 수도 있습니다.

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) 데도 사용할 수 있습니다 XAML에서 각 플랫폼에서 다른 글꼴을 렌더링 합니다. 다음 예제에서는 사용자 지정 글꼴을 사용 하 여 iOS에서 (<span style="font-family:MarkerFelt-Thin">MarkerFelt 씬</span>) 하 고 다른 플랫폼에서만 크기/특성을 지정 합니다.

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

사용자 지정 글꼴을 지정할 때 항상 이므로 사용 하면 좋습니다 `OnPlatform`와 마찬가지로 모든 플랫폼에서 사용할 수 있는 글꼴을 찾는 것이 어렵습니다.

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>사용자 지정 글꼴을 사용 하 여

기본 제공 서체 이외의 글꼴을 사용 하 여 일부 플랫폼별 코딩이 필요 합니다. 사용자 지정 글꼴 스크린샷은 **바다가 재** 에서 [Google의 오픈 소스 글꼴](https://www.google.com/fonts) iOS, Android 및 Xamarin.Forms를 사용 하 여 Windows Phone 렌더링 합니다.

 [ ![IOS 및 Android 사용자 지정 글꼴](fonts-images/custom-sml.png "사용자 지정 글꼴 예제")](fonts-images/custom.png "사용자 지정 글꼴 예제")

각 플랫폼에 대해 필요한 단계는 다음과 같습니다. 응용 프로그램을 사용 하 여 사용자 지정 글꼴 파일을 포함 하는 경우 배포에 대 한 글꼴의 라이선스를 허용 하는지 확인 해야 합니다.

### <a name="ios"></a>iOS

먼저는 로드 되었는지 확인 한 다음 참조 하는 Xamarin.Forms를 사용 하 여 이름 하 여 사용자 지정 글꼴을 표시 하는 것이 불가능 `Font` 메서드.
지침에 따라 [이 블로그 게시물](http://blog.xamarin.com/custom-fonts-in-ios/):

1. 사용 하 여 글꼴 파일 추가 **빌드 작업: BundleResource**, 및
2. 업데이트는 **Info.plist** 파일 (**응용 프로그램에서 제공 하는 글꼴**, 또는 `UIAppFonts`키,), 다음
3. Xamarin.Forms에는 글꼴을 정의한 경우 항상를 이름으로 참조!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android 용 Xamarin.Forms에는 특정 명명 규칙에 따라 프로젝트에 추가 된 사용자 지정 글꼴을 참조할 수 있습니다. 글꼴 파일을 먼저 추가 **자산** 집합과 응용 프로그램 프로젝트의 폴더 *빌드 작업: AndroidAsset*합니다. 그런 다음 전체 경로 사용 하 고 *글꼴 이름을* 아래 코드 조각에서 보여 주듯이 xamarin.forms에 글꼴 이름으로 해시 (#)로 구분:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Xamarin.Forms Windows 플랫폼에 특정 한 명명 표준을 따라 프로젝트에 추가 된 사용자 지정 글꼴을 참조할 수 있습니다. 글꼴 파일을 먼저 추가 **/Assets/글꼴/** 집합과 응용 프로그램 프로젝트의 폴더는 <span class="UIItem">빌드 동작: 내용</span>합니다. 다음 전체 경로 및 글꼴 파일 이름, 해시 (#) 다음을 사용 하 여 및 <span class="UIItem">글꼴 이름을</span>아래 코드 조각에서 보여 주듯이,:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> 글꼴 파일 이름과 글꼴 이름을 참고 달라질 수 있습니다. Windows에서 글꼴 이름을 검색 하려면.ttf 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 **미리 보기**합니다. 그런 다음 미리 보기 창에서 글꼴 이름의 확인할 수 있습니다.

응용 프로그램에 대 한 일반적인 코드 완료 됩니다. 플랫폼 특정 전화 걸기 코드는 이제 [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md)로 구현됩니다.

### <a name="xaml"></a>XAML

사용할 수도 있습니다 [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) 사용자 지정 글꼴을 렌더링 하는 XAML에서:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>요약

Xamarin.Forms는 간단한 기본 설정 하는 데 제공 쉽게 지원 되는 모든 플랫폼에 대 한 텍스트 크기를 조정 합니다. 또한 글꼴 및 크기를 지정할 수 있습니다 &ndash; 각 플랫폼에 대해 다르게도 &ndash; 보다 세부적인 제어가 필요한 경우. `FormattedString` 클래스를 사용 하 여 다른 글꼴 사양을 포함 하는 문자열을 만드는 데 사용할 수는 `Span` 클래스입니다.

글꼴 정보에서 올바른 형식의 글꼴 특성을 사용 하 여 Xaml 지정할 수도 있습니다 또는 `FormattedString` 요소 `Span` 자식입니다.


## <a name="related-links"></a>관련 링크

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
