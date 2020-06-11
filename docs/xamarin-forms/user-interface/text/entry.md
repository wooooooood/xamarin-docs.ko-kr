---
제목: " Xamarin.Forms 항목" 설명: "이 문서에서는 entry 클래스를 사용 하 여 Xamarin.Forms 응용 프로그램에서 한 줄 텍스트 또는 암호 입력을 허용 하는 방법을 설명 합니다."
assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E: xamarin-forms author: davidbritch: dabritch:: 09/25/2019-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-entry"></a>Xamarin.Forms엔트리의

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_한 줄 텍스트 또는 암호 입력_

는 Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) 한 줄 텍스트 입력에 사용 됩니다. `Entry`뷰와 마찬가지로는 [`Editor`](xref:Xamarin.Forms.Editor) 여러 키보드 유형을 지원 합니다. 또한를 `Entry` 암호 필드로 사용할 수 있습니다.

## <a name="display-customization"></a>사용자 지정 표시

### <a name="setting-and-reading-text"></a>텍스트 설정 및 읽기

`Entry`다른 텍스트 표시 뷰와 마찬가지로는 속성을 노출 합니다 [`Text`](xref:Xamarin.Forms.InputView.Text) . 이 속성을 사용 하 여에 표시 되는 텍스트를 설정 하 고 읽을 수 있습니다 `Entry` . 다음 예제에서는 XAML에서 속성을 설정 하는 방법을 보여 줍니다 `Text` .

```xaml
<Entry Text="I am an Entry" />
```

C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

텍스트를 읽으려면 `Text` c #의 속성에 액세스 합니다.

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트 설정

[`Entry`](xref:Xamarin.Forms.Entry)사용자 입력을 저장 하지 않을 경우 자리 표시자 텍스트를 표시 하도록를 설정할 수 있습니다. 이렇게 하려면 속성을으로 설정 하 여이를 수행 합니다 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) `string` . 일반적으로에 적절 한 콘텐츠 형식을 나타내는 데 사용 됩니다 `Entry` . 또한 속성을로 설정 하 여 자리 표시자 텍스트 색을 제어할 수 있습니다 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) [`Color`](xref:Xamarin.Forms.Color) .

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> 의 너비는 `Entry` 속성을 설정 하 여 정의할 수 있습니다 `WidthRequest` . `Entry`속성의 값에 따라 정의 되는의 너비에 의존 하지 않습니다 `Text` .

### <a name="preventing-text-entry"></a>텍스트 입력 방지

사용자는 속성을로 설정 하 여의 텍스트를 수정할 수 없도록 할 수 있습니다 [`Entry`](xref:Xamarin.Forms.Entry) `IsReadOnly` .이 속성의 기본값은입니다 `false` `true` .

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> 속성은의 `IsReadonly` [`Entry`](xref:Xamarin.Forms.Entry) `IsEnabled` 시각적 모양을 회색으로 변경 하는 속성과 달리의 시각적 모양을 변경 하지 않습니다 `Entry` .

### <a name="limiting-input-length"></a>입력 길이 제한

속성은에 [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 대해 허용 되는 입력 길이를 제한 하는 데 사용할 수 있습니다 [`Entry`](xref:Xamarin.Forms.Entry) . 이 속성은 양의 정수로 설정 해야 합니다.

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength)속성 값이 0 이면 입력이 허용 되지 않으며, `int.MaxValue` 에 대 한 기본값인 값이 이면 [`Entry`](xref:Xamarin.Forms.Entry) 입력 될 수 있는 문자 수에 대 한 유효 한도가 없음을 나타냅니다.

### <a name="character-spacing"></a>문자 간격

[`Entry`](xref:Xamarin.Forms.Entry)속성을 값으로 설정 하 여에 문자 간격을 적용할 수 있습니다 `Entry.CharacterSpacing` `double` .

```xaml
<Entry ...
       CharacterSpacing="10" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

그 결과에 표시 되는 텍스트의 문자는 [`Entry`](xref:Xamarin.Forms.Entry) `CharacterSpacing` 장치 독립적인 단위로 구분 됩니다.

> [!NOTE]
> `CharacterSpacing`속성 값은 및 속성에 의해 표시 되는 텍스트에 적용 됩니다 `Text` `Placeholder` .

### <a name="password-fields"></a>암호 필드

`Entry`속성을 제공 합니다 `IsPassword` . `IsPassword`가 이면 `true` 필드의 내용이 검정 원으로 표시 됩니다.

XAML에서:

```xaml
<Entry IsPassword="true" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![항목 IsPassword 예제](entry-images/password.png)

