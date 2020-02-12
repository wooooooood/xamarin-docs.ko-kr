---
title: Xamarin.Forms 항목
description: 이 문서에서는 Xamarin.Forms 항목 클래스를 사용 하 여 한 줄 텍스트 또는 응용 프로그램에서 암호 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/25/2019
ms.openlocfilehash: c7daad8898d3048f10c9489ee4e2c78f147c6e52
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77131143"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms 항목

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_한 줄 텍스트 또는 암호 입력_

Xamarin.ios [`Entry`](xref:Xamarin.Forms.Entry) 는 한 줄 텍스트 입력에 사용 됩니다. [`Editor`](xref:Xamarin.Forms.Editor) 뷰와 같은 `Entry`는 여러 키보드 유형을 지원 합니다. 또한 `Entry`를 암호 필드로 사용할 수 있습니다.

## <a name="display-customization"></a>사용자 지정 표시

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

다른 텍스트 표시 뷰와 마찬가지로 `Entry`[`Text`](xref:Xamarin.Forms.InputView.Text) 속성을 노출 합니다. 이 속성을 사용 하 여 `Entry`표시 되는 텍스트를 설정 하 고 읽을 수 있습니다. 다음 예제에서는 XAML에서 `Text` 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Entry Text="I am an Entry" />
```

C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

텍스트를 읽으려면의 C#`Text` 속성에 액세스 합니다.

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트를 설정합니다.

사용자 입력을 저장 하지 않을 경우 자리 표시자 텍스트를 표시 하도록 [`Entry`](xref:Xamarin.Forms.Entry) 를 설정할 수 있습니다. 이렇게 하려면 [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) 속성을 `string`설정 하 고 `Entry`에 적절 한 콘텐츠 형식을 나타내는 데 자주 사용 됩니다. 또한 [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) 속성을 [`Color`](xref:Xamarin.Forms.Color)설정 하 여 자리 표시자 텍스트 색을 제어할 수 있습니다.

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> `Entry`의 너비는 `WidthRequest` 속성을 설정 하 여 정의할 수 있습니다. `Text` 속성의 값에 따라 정의 되는 `Entry`의 너비에 따라 달라 집니다.

### <a name="preventing-text-entry"></a>텍스트 입력 방지

사용자는 기본값인 `false`인 `IsReadOnly` 속성을 `true`로 설정 하 여 [`Entry`](xref:Xamarin.Forms.Entry) 의 텍스트를 수정할 수 없습니다.

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly` 속성은 `Entry`의 시각적 모양을 회색으로 변경 하는 `IsEnabled` 속성과 달리 [`Entry`](xref:Xamarin.Forms.Entry)의 시각적 모양을 변경 하지 않습니다.

### <a name="limiting-input-length"></a>입력된 길이 제한

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성을 사용 하 여 [`Entry`](xref:Xamarin.Forms.Entry)에 대해 허용 되는 입력 길이를 제한할 수 있습니다. 이 속성을 양수로 설정 되어야 합니다.

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

