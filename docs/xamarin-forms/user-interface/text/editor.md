---
title: Xamarin.Forms 편집기
description: 이 문서에서는 Xamarin.Forms 편집기 컨트롤을 사용 하 여 응용 프로그램에서 여러 줄 텍스트 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2018
ms.openlocfilehash: 97bb5ec954f36e48d8ae115baf8738862e5a8358
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649541"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms 편집기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_여러 줄 텍스트 입력_

합니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 컨트롤은 여러 줄 입력을 허용 하는 데 사용 합니다. 이 문에서는 다음과 같은 내용을 설명합니다.

- **[사용자 지정](#customization)**  &ndash; 키보드 및 색 옵션입니다.
- **[상호 작용](#interactivity)**  &ndash; 에 대 한 상호 작용을 위해 수신 대기할 수 있는 이벤트입니다.

## <a name="customization"></a>사용자 지정

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

[ `Editor` ](xref:Xamarin.Forms.Editor), 기타 텍스트 표시 보기와 같이 표시를 `Text` 속성입니다. 이 속성을 설정 하 고 제공한 텍스트 읽기를 사용할 수 있습니다는 `Editor`합니다. 다음 예제에서는 설정 된 `Text` XAML에서 속성:

```xaml
<Editor Text="I am an Editor" />
```

C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

텍스트를 읽으려면 액세스는 `Text` C#의 속성:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트를 설정합니다.

합니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 사용자 입력을 저장 하지 않는 경우 자리 표시자 텍스트를 표시 하도록 설정할 수 있습니다. 설정 하 여 이렇게를 [ `Placeholder` ](xref:Xamarin.Forms.Editor.Placeholder) 속성을을 `string`에 적절 한 콘텐츠의 형식을 지정 하기 위해 자주 사용 되는 `Editor`합니다. 자리 표시자 텍스트 색을 설정 하 여 제어할 수 또한 합니다 [ `PlaceholderColor` ](xref:Xamarin.Forms.Editor.PlaceholderColor) 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>텍스트 입력 방지

텍스트를 수정할 사용자를 방지할 수 있습니다는 [ `Editor` ](xref:Xamarin.Forms.Editor) 설정 하 여 합니다 `IsReadOnly` 기본값이 있는 속성의 `false`를 `true`:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly` 속성의 시각적 모양을 변경 하지 않습니다는 [ `Editor` ](xref:Xamarin.Forms.Editor)달리를 `IsEnabled` 도의 시각적 모양을 변경 하는 속성은 `Editor` 회색입니다.

### <a name="limiting-input-length"></a>입력된 길이 제한

합니다 [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 에 허용 되는 입력된 길이도 제한 속성을 사용할 수 있습니다 합니다 [ `Editor` ](xref:Xamarin.Forms.Editor)합니다. 이 속성을 양수로 설정 되어야 합니다.

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 속성 값 0은 입력 하지 않고 사용할 수 있습니다 값 `int.MaxValue`에 대 한 기본 값인는 [ `Editor` ](xref:Xamarin.Forms.Editor), 중임을 나타냅니다 없습니다 효과적인 입력할 수 있는 문자 수가 제한 됩니다.

### <a name="auto-sizing-an-editor"></a>자동 크기 조정 편집기

[ `Editor` ](xref:Xamarin.Forms.Editor) 설정 하면 크기를 자동으로 해당 콘텐츠를 지정할 수는 [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) 속성을 [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), 합니다 의값인[ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) 열거형입니다. 이 열거형에는 두 값에 있습니다.

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) 자동 크기 조정을 해제 되 고 값은 기본값을 나타냅니다.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) 자동 크기 조정 사용 됨을 나타냅니다.

이렇게 하려면 코드에서 다음과 같이 합니다.

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

자동 크기 조정 설정 된 경우, 높이 [ `Editor` ](xref:Xamarin.Forms.Editor) 사용자 텍스트를 사용 하 여 채웁니다 고 높이 사용자는 텍스트를 삭제 하는 경우 늘어납니다.

> [!NOTE]
> [ `Editor` ](xref:Xamarin.Forms.Editor) 는 자동 크기가 아닌 경우는 [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) 속성이 설정 되어 있습니다.

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가 상호 작용할 때 표시 되는 키보드를 [ `Editor` ](xref:Xamarin.Forms.Editor) 를 통해 프로그래밍 방식으로 설정할 수 있습니다 합니다 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) 합니다 에서다음속성중하나에속성을[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 클래스:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – 문자 보내기에 대 한 사용을 모 지 유용 합니다.
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

각 키보드의 예제를 찾을 수 있습니다 우리의 [레시피](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) 리포지토리.

합니다 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 클래스에는 [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) 키보드 첫 글자를 대문자로, spellcheck, 및 제안 동작을 지정 하 여 사용자 지정 하는 팩터리 메서드. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 열거형 값은 사용자 지정된 `Keyboard`가 반환되는 메서드에 대한 인수로 지정됩니다. `KeyboardFlags` 열거는 다음 값을 포함합니다.

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

합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성 컨트롤 여부 맞춤법 검사 사용 가능 합니다. 기본적으로 속성 설정 `true`합니다. 사용자가 텍스트를 입력 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 등의 일부 텍스트 항목 시나리오에 대 한 맞춤법 검사에서는 음수 환경 등을 설정 하 여 비활성화 해야 합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성을 `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> 경우는 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`, 사용자 지정 키보드를 사용 하지 않을, 네이티브 맞춤법 검사기를 사용할 수 없습니다. 그러나 경우에 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 가 된 맞춤법 검사를 사용 하지 않도록 설정 하는 집합 검사와 같은 [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` 속성은 무시 됩니다. 에 대 한 맞춤법 검사를 사용 하도록 설정 하려면 속성을 사용할 수 없습니다 따라서는 `Keyboard` 는 명시적으로 사용 되지 않습니다.

### <a name="enabling-and-disabling-text-prediction"></a>설정 및 텍스트 자동 완성을 사용 하지 않도록 설정

`IsTextPredictionEnabled` 속성 컨트롤 여부를 텍스트 예측 및 자동 텍스트 보정을 사용할 수 있습니다. 기본적으로 속성 설정 `true`합니다. 사용자가 텍스트를 입력 하는 대로 word 예측 표시 됩니다.

그러나 일부 텍스트 항목 시나리오에 대 한 사용자 이름, 텍스트 자동 완성 및 자동 텍스트를 입력 하는 등 수정 음수 환경을 제공 하 고 설정 하 여 비활성화 해야 합니다 `IsTextPredictionEnabled` 속성을 `false`:

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> 경우는 `IsTextPredictionEnabled` 속성 `false`,이 고 사용자 지정 키보드 가장 되지 않습니다 텍스트 자동 완성 및 자동 사용 텍스트 수정 되지 합니다. 그러나 경우는 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 사용 하지 않도록 설정 텍스트 예측, 설정한는 `IsTextPredictionEnabled` 속성은 무시 됩니다. 에 대 한 텍스트 자동 완성을 사용 하도록 설정 하려면 속성을 사용할 수 없습니다 따라서는 `Keyboard` 는 명시적으로 사용 되지 않습니다.

### <a name="colors"></a>색

`Editor` 통해 사용자 지정 배경 색을 사용 하도록 설정할 수는 `BackgroundColor` 속성입니다. 특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에 텍스트 색에 대 한 다른 기본값에 있으므로 각 플랫폼에 대 한 사용자 지정 배경 색을 설정 해야 합니다. 참조 [플랫폼 조정 작업](~/xamarin-forms/platform/device.md) 각 플랫폼에 대 한 UI를 최적화 하는 방법에 대 한 자세한 내용은 합니다.

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

![](editor-images/textbackgroundcolor.png "BackgroundColor 예제를 사용 하 여 편집기")

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 하 고 자리 표시자 텍스트를가 려 서 없는 있는지 확인 합니다.

## <a name="interactivity"></a>대화형 작업

`Editor` 두 이벤트를 노출 합니다.

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; 텍스트 편집기에서 변경 될 때 발생 합니다. 전과 변경 후에 텍스트를 제공합니다.
- [완료](xref:Xamarin.Forms.Editor.Completed) &ndash; 사용자 입력 키보드의 return 키를 눌러 종료 될 때 발생 합니다.

> [!NOTE]
> 합니다 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 올 클래스 [ `Entry` ](xref:Xamarin.Forms.Entry) 상속, 역시 [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) 하 고 [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused)이벤트입니다.

### <a name="completed"></a>완료

합니다 `Completed` 이벤트와의 상호 작용을 완료 하 react를 사용 하는 `Editor`합니다. `Completed` 키보드에서 return 키를 입력 하 여 (또는 UWP에서 Tab 키를 눌러) 필드를 사용 하 여 입력을 종료할 때 발생 합니다. 이벤트 처리기는 보낸 사람을 수행 하 여 제네릭 이벤트 처리기를 및 `EventArgs`:

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

`TextChanged` 이벤트 필드의 내용 변경에 대응 하는 데 사용 됩니다.

`TextChanged` 때마다 발생 합니다 `Text` 의 `Editor` 변경 합니다. 인스턴스를 사용 하는 이벤트에 대 한 처리기 `TextChangedEventArgs`합니다. `TextChangedEventArgs` 이전 및 새 값에 대 한 액세스를 제공 합니다 `Editor` `Text` 를 통해 합니다 `OldTextValue` 및 `NewTextValue` 속성:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Completed 이벤트 코드 및 XAML을 구독할 수 있습니다.

코드

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

- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [편집기 API](xref:Xamarin.Forms.Editor)