암호 필드로 구성 된의 인스턴스와 함께 자리 표시자를 사용할 수 있습니다 `Entry` .

XAML에서:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![항목 IsPassword 및 자리 표시자 예제](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>커서 위치 및 텍스트 선택 길이 설정

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition)속성을 사용 하 여 속성에 저장 된 문자열에 다음 문자를 삽입할 위치를 반환 하거나 설정할 수 있습니다 [`Text`](xref:Xamarin.Forms.InputView.Text) .

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

속성의 기본값은 [`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) 0 이며이는의 시작 부분에 텍스트가 삽입 됨을 나타냅니다 `Entry` .

또한 [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) 속성을 사용 하 여 내에서 텍스트 선택의 길이를 반환 하거나 설정할 수 있습니다 `Entry` .

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

속성의 기본값은 [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) 0으로, 선택 된 텍스트가 없음을 나타냅니다.

### <a name="displaying-a-clear-button"></a>지우기 단추 표시

`ClearButtonVisibility`속성을 사용 하 여 [`Entry`](xref:Xamarin.Forms.Entry) 가 텍스트를 지울 수 있는 지우기 단추를 표시 하는지 여부를 제어할 수 있습니다. 이 속성은 열거형 멤버로 설정 해야 합니다 `ClearButtonVisibility` .

- `Never`지우기 단추가 표시 되지 않는 것을 나타냅니다. 이 값은 `Entry.ClearButtonVisibility` 속성의 기본값입니다.
- `WhileEditing`에 [`Entry`](xref:Xamarin.Forms.Entry) 포커스와 텍스트를 포함 하는 동안 지우기 단추가 표시 됨을 나타냅니다.

다음 예제에서는 XAML에서 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

다음 스크린샷은 clear 단추가 활성화 된을 보여 줍니다 [`Entry`](xref:Xamarin.Forms.Entry) .

![IOS 및 Android에서 지우기 단추가 있는 항목의 스크린샷](entry-images/entry-clear-button.png)

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가와 상호 작용할 때 표시 되는 키보드는 [`Entry`](xref:Xamarin.Forms.Entry) 속성을 통해 프로그래밍 방식으로 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 클래스의 다음 속성 중 하나로 설정할 수 있습니다 [`Keyboard`](xref:Xamarin.Forms.Keyboard) .

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
<Entry Keyboard="Chat" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
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
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>반환 키 사용자 지정

에 포커스가 있을 때 표시 되는 소프트 키보드의 반환 키 모양은 [`Entry`](xref:Xamarin.Forms.Entry) [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) 속성을 열거형의 값으로 설정 하 여 사용자 지정할 수 있습니다 [`ReturnType`](xref:Xamarin.Forms.ReturnType) .

- [`Default`](xref:Xamarin.Forms.ReturnType.Default)– 특정 반환 키가 필요 하지 않으며 플랫폼 기본값이 사용 됨을 나타냅니다.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done)– "Done" 반환 키를 나타냅니다.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go)– "Go" 반환 키를 나타냅니다.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next)– "Next" 반환 키를 나타냅니다.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search)– "검색" 반환 키를 나타냅니다.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send)– "송신" 반환 키를 나타냅니다.

다음 XAML 예제에서는 반환 키를 설정 하는 방법을 보여 줍니다.

```xaml
<Entry ReturnType="Send" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> 반환 키의 정확한 모양은 플랫폼에 따라 달라 집니다. IOS에서 반환 키는 텍스트 기반 단추입니다. 그러나 Android 및 유니버설 Windows 플랫폼에서 반환 키는 아이콘 기반 단추입니다.

반환 키를 누르면 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생 하 고 `ICommand` 속성에 의해 지정 된 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) 이 실행 됩니다. 또한 `object` 속성에 의해 지정 된는 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 매개 변수로에 전달 됩니다 `ICommand` . 명령에 대한 자세한 내용은 [명령 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)를 참조하세요.

