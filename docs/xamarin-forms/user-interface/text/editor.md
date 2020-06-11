---
제목: " Xamarin.Forms 편집기" 설명: "이 문서에서는 편집기 컨트롤을 사용 하 여 Xamarin.Forms 응용 프로그램에서 여러 줄 텍스트 입력을 허용 하는 방법을 설명 합니다."
assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07: xamarin-forms author: davidbritch: dabritch:: 09/26/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-editor"></a>Xamarin.Forms 편집기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_여러 줄 텍스트 입력_

[`Editor`](xref:Xamarin.Forms.Editor)컨트롤은 여러 줄 입력을 허용 하는 데 사용 됩니다. 이 문서에서는 다음 내용을 설명합니다.

- **[사용자 지정](#customization)** &ndash; 키보드 및 색 옵션
- **[대화형 작업](#interactivity)** &ndash; 에서 대화형 작업을 제공 하기 위해 수신 대기 하는 이벤트입니다.

## <a name="customization"></a>사용자 지정

### <a name="setting-and-reading-text"></a>텍스트 설정 및 읽기

[`Editor`](xref:Xamarin.Forms.Editor)다른 텍스트 표시 뷰와 마찬가지로는 속성을 노출 합니다 `Text` . 이 속성을 사용 하 여에 표시 되는 텍스트를 설정 하 고 읽을 수 있습니다 `Editor` . 다음 예제에서는 XAML에서 속성을 설정 하는 방법을 보여 줍니다 `Text` .

```xaml
<Editor Text="I am an Editor" />
```

C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

텍스트를 읽으려면 `Text` c #의 속성에 액세스 합니다.

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트 설정

[`Editor`](xref:Xamarin.Forms.Editor)사용자 입력을 저장 하지 않을 경우 자리 표시자 텍스트를 표시 하도록를 설정할 수 있습니다. 이렇게 하려면 속성을으로 설정 하 여이를 수행 합니다 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string` . 일반적으로에 적절 한 콘텐츠 형식을 나타내는 데 사용 됩니다 `Editor` . 또한 속성을로 설정 하 여 자리 표시자 텍스트 색을 제어할 수 있습니다 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>텍스트 입력 방지

사용자는 속성을로 설정 하 여의 텍스트를 수정할 수 없도록 할 수 있습니다 [`Editor`](xref:Xamarin.Forms.Editor) `IsReadOnly` .이 속성의 기본값은입니다 `false` `true` .

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> 속성은의 `IsReadonly` [`Editor`](xref:Xamarin.Forms.Editor) `IsEnabled` 시각적 모양을 회색으로 변경 하는 속성과 달리의 시각적 모양을 변경 하지 않습니다 `Editor` .

### <a name="limiting-input-length"></a>입력 길이 제한

속성은에 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 대해 허용 되는 입력 길이를 제한 하는 데 사용할 수 있습니다 [`Editor`](xref:Xamarin.Forms.Editor) . 이 속성은 양의 정수로 설정 해야 합니다.

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)속성 값이 0 이면 입력이 허용 되지 않으며, `int.MaxValue` 에 대 한 기본값인 값이 이면 [`Editor`](xref:Xamarin.Forms.Editor) 입력 될 수 있는 문자 수에 대 한 유효 한도가 없음을 나타냅니다.

### <a name="character-spacing"></a>문자 간격

[`Editor`](xref:Xamarin.Forms.Editor)속성을 값으로 설정 하 여에 문자 간격을 적용할 수 있습니다 `Editor.CharacterSpacing` `double` .

```xaml
<Editor ...
        CharacterSpacing="10" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Editor editor = new editor { CharacterSpacing = 10 };
```

그 결과에 표시 되는 텍스트의 문자는 [`Editor`](xref:Xamarin.Forms.Editor) `CharacterSpacing` 장치 독립적인 단위로 구분 됩니다.

> [!NOTE]
> `CharacterSpacing`속성 값은 및 속성에 의해 표시 되는 텍스트에 적용 됩니다 `Text` `Placeholder` .

### <a name="auto-sizing-an-editor"></a>편집기 자동 크기 조정

[`Editor`](xref:Xamarin.Forms.Editor) [`Editor.AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) 속성을 열거형 값으로 설정 하 여 해당 콘텐츠에 대 한 크기를 자동으로 조정 하도록 만들 수 있습니다 [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) [`EditoAutoSizeOption`](xref:Xamarin.Forms.EditorAutoSizeOption) . 이 열거형에는 두 개의 값이 있습니다.

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled)자동 크기 조정을 사용 하지 않도록 설정 하 고 기본값을 나타냅니다.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges)자동 크기 조정을 사용 하도록 설정 됨을 나타냅니다.

이러한 작업은 다음과 같이 코드에서 수행할 수 있습니다.

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

자동 크기 조정을 사용 하는 경우 [`Editor`](xref:Xamarin.Forms.Editor) 사용자가 텍스트를 채울 때의 높이가 증가 하 고 사용자가 텍스트를 삭제할 때 높이가 줄어듭니다.

