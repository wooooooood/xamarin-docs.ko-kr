---
title: Xamarin.Forms 편집기
description: 이 문서에서는 Xamarin.Forms 편집기 컨트롤을 사용 하 여 응용 프로그램에서 여러 줄 텍스트 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/26/2019
ms.openlocfilehash: 1ae176cfebdde31038c30895d1bf562ff3396eaa
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306610"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms 편집기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_여러 줄 텍스트 입력_

[`Editor`](xref:Xamarin.Forms.Editor) 컨트롤은 여러 줄 입력을 허용 하는 데 사용 됩니다. 이 문서에서는 다음 내용을 설명합니다.

- **[사용자 지정](#customization)** &ndash; 키보드 및 색 옵션입니다.
- 대화형 작업을 제공 하기 위해 수신 대기 될 수 있는 **[대화형](#interactivity)** &ndash; 이벤트입니다.

## <a name="customization"></a>사용자 지정

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

다른 텍스트 표시 뷰와 마찬가지로 [`Editor`](xref:Xamarin.Forms.Editor)`Text` 속성을 노출 합니다. 이 속성을 사용 하 여 `Editor`표시 되는 텍스트를 설정 하 고 읽을 수 있습니다. 다음 예제에서는 XAML에서 `Text` 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Editor Text="I am an Editor" />
```

C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

텍스트를 읽으려면의 C#`Text` 속성에 액세스 합니다.

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트를 설정합니다.

사용자 입력을 저장 하지 않을 경우 자리 표시자 텍스트를 표시 하도록 [`Editor`](xref:Xamarin.Forms.Editor) 를 설정할 수 있습니다. 이렇게 하려면 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) 속성을 `string`설정 하 고 `Editor`에 적절 한 콘텐츠 형식을 나타내는 데 자주 사용 됩니다. 또한 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) 속성을 [`Color`](xref:Xamarin.Forms.Color)설정 하 여 자리 표시자 텍스트 색을 제어할 수 있습니다.

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>텍스트 입력 방지

사용자는 기본값인 `false`인 `IsReadOnly` 속성을 `true`로 설정 하 여 [`Editor`](xref:Xamarin.Forms.Editor) 의 텍스트를 수정할 수 없습니다.

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly` 속성은 `Editor`의 시각적 모양을 회색으로 변경 하는 `IsEnabled` 속성과 달리 [`Editor`](xref:Xamarin.Forms.Editor)의 시각적 모양을 변경 하지 않습니다.

### <a name="limiting-input-length"></a>입력된 길이 제한

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성을 사용 하 여 [`Editor`](xref:Xamarin.Forms.Editor)에 대해 허용 되는 입력 길이를 제한할 수 있습니다. 이 속성을 양수로 설정 되어야 합니다.

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성 값이 0 이면 입력이 허용 되지 않고 [`Editor`](xref:Xamarin.Forms.Editor)에 대 한 기본값인 `int.MaxValue`값이 입력 될 수 있는 문자 수에 대 한 유효 한도가 없음을 나타냅니다.

### <a name="character-spacing"></a>문자 간격

`Editor.CharacterSpacing` 속성을 `double` 값으로 설정 하 여 [`Editor`](xref:Xamarin.Forms.Editor) 에 문자 간격을 적용할 수 있습니다.

```xaml
<Editor ...
        CharacterSpacing="10" />
```

해당 하는 C# 코드가입니다.

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

그 결과 [`Editor`](xref:Xamarin.Forms.Editor) 표시 되는 텍스트의 문자는 장치 독립적 단위 `CharacterSpacing` 간격이 떨어져 있습니다.

> [!NOTE]
> `CharacterSpacing` 속성 값은 `Text` 및 `Placeholder` 속성에 의해 표시 되는 텍스트에 적용 됩니다.

### <a name="auto-sizing-an-editor"></a>자동 크기 조정 편집기

[`Editor`](xref:Xamarin.Forms.Editor) [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) 속성을 [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)( [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) 열거형의 값)로 설정 하 여 해당 콘텐츠에 자동 크기를 설정할 수 있습니다. 이 열거형에는 두 값에 있습니다.

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) 는 자동 크기 조정을 사용 하지 않도록 설정 하 고 기본값을 나타냅니다.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) 자동 크기 조정이 사용 하도록 설정 되었음을 나타냅니다.