### <a name="enabling-and-disabling-spell-checking"></a>맞춤법 검사 사용 및 사용 안 함

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)속성은 맞춤법 검사 사용 여부를 제어 합니다. 기본적으로 속성은로 설정 됩니다 `true` . 사용자가 텍스트를 입력 하면 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우 맞춤법 검사는 부정적인 경험을 제공 하므로 속성을로 설정 하 여 사용 하지 않도록 설정 해야 합니다 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) `false` .

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled)속성이로 설정 되 `false` 고 사용자 지정 키보드를 사용 하지 않는 경우 네이티브 맞춤법 검사기가 비활성화 됩니다. 그러나 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 와 같은 맞춤법 검사를 사용 하지 않도록 설정한 경우에는 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) `IsSpellCheckEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 `Keyboard` 명시적으로 사용 하지 않도록 설정 하는에 대 한 맞춤법 검사를 사용 하도록 설정할 수 없습니다.

### <a name="enabling-and-disabling-text-prediction"></a>텍스트 예측 사용 및 사용 안 함

[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)속성은 텍스트 예측과 자동 텍스트 교정이 사용 되는지 여부를 제어 합니다. 기본적으로 속성은로 설정 됩니다 `true` . 사용자가 텍스트를 입력 하면 단어 예측이 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우 텍스트 예측 및 자동 텍스트 수정이 부정적 환경을 제공 하므로 속성을로 설정 하 여 사용 하지 않도록 설정 해야 합니다 [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) `false` .

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled)속성이로 설정 되 `false` 고 사용자 지정 키보드를 사용 하지 않는 경우 텍스트 예측 및 자동 텍스트 수정을 사용할 수 없습니다. 그러나가 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 텍스트 예측을 사용 하지 않도록 설정 된 경우에는 `IsTextPredictionEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는에 대해 텍스트 예측을 사용 하도록 설정할 수 없습니다 `Keyboard` .

### <a name="colors"></a>색

다음 바인딩 가능한 속성을 통해 사용자 지정 배경 및 텍스트 색을 사용 하도록 항목을 설정할 수 있습니다.

- **Textcolor** &ndash; 텍스트의 색을 설정 합니다.
- **BackgroundColor** &ndash; 텍스트 뒤에 표시 되는 색을 설정 합니다.

각 플랫폼에서 색을 사용할 수 있도록 하려면 특별 한 주의가 필요 합니다. 각 플랫폼에는 텍스트와 배경색에 대 한 기본값이 다르기 때문에 설정 하는 경우에는 둘 다 설정 해야 하는 경우가 많습니다.

항목의 텍스트 색을 설정 하려면 다음 코드를 사용 합니다.

XAML에서:

```xaml
<Entry TextColor="Green" />
```

C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![항목 TextColor 예](entry-images/textcolor.png)

자리 표시자는 지정 된의 영향을 받지 않습니다 `TextColor` .

XAML에서 배경색을 설정 하려면 다음을 수행 합니다.

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![항목 BackgroundColor 예제](entry-images/textbackgroundcolor.png)

선택한 배경색과 텍스트 색은 각 플랫폼에서 사용할 수 있고 자리 표시자 텍스트는 모호 하지 않도록 주의 해야 합니다.

## <a name="events-and-interactivity"></a>이벤트 및 대화형 작업

항목은 두 개의 이벤트를 노출 합니다.

- [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged)&ndash;항목의 텍스트가 변경 될 때 발생 합니다. 변경 전후에 텍스트를 제공 합니다.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed)&ndash;사용자가 키보드에서 return 키를 눌러 입력을 끝낸 경우 발생 합니다.

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement)에서 상속 하는 클래스에 [`Entry`](xref:Xamarin.Forms.Entry) 도 [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 및 [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트가 있습니다.

### <a name="completed"></a>완료됨

`Completed`이벤트는 항목과의 상호 작용이 완료 될 때 대응 하는 데 사용 됩니다. `Completed`사용자가 키보드에서 return 키를 누르거나 UWP에서 Tab 키를 눌러 필드를 사용 하 여 입력을 종료할 때 발생 합니다. 이벤트에 대 한 처리기는 일반 이벤트 처리기로, 발신자 및 다음을 수행 합니다 `EventArgs` .

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

완료 된 이벤트를 XAML로 구독할 수 있습니다.

```xaml
<Entry Completed="Entry_Completed" />
```

및 c #:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

이벤트가 발생 한 후 속성에 의해 지정 된 [`Completed`](xref:Xamarin.Forms.Entry.Completed) `ICommand` [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) 이 실행 되 고 `object` 속성에 의해 지정 된이에 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 전달 됩니다 `ICommand` .

### <a name="textchanged"></a>TextChanged

`TextChanged`이벤트는 필드 내용의 변경에 대응 하는 데 사용 됩니다.

`TextChanged`의가 변경 될 때마다 발생 합니다 `Text` `Entry` . 이벤트에 대 한 처리기는의 인스턴스를 사용 `TextChangedEventArgs` 합니다. `TextChangedEventArgs``Entry` `Text` 및 속성을 통해의 이전 값과 새 값에 대 한 액세스를 제공 합니다 `OldTextValue` `NewTextValue` .

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

이 `TextChanged` 이벤트는 XAML에서 구독할 수 있습니다.

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

및 c #:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [항목 API](xref:Xamarin.Forms.Entry)
