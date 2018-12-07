---
title: Xamarin.Forms 항목
description: 이 문서에서는 Xamarin.Forms 항목 클래스를 사용 하 여 한 줄 텍스트 또는 응용 프로그램에서 암호 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
ms.openlocfilehash: 39af3b0e28bbbf9397ceece55adc330e364dcc3d
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061787"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms 항목

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_한 줄 텍스트 또는 암호 입력_

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) 한 줄 텍스트 입력에 사용 됩니다. 합니다 `Entry`같은 합니다 [ `Editor` ](xref:Xamarin.Forms.Editor) 보기에서 다양 한 키보드 형식을 지원 합니다. 또한는 `Entry` 암호 필드를 사용할 수 있습니다.

## <a name="display-customization"></a>사용자 지정 표시

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

`Entry`, 다른 텍스트 표시 보기를 표시 합니다 [ `Text` ](xref:Xamarin.Forms.Entry.Text) 속성. 이 속성을 설정 하 고 제공한 텍스트 읽기를 사용할 수 있습니다는 `Entry`합니다. 다음 예제에서는 설정 된 `Text` XAML에서 속성:

```xaml
<Entry Text="I am an Entry" />
```

C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

텍스트를 읽으려면 액세스는 `Text` C#의 속성:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> 너비를 `Entry` 설정 하 여 정의할 수 있습니다 해당 `WidthRequest` 속성입니다. 너비에 종속 되지 않는 `Entry` 의 값에 따라 정의 되 고 해당 `Text` 속성입니다.

### <a name="limiting-input-length"></a>입력된 길이 제한

합니다 [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 에 허용 되는 입력된 길이도 제한 속성을 사용할 수 있습니다 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 이 속성을 양수로 설정 되어야 합니다.

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 속성 값 0은 입력 하지 않고 사용할 수 있습니다 값 `int.MaxValue`에 대 한 기본 값인는 [ `Entry` ](xref:Xamarin.Forms.Entry), 중임을 나타냅니다 없습니다 효과적인 입력할 수 있는 문자 수가 제한 됩니다.

### <a name="setting-the-cursor-position-and-text-selection-length"></a>커서 위치 및 텍스트 선택 길이 설정합니다.

합니다 [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) 속성을 반환 하거나 설정 위치는 다음 문자를 삽입할에 저장 된 문자열을 사용할 수 있습니다 합니다 [ `Text` ](xref:Xamarin.Forms.Entry.Text) 속성:

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

기본값을 [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) 속성은 텍스트의 시작 부분에 삽입 되는 여부를 나타내는 0을 `Entry`입니다.

또한 합니다 [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) 를 반환 하거나 선택한 텍스트의 길이 설정 하려면 속성을 사용할 수 있습니다는 `Entry`:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

기본값은 [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) 속성이 0으로, 텍스트가 선택 되어 있는지를 나타냅니다.

### <a name="customizing-the-keyboard"></a>키보드 사용자 지정

사용자가 상호 작용할 때 표시 되는 키보드를 [ `Entry` ](xref:Xamarin.Forms.Entry) 를 통해 프로그래밍 방식으로 설정할 수 있습니다 합니다 [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) 합니다 에서다음속성중하나에속성을[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 클래스:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – 문자 보내기에 대 한 사용을 모 지 유용 합니다.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – 기본 키보드를 합니다.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) -전자 메일 주소를 입력할 때 사용 합니다.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) -숫자를 입력할 때 사용 합니다.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – 사용 하지 않고 텍스트를 입력할 때 [ `KeyboardFlags` ](xref:Xamarin.Forms.KeyboardFlags) 지정 합니다.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – 전화 번호를 입력할 때 사용 합니다.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – 텍스트를 입력할 때 사용 합니다.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – 사용 파일 경로 & 웹 주소를 입력 합니다.

이렇게 하려면 XAML에서 다음과 같이 합니다.

```xaml
<Entry Keyboard="Chat" />
```

해당 하는 C# 코드가입니다.

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

각 키보드의 예제를 찾을 수 있습니다 우리의 [레시피](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) 리포지토리.