[`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) 속성 값이 0 이면 입력이 허용 되지 않고 [`Entry`](xref:Xamarin.Forms.Entry)에 대 한 기본값인 `int.MaxValue`값이 입력 될 수 있는 문자 수에 대 한 유효 한도가 없음을 나타냅니다.

### <a name="character-spacing"></a>문자 간격

`Entry.CharacterSpacing` 속성을 `double` 값으로 설정 하 여 [`Entry`](xref:Xamarin.Forms.Entry) 에 문자 간격을 적용할 수 있습니다.

```xaml
<Entry ...
       CharacterSpacing="10" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
Entry entry = new Entry { CharacterSpacing = 10 };
```

그 결과 [`Entry`](xref:Xamarin.Forms.Entry) 표시 되는 텍스트의 문자는 장치 독립적 단위 `CharacterSpacing` 간격이 떨어져 있습니다.

> [!NOTE]
> `CharacterSpacing` 속성 값은 `Text` 및 `Placeholder` 속성에 의해 표시 되는 텍스트에 적용 됩니다.

### <a name="password-fields"></a>암호 필드

`Entry` `IsPassword` 속성을 제공 합니다. `IsPassword` `true`되 면 필드의 내용이 검정 원으로 표시 됩니다.

XAML:

```xaml
<Entry IsPassword="true" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![항목 IsPassword 예제](entry-images/password.png)

자리 표시자는 암호 필드로 구성 된 `Entry`의 인스턴스와 함께 사용할 수 있습니다.

XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![항목 IsPassword 및 자리 표시자 예제](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>커서 위치 및 텍스트 선택 길이 설정합니다.

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) 속성을 사용 하 여 [`Text`](xref:Xamarin.Forms.InputView.Text) 속성에 저장 된 문자열에 다음 문자를 삽입할 위치를 반환 하거나 설정할 수 있습니다.

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

[`CursorPosition`](xref:Xamarin.Forms.Entry.CursorPosition) 속성의 기본값은 0 이며,이 값은 텍스트가 `Entry`시작 부분에 삽입 됨을 나타냅니다.

또한 [`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) 속성을 사용 하 여 `Entry`내에서 텍스트 선택의 길이를 반환 하거나 설정할 수 있습니다.

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

[`SelectionLength`](xref:Xamarin.Forms.Entry.SelectionLength) 속성의 기본값은 0으로, 선택 된 텍스트가 없음을 나타냅니다.

### <a name="displaying-a-clear-button"></a>지우기 단추 표시

`ClearButtonVisibility` 속성을 사용 하 여 사용자가 텍스트를 지울 수 있도록 하는 [`Entry`](xref:Xamarin.Forms.Entry) 지우기 단추를 표시할지 여부를 제어할 수 있습니다. 이 속성은 `ClearButtonVisibility` 열거형 멤버로 설정 해야 합니다.

- `Never`은 지우기 단추가 표시 되지 않음을 나타냅니다. 이 값은 `Entry.ClearButtonVisibility` 속성의 기본값입니다.
- `WhileEditing`는 포커스와 텍스트를 포함 하는 동안 [`Entry`](xref:Xamarin.Forms.Entry)에 clear 단추가 표시 됨을 나타냅니다.

다음 예제에서는 XAML에서 속성을 설정 하는 방법을 보여 줍니다.

```xaml
<Entry Text="Xamarin.Forms"
       ClearButtonVisibility="WhileEditing" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { Text = "Xamarin.Forms", ClearButtonVisibility = ClearButtonVisibility.WhileEditing };
```

다음 스크린샷은 clear 단추가 사용 하도록 설정 된 [`Entry`](xref:Xamarin.Forms.Entry) 를 보여 줍니다.

![IOS 및 Android에서 지우기 단추가 있는 항목의 스크린샷](entry-images/entry-clear-button.png)

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가 [`Entry`](xref:Xamarin.Forms.Entry) 와 상호 작용할 때 제공 되는 키보드는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 클래스에서 다음 속성 중 하나로 [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) 속성을 통해 프로그래밍 방식으로 설정할 수 있습니다.

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

#### <a name="customizing-the-return-key"></a>Return 키를 사용자 지정

[`Entry`](xref:Xamarin.Forms.Entry) 포커스가 있을 때 표시 되는 소프트 키보드의 반환 키 모양은 [`ReturnType`](xref:Xamarin.Forms.Entry.ReturnType) 속성을 [`ReturnType`](xref:Xamarin.Forms.ReturnType) 열거형 값으로 설정 하 여 사용자 지정할 수 있습니다.

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – 특정 반환 키가 필요 하지 않으며 플랫폼 기본값이 사용 됨을 나타냅니다.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) – "Done" 반환 키를 나타냅니다.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) – "Go" 반환 키를 나타냅니다.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – "Next" 반환 키를 나타냅니다.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – "검색" 반환 키를 나타냅니다.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – "송신" 반환 키를 나타냅니다.

다음 XAML 예제에는 return 키를 설정 하는 방법을 보여 줍니다.

```xaml
<Entry ReturnType="Send" />
```

해당하는 C# 코드는 다음과 같습니다.

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Return 키의 정확한 모양을 플랫폼에 따라 달라 집니다. IOS에서 return 키 텍스트를 기반으로 단추입니다. 그러나 Android 및 유니버설 Windows 플랫폼에서 return 키는 아이콘 기반 단추.

반환 키를 누르면 [`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생 하 고 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) 속성으로 지정 된 모든 `ICommand` 실행 됩니다. 또한 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 속성으로 지정 된 모든 `object`는 `ICommand` 매개 변수로 전달 됩니다. 명령에 대한 자세한 내용은 [명령 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)를 참조하세요.

### <a name="enabling-and-disabling-spell-checking"></a>설정 및 맞춤법 검사 사용 안 함

[`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성은 맞춤법 검사 사용 여부를 제어 합니다. 기본적으로 속성은 `true`로 설정 됩니다. 사용자가 텍스트를 입력 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 것과 같은 일부 텍스트 입력 시나리오의 경우 맞춤법 검사는 부정적인 환경을 제공 하며 [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성을 `false`로 설정 하 여 사용 하지 않도록 설정 해야 합니다.

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`로 설정 되 고 사용자 지정 키보드를 사용 하지 않는 경우 네이티브 맞춤법 검사기가 비활성화 됩니다. 그러나 [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat)와 같이 맞춤법 검사를 사용 하지 않도록 설정 하는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 설정 된 경우에는 `IsSpellCheckEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는 `Keyboard`에 대 한 맞춤법 검사를 사용 하도록 설정할 수 없습니다.

### <a name="enabling-and-disabling-text-prediction"></a>설정 및 텍스트 자동 완성을 사용 하지 않도록 설정

[`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성은 텍스트 예측 및 자동 텍스트 수정을 사용 하도록 설정할지 여부를 제어 합니다. 기본적으로 속성은 `true`로 설정 됩니다. 사용자가 텍스트를 입력 하는 대로 word 예측 표시 됩니다.

그러나 사용자 이름을 입력 하는 경우와 같은 일부 텍스트 입력 시나리오에서는 텍스트 예측 및 자동 텍스트 수정이 부정적 환경을 제공 하므로 [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성을 `false`으로 설정 하 여 사용 하지 않도록 설정 해야 합니다.

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성이 `false`로 설정 되어 있고 사용자 지정 키보드를 사용 하지 않는 경우 텍스트 예측 및 자동 텍스트 수정을 사용할 수 없습니다. 그러나 텍스트 예측을 사용 하지 않도록 설정 하는 [`Keyboard`](xref:Xamarin.Forms.Keyboard) 설정 된 경우에는 `IsTextPredictionEnabled` 속성이 무시 됩니다. 따라서 속성을 사용 하 여 명시적으로 사용 하지 않도록 설정 하는 `Keyboard`에 대해 텍스트 예측을 사용 하도록 설정할 수 없습니다.

### <a name="colors"></a>색

항목은 사용자 지정 배경 및 텍스트 색의 다음 바인딩 가능한 속성을 통해 사용 하도록 설정할 수 있습니다.

- **Textcolor** &ndash; 텍스트의 색을 설정 합니다.
- **BackgroundColor** &ndash; 텍스트 뒤에 표시 되는 색을 설정 합니다.

특별 한 주의 색 각 플랫폼에서 사용할 수 있는지 확인 하는 데 필요한 경우 각 플랫폼에는 텍스트 및 배경색에 대 한 다른 기본값을 하나 설정 하는 경우 모두 설정 해야 경우가 많습니다.

항목의 텍스트 색을 설정 하려면 다음 코드를 사용 합니다.

XAML:

```xaml
<Entry TextColor="Green" />
```

C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![항목 TextColor 예](entry-images/textcolor.png)

자리 표시자는 지정 된 `TextColor`의 영향을 받지 않습니다.

XAML의 배경색을 설정:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![항목 BackgroundColor 예제](entry-images/textbackgroundcolor.png)

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 하 고 자리 표시자 텍스트를가 려 서 없는 되도록 주의 해야 합니다.

## <a name="events-and-interactivity"></a>이벤트 및 대화형 작업

두 이벤트를 노출 하는 항목:

- 항목의 텍스트가 변경 될 때 발생 하는 [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) &ndash;입니다. 전과 변경 후에 텍스트를 제공합니다.
- 사용자가 키보드에서 return 키를 눌러 입력을 종료 하면 [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; 발생 합니다.

> [!NOTE]
> [`Entry`](xref:Xamarin.Forms.Entry) 를 상속 하는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스에도 [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) 및 [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused) 이벤트가 있습니다.

### <a name="completed"></a>Completed

`Completed` 이벤트는 항목과의 상호 작용이 완료 될 때 대응 하는 데 사용 됩니다. 사용자가 키보드에서 return 키를 누르거나 UWP에서 Tab 키를 눌러 필드에 입력을 끝내 면 `Completed` 발생 합니다. 이벤트에 대 한 처리기는 일반 이벤트 처리기로, 발신자 및 `EventArgs`을 수행 합니다.

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

XAML에서 completed 이벤트를 구독할 수 있습니다.

```xaml
<Entry Completed="Entry_Completed" />
```

및 C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

[`Completed`](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생 한 후에는 [`ReturnCommand`](xref:Xamarin.Forms.Entry.ReturnCommand) 속성으로 지정 된 모든 `ICommand` 실행 되며 [`ReturnCommandParameter`](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 속성에 의해 지정 된 `object` `ICommand`으로 전달 됩니다.

### <a name="textchanged"></a>TextChanged

`TextChanged` 이벤트는 필드 내용의 변경에 대응 하는 데 사용 됩니다.

`Entry`의 `Text` 변경 될 때마다 `TextChanged` 발생 합니다. 이벤트 처리기는 `TextChangedEventArgs`의 인스턴스를 사용 합니다. `TextChangedEventArgs`는 `OldTextValue` 및 `NewTextValue` 속성을 통해 `Entry` `Text`의 이전 값과 새 값에 대 한 액세스를 제공 합니다.

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` 이벤트를 XAML로 구독할 수 있습니다.

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

및 C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [항목 API](xref:Xamarin.Forms.Entry)