이렇게 하려면 코드에서 다음과 같이 합니다.

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

자동 크기 조정을 사용 하도록 설정 하면 사용자가 텍스트를 채울 때 [`Editor`](xref:Xamarin.Forms.Editor) 높이가 증가 하 고, 사용자가 텍스트를 삭제할 때 높이가 감소 됩니다.

> [!NOTE]
> [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성이 설정 된 경우 [`Editor`](xref:Xamarin.Forms.Editor) 는 자동으로 크기가 조정 되지 않습니다.

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가 [`Editor`](xref:Xamarin.Forms.Editor) 와 상호 작용할 때 제공 되는 키보드는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 클래스에서 다음 속성 중 하나로 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 속성을 통해 프로그래밍 방식으로 설정할 수 있습니다.

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – 텍스트 작성 및 이모티콘이 유용한 경우에 사용합니다.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – 기본 키보드입니다.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – 전자 메일 주소를 입력할 때 사용합니다.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – 숫자를 입력할 때 사용합니다.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – 지정된 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 없이 텍스트를 입력할 때 사용합니다.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – 전화 번호를 입력할 때 사용합니다.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – 텍스트를 입력할 때 사용합니다.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – 파일 경로 및 웹 주소를 입력하는 데 사용합니다.

이렇게 하려면 XAML에서 다음과 같이 합니다.

```xaml
<Editor Keyboard="Chat" />
```

해당 하는 C# 코드가입니다.

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
```

각 키보드의 예제는 [조리법](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) 리포지토리에서 찾을 수 있습니다.

또한 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 클래스에는 대소문자, 맞춤법 검사 및 제안 동작을 지정하여 키보드를 사용자 지정하는 데 사용할 수 있는 [`Create`](xref:Xamarin.Forms.Keyboard.Create*) 팩터리 메서드가 있습니다. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 열거형 값은 사용자 지정된 `Keyboard`가 반환되는 메서드에 대한 인수로 지정됩니다. `KeyboardFlags` 열거는 다음 값을 포함합니다.

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – 키보드에 추가되는 기능이 없습니다.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – 입력된 각 문장의 첫 번째 단어의 첫 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) - 입력한 텍스트에 대해 맞춤법 검사를 수행할지를 나타냅니다.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) - 입력한 텍스트에 대해 완성된 단어를 제안할지를 나타냅니다.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – 각 단어의 첫 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – 모든 문자가 자동으로 대문자로 시작함을 나타냅니다.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – 자동 대소문자 표시가 이루어지지 않음을 나타냅니다.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – 입력한 텍스트에 대해 맞춤법 검사, 단어 완성 및 문장 첫 글자 대문자 입력이 이루어질 것임을 나타냅니다.

다음 XAML 코드 예제에서는 완성 단어를 제안하고 입력한 모든 글자를 대문자로 시작하도록 기본 [`Keyboard`](xref:Xamarin.Forms.Keyboard)를 사용자 지정하는 방법을 보여 줍니다.

```xaml
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

해당 하는 C# 코드가입니다.

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>설정 및 맞춤법 검사 사용 안 함

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성은 맞춤법 검사 사용 여부를 제어 합니다. 기본적으로 속성은 `true`로 설정 됩니다. 사용자가 텍스트를 입력 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우에는 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성을 `false`으로 설정 하 여 맞춤법 검사에 부정적인 환경을 제공 하므로 사용 하지 않도록 설정 해야 합니다.

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`로 설정 되 고 사용자 지정 키보드를 사용 하지 않는 경우 네이티브 맞춤법 검사기가 비활성화 됩니다. 그러나 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)와 같이 맞춤법 검사를 사용 하지 않도록 설정 하는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 설정 된 경우에는 `IsSpellCheckEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는 `Keyboard`에 대 한 맞춤법 검사를 사용 하도록 설정할 수 없습니다.

### <a name="enabling-and-disabling-text-prediction"></a>설정 및 텍스트 자동 완성을 사용 하지 않도록 설정

`IsTextPredictionEnabled` 속성은 텍스트 예측 및 자동 텍스트 수정을 사용 하도록 설정할지 여부를 제어 합니다. 기본적으로 속성은 `true`로 설정 됩니다. 사용자가 텍스트를 입력 하는 대로 word 예측 표시 됩니다.

그러나 사용자 이름을 입력 하는 경우와 같은 일부 텍스트 입력 시나리오에서는 텍스트 예측 및 자동 텍스트 수정이 부정적 환경을 제공 하므로 `IsTextPredictionEnabled` 속성을 `false`으로 설정 하 여 사용 하지 않도록 설정 해야 합니다.

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> `IsTextPredictionEnabled` 속성이 `false`로 설정 되어 있고 사용자 지정 키보드를 사용 하지 않는 경우 텍스트 예측 및 자동 텍스트 수정을 사용할 수 없습니다. 그러나 텍스트 예측을 사용 하지 않도록 설정 하는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 설정 된 경우에는 `IsTextPredictionEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는 `Keyboard`에 대해 텍스트 예측을 사용 하도록 설정할 수 없습니다.

### <a name="colors"></a>색

`Editor` `BackgroundColor` 속성을 통해 사용자 지정 배경색을 사용 하도록 설정할 수 있습니다. 특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 색에 대 한 다른 기본값에 있으므로 각 플랫폼에 대 한 사용자 지정 배경 색을 설정 해야 합니다. 각 플랫폼에 대 한 UI를 최적화 하는 방법에 대 한 자세한 내용은 [플랫폼 조정 작업](~/xamarin-forms/platform/device.md) 을 참조 하세요.

C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Editor with BackgroundColor Example")

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 하 고 자리 표시자 텍스트를가 려 서 없는 있는지 확인 합니다.

## <a name="interactivity"></a>상호 작용

`Editor`는 두 개의 이벤트를 노출 합니다.

- 편집기에서 텍스트가 변경 될 때 [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; 발생 합니다. 전과 변경 후에 텍스트를 제공합니다.
- 사용자가 키보드에서 return 키를 눌러 입력을 종료 하면 [완료](xref:Xamarin.Forms.Editor.Completed) 된 &ndash; 발생 합니다.

> [!NOTE]
> [`Entry`](xref:Xamarin.Forms.Entry) 를 상속 하는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스에도 [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 및 [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트가 있습니다.

### <a name="completed"></a>Completed

`Completed` 이벤트는 `Editor`상호 작용이 완료 될 때 대응 하는 데 사용 됩니다. 사용자가 키보드에 return 키를 입력 하거나 UWP에서 Tab 키를 눌러 필드에 입력을 끝내 면 `Completed` 발생 합니다. 이벤트에 대 한 처리기는 일반 이벤트 처리기로, 발신자 및 `EventArgs`을 수행 합니다.

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Completed 이벤트 코드 및 XAML을 구독할 수 있습니다.

C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged` 이벤트는 필드 내용의 변경에 대응 하는 데 사용 됩니다.

`Editor`의 `Text` 변경 될 때마다 `TextChanged` 발생 합니다. 이벤트 처리기는 `TextChangedEventArgs`의 인스턴스를 사용 합니다. `TextChangedEventArgs`는 `OldTextValue` 및 `NewTextValue` 속성을 통해 `Editor` `Text`의 이전 값과 새 값에 대 한 액세스를 제공 합니다.

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Completed 이벤트 코드 및 XAML을 구독할 수 있습니다.

코드:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [편집기 API](xref:Xamarin.Forms.Editor)