합니다 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 클래스에는 [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) 키보드 첫 글자를 대문자로, spellcheck, 및 제안 동작을 지정 하 여 사용자 지정 하는 팩터리 메서드. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) 사용자 지정 된 메서드에 인수로 지정 된 열거형 값 `Keyboard` 반환 합니다. `KeyboardFlags` 열거형 다음 값을 포함 합니다.

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – 기능이 없는 키보드에 추가 됩니다.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) -입력 한 각 문장의 첫 번째 단어의 첫 번째 문자는 자동으로 대문자 여야 나타냅니다.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) -입력 한 텍스트에 해당 맞춤법 검사를 수행할지를 나타냅니다.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) -입력 한 텍스트 완성 제공 되는 해당 단어를 나타냅니다.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – 각 단어의 첫 번째 문자는 자동으로 대문자 여야 나타냅니다.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – 모든 문자를 자동으로 대문자를 나타냅니다.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – 없습니다 자동 대/소문자가 발생 함을 나타냅니다.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – spellcheck, 단어 완성 및 문장의 첫 글자를 대문자로 입력 한 텍스트의 발생을 나타냅니다.

다음 XAML 코드 예제에서는 기본 사용자 지정 하는 방법을 보여 줍니다 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 단어 완성을 제공 하 고 모든 입력된 문자를 대문자로 표시 하 고 있습니다.:

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

해당 하는 C# 코드가입니다.

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Return 키를 사용자 지정

즉 소프트 키보드에서 return 키의 모양을 때 표시 되는 [ `Entry` ](xref:Xamarin.Forms.Entry) 포커스가, 설정 하 여 사용자 지정할 수 있습니다 합니다 [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) 속성 값을 [ `ReturnType` ](xref:Xamarin.Forms.ReturnType) 열거형:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) – 특정 반환 키 없음 필수임을 나타내고 된 플랫폼 기본값이 사용 됩니다.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) –를 "완료" return 키를 나타냅니다.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) -"이동" return 키를 나타냅니다.
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) – "다음" return 키를 나타냅니다.
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) – "검색" return 키를 나타냅니다.
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) – "송신" return 키를 나타냅니다.

다음 XAML 예제에는 return 키를 설정 하는 방법을 보여 줍니다.

```xaml
<Entry ReturnType="Send" />
```

해당 하는 C# 코드가입니다.

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Return 키의 정확한 모양을 플랫폼에 따라 달라 집니다. IOS에서 return 키 텍스트를 기반으로 단추입니다. 그러나 Android 및 유니버설 Windows 플랫폼에서 return 키는 아이콘 기반 단추.