> [!NOTE]
> 속성이 설정 된 경우에는가 [`Editor`](xref:Xamarin.Forms.Editor) 자동으로 크기가 조정 되지 않습니다 [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가와 상호 작용할 때 표시 되는 키보드는 [`Editor`](xref:Xamarin.Forms.Editor) 속성을 통해 프로그래밍 방식으로 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 클래스의 다음 속성 중 하나로 설정할 수 있습니다 [`Keyboard`](xref:Xamarin.Forms.Keyboard) .

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – 텍스트 작성 및 이모티콘이 유용한 경우에 사용합니다.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – 기본 키보드입니다.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – 전자 메일 주소를 입력할 때 사용합니다.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – 숫자를 입력할 때 사용합니다.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – 지정된 [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 없이 텍스트를 입력할 때 사용합니다.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – 전화 번호를 입력할 때 사용합니다.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – 텍스트를 입력할 때 사용합니다.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – 파일 경로 및 웹 주소를 입력하는 데 사용합니다.

이렇게 하면 다음과 같이 XAML로 수행할 수 있습니다.

```xaml
<Editor Keyboard="Chat" />
```

해당하는 C# 코드는 다음과 같습니다.

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

해당하는 C# 코드는 다음과 같습니다.

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>맞춤법 검사 사용 및 사용 안 함

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)속성은 맞춤법 검사 사용 여부를 제어 합니다. 기본적으로 속성은로 설정 됩니다 `true` . 사용자가 텍스트를 입력 하면 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우에는 다음과 같이 속성을로 설정 하 여 맞춤법 검사에 부정적인 환경을 제공 하므로 사용 하지 않도록 설정 해야 합니다 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) `false` .

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)속성이로 설정 되 `false` 고 사용자 지정 키보드를 사용 하지 않는 경우 네이티브 맞춤법 검사기가 비활성화 됩니다. 그러나 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 와 같은 맞춤법 검사를 사용 하지 않도록 설정한 경우에는 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) `IsSpellCheckEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 `Keyboard` 명시적으로 사용 하지 않도록 설정 하는에 대 한 맞춤법 검사를 사용 하도록 설정할 수 없습니다.

### <a name="enabling-and-disabling-text-prediction"></a>텍스트 예측 사용 및 사용 안 함

`IsTextPredictionEnabled`속성은 텍스트 예측과 자동 텍스트 교정이 사용 되는지 여부를 제어 합니다. 기본적으로 속성은로 설정 됩니다 `true` . 사용자가 텍스트를 입력 하면 단어 예측이 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우 텍스트 예측 및 자동 텍스트 수정이 부정적 환경을 제공 하므로 속성을로 설정 하 여 사용 하지 않도록 설정 해야 합니다 `IsTextPredictionEnabled` `false` .

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> `IsTextPredictionEnabled`속성이로 설정 되 `false` 고 사용자 지정 키보드를 사용 하지 않는 경우 텍스트 예측 및 자동 텍스트 수정을 사용할 수 없습니다. 그러나가 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 텍스트 예측을 사용 하지 않도록 설정 된 경우에는 `IsTextPredictionEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는에 대해 텍스트 예측을 사용 하도록 설정할 수 없습니다 `Keyboard` .

### <a name="colors"></a>색

`Editor`속성을 통해 사용자 지정 배경색을 사용 하도록 설정할 수 있습니다 `BackgroundColor` . 각 플랫폼에서 색을 사용할 수 있도록 하려면 특별 한 주의가 필요 합니다. 각 플랫폼에는 텍스트 색의 기본값이 다르기 때문에 각 플랫폼에 대 한 사용자 지정 배경색을 설정 해야 할 수도 있습니다. 각 플랫폼에 대 한 UI를 최적화 하는 방법에 대 한 자세한 내용은 [플랫폼 조정 작업](~/xamarin-forms/platform/device.md) 을 참조 하세요.

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

XAML에서:

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

선택한 배경색과 텍스트 색을 각 플랫폼에서 사용할 수 있으며 자리 표시자 텍스트가 모호 하지 않은지 확인 합니다.

## <a name="interactivity"></a>대화형 작업

`Editor`는 다음과 같은 두 이벤트를 노출 합니다.

- [TextChanged](xref:Xamarin.Forms.InputView.TextChanged) &ndash; 편집기에서 텍스트가 변경 될 때 발생 합니다. 변경 전후에 텍스트를 제공 합니다.
- [완료 됨](xref:Xamarin.Forms.Editor.Completed) &ndash; 사용자가 키보드에서 return 키를 눌러 입력을 끝낸 경우 발생 합니다.

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 상속 하는 클래스에 [`Entry`](xref:Xamarin.Forms.Entry) 도 [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 및 [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트가 있습니다.

### <a name="completed"></a>완료됨

`Completed`이벤트는와의 상호 작용이 완료 될 때 대응 하는 데 사용 됩니다 `Editor` . `Completed`사용자가 키보드에서 return 키를 입력 하거나 UWP에서 Tab 키를 눌러 입력을 끝낼 때 발생 합니다. 이벤트에 대 한 처리기는 일반 이벤트 처리기로, 발신자 및 다음을 수행 합니다 `EventArgs` .

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

완료 된 이벤트는 코드 및 XAML에서 구독할 수 있습니다.

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

XAML에서:

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

`TextChanged`이벤트는 필드 내용의 변경에 대응 하는 데 사용 됩니다.

`TextChanged`의가 변경 될 때마다 발생 합니다 `Text` `Editor` . 이벤트에 대 한 처리기는의 인스턴스를 사용 `TextChangedEventArgs` 합니다. `TextChangedEventArgs``Editor` `Text` 및 속성을 통해의 이전 값과 새 값에 대 한 액세스를 제공 합니다 `OldTextValue` `NewTextValue` .

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

완료 된 이벤트는 코드 및 XAML에서 구독할 수 있습니다.

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

XAML에서:

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