Return 키를 누르면 합니다 [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) 이벤트가 발생 하 고 모든 `ICommand` 에 지정 된는 [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) 속성 실행 됩니다. 또한 모든 `object` 에 지정 된 합니다 [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 속성에 전달 될는 `ICommand` 매개 변수로 합니다. 명령에 대 한 자세한 내용은 참조 하세요. [The 명령 인터페이스](~/xamarin-forms/app-fundamentals/data-binding/commanding.md)합니다.

### <a name="enabling-and-disabling-spell-checking"></a>설정 및 맞춤법 검사 사용 안 함

합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성 컨트롤 여부 맞춤법 검사 사용 가능 합니다. 기본적으로 속성 설정 `true`합니다. 사용자가 텍스트를 입력 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 등의 일부 텍스트 항목 시나리오에 대 한 맞춤법 검사 음수 환경을 제공 하 고 설정 하 여 비활성화 해야 합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성을 `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> 경우는 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`, 사용자 지정 키보드를 사용 하지 않을, 네이티브 맞춤법 검사기를 사용할 수 없습니다. 그러나 경우에 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 가 된 맞춤법 검사를 사용 하지 않도록 설정 하는 집합 검사와 같은 [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` 속성은 무시 됩니다. 에 대 한 맞춤법 검사를 사용 하도록 설정 하려면 속성을 사용할 수 없습니다 따라서는 `Keyboard` 는 명시적으로 사용 되지 않습니다.

### <a name="enabling-and-disabling-text-prediction"></a>설정 및 텍스트 자동 완성을 사용 하지 않도록 설정

합니다 [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성 컨트롤 여부를 텍스트 예측 및 자동 텍스트 보정을 사용할 수 있습니다. 기본적으로 속성 설정 `true`합니다. 사용자가 텍스트를 입력 하는 대로 word 예측 표시 됩니다.

그러나 일부 텍스트 항목 시나리오에 대 한 사용자 이름, 텍스트 자동 완성 및 자동 텍스트를 입력 하는 등 수정 음수 환경을 제공 하 고 설정 하 여 비활성화 해야 합니다 [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성을 `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> 경우는 [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) 속성 `false`,이 고 사용자 지정 키보드 가장 되지 않습니다 텍스트 자동 완성 및 자동 사용 텍스트 수정 되지 합니다. 그러나 경우는 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 사용 하지 않도록 설정 텍스트 예측, 설정한는 `IsTextPredictionEnabled` 속성은 무시 됩니다. 에 대 한 텍스트 자동 완성을 사용 하도록 설정 하려면 속성을 사용할 수 없습니다 따라서는 `Keyboard` 는 명시적으로 사용 되지 않습니다.

### <a name="setting-placeholder-text"></a>자리 표시자 텍스트를 설정합니다.

합니다 [ `Entry` ](xref:Xamarin.Forms.Entry) 사용자 입력을 저장 하지 않는 경우 자리 표시자 텍스트를 표시 하도록 설정할 수 있습니다. 설정 하 여 이렇게를 [ `Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) 속성을을 `string`에 적절 한 콘텐츠의 형식을 지정 하기 위해 자주 사용 되는 `Entry`합니다. 자리 표시자 텍스트 색을 설정 하 여 제어할 수 또한 합니다 [ `PlaceholderColor` ](xref:Xamarin.Forms.Entry.PlaceholderColor) 속성을 [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

### <a name="password-fields"></a>암호 필드

`Entry` 제공 된 `IsPassword` 속성입니다. 때 `IsPassword` 는 `true`, 필드의 내용을 black 원으로 표시 됩니다.

XAML:

```xaml
<Entry IsPassword="true" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "항목 IsPassword 예")

인스턴스를 사용 하 여 자리 표시자를 사용할 수 있습니다 `Entry` 암호 필드로 구성 된:

XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "항목 IsPassword 및 자리 표시자 예제")

### <a name="colors"></a>색

항목은 사용자 지정 배경 및 텍스트 색의 다음 바인딩 가능한 속성을 통해 사용 하도록 설정할 수 있습니다.

- **TextColor** &ndash; 텍스트의 색을 설정 합니다.
- **BackgroundColor** &ndash; 텍스트 뒤에 표시 된 색을 설정 합니다.

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

![](entry-images/textcolor.png "항목 TextColor 예")

지정 된 자리 표시자 하지는 영향을 받는 `TextColor`합니다.

XAML의 배경색을 설정:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "항목의 BackgroundColor 예")

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 하 고 자리 표시자 텍스트를가 려 서 없는 되도록 주의 해야 합니다.

## <a name="events-and-interactivity"></a>이벤트 및 대화형 작업

두 이벤트를 노출 하는 항목:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; 항목의 텍스트 변경 될 때 발생 합니다. 전과 변경 후에 텍스트를 제공합니다.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; 사용자 입력 키보드의 return 키를 눌러 종료 될 때 발생 합니다.

### <a name="completed"></a>완료

`Completed` 이벤트 진입점와의 상호 작용의 완료에 대응 하는 데 사용 됩니다. `Completed` 키보드에서 return 키를 눌러 필드를 사용 하 여 입력을 종료할 때 발생 합니다. 이벤트 처리기는 보낸 사람을 수행 하 여 제네릭 이벤트 처리기를 및 `EventArgs`:

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

후 합니다 [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) 어떤 이벤트가 발생 `ICommand` 에 지정 된를 [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) 속성 실행 되 면 사용 하 여를 `object` 에 지정 된는 [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) 속성에 전달 되는 `ICommand`합니다.

### <a name="textchanged"></a>TextChanged

`TextChanged` 이벤트 필드의 내용 변경에 대응 하는 데 사용 됩니다.

`TextChanged` 때마다 발생 합니다 `Text` 의 `Entry` 변경 합니다. 인스턴스를 사용 하는 이벤트에 대 한 처리기 `TextChangedEventArgs`합니다. `TextChangedEventArgs` 이전 및 새 값에 대 한 액세스를 제공 합니다 `Entry` `Text` 를 통해 합니다 `OldTextValue` 및 `NewTextValue` 속성:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` XAML에서 이벤트를 구독할 수 있습니다.

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

및 C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>관련 링크

- [텍스트 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [API 항목](xref:Xamarin.Forms.Entry)
