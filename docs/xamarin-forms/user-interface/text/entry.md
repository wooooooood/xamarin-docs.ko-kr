---
title: Xamarin.Forms 항목
description: 이 문서에서는 Xamarin.Forms 항목 클래스를 사용 하 여 한 줄 텍스트 또는 응용 프로그램에서 암호 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 95afdfde878759d4a598e200d16fe6fb1fa2005e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998247"
---
# <a name="xamarinforms-entry"></a>Xamarin.Forms 항목

_한 줄 텍스트 또는 암호 입력_

Xamarin.Forms `Entry` 한 줄 텍스트 입력에 사용 됩니다. 합니다 `Entry`같은 `Editor` 보기에서 다양 한 키보드 형식을 지원 합니다. 또한는 `Entry` 암호 필드를 사용할 수 있습니다.

## <a name="display-customization"></a>사용자 지정 표시

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

`Entry`, 다른 텍스트 표시 보기를 노출 합니다 `Text` 속성입니다. 이 속성을 설정 하 고 제공한 텍스트 읽기를 사용할 수 있습니다는 `Entry`합니다. 다음 예제에서는 설정 된 `Text` XAML에서 속성:

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

### <a name="keyboards"></a>키보드

사용자가 상호 작용할 때 표시 되는 키보드를 `Entry` 를 통해 프로그래밍 방식으로 설정할 수 있습니다는 `Keyboard` 속성입니다.

키보드 형식에 대 한 옵션은 같습니다.

- **기본** &ndash; 기본 키보드
- **채팅** &ndash; 문자 보내기 및 위치에 사용 되는 모 지 유용
- **전자 메일** &ndash; 전자 메일 주소를 입력할 때 사용
- **숫자** &ndash; 숫자를 입력할 때 사용
- **전화** &ndash; 전화 번호를 입력할 때 사용
- **Url** &ndash; 파일 경로 및 웹 주소를 입력 하는 데

[예제에서는 각 키보드](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) 레시피 섹션에서.

### <a name="enabling-and-disabling-spell-checking"></a>설정 및 맞춤법 검사 사용 안 함

합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성 컨트롤 여부 맞춤법 검사 사용 가능 합니다. 기본적으로 속성 설정 `true`합니다. 사용자가 텍스트를 입력 맞춤법 오류가 표시 됩니다.

그러나 사용자 이름을 입력 하는 등의 일부 텍스트 항목 시나리오에 대 한 맞춤법 검사에서는 음수 환경 등을 설정 하 여 비활성화 해야 합니다 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성을 `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> 경우는 [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) 속성이 `false`, 사용자 지정 키보드를 사용 하지 않을, 네이티브 맞춤법 검사기를 사용할 수 없습니다. 그러나 경우에 [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) 가 된 맞춤법 검사를 사용 하지 않도록 설정 하는 집합 검사와 같은 [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` 속성은 무시 됩니다. 에 대 한 맞춤법 검사를 사용 하도록 설정 하려면 속성을 사용할 수 없습니다 따라서는 `Keyboard` 는 명시적으로 사용 되지 않습니다.

### <a name="placeholders"></a>자리 표시자

`Entry` 사용자 입력을 저장 하지 않는 경우 자리 표시자 텍스트를 표시 하도록 설정할 수 있습니다. 실제로이 주로 표시 됩니다 형태로 지정된 된 필드에 적절 한 콘텐츠를 명확 하 게. 자리 표시자 텍스트 색을 사용자 지정할 수 없습니다과 관계 없이 동일 합니다 `TextColor` 설정 합니다. 으로 대체 해야 디자인 사용자 지정 자리 표시자 색에 대 한 호출을 하는 경우는 [사용자 지정 렌더러]()합니다. 다음 만들어집니다는 `Entry` XAML에 자리 표시자로 "Username"을 사용 하 여:

```xaml
<Entry Placeholder="Username" />
```

C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "항목 자리 표시자 예")

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

- [TextChanged](xref:Xamarin.Forms.Entry.TextChanged) &ndash; 항목의 텍스트 변경 될 때 발생 합니다. 전과 변경 후에 텍스트를 제공합니다.
- [완료](xref:Xamarin.Forms.Entry.Completed) &ndash; 사용자 입력 키보드의 return 키를 눌러 종료 될 때 발생 합니다.

### <a name="completed"></a>완료

`Completed` 이벤트 진입점와의 상호 작용의 완료에 대응 하는 데 사용 됩니다. `Completed` 키보드에서 return 키를 입력 하 여 필드를 사용 하 여 입력을 종료할 때 발생 합니다. 이벤트 처리기는 보낸 사람을 수행 하 여 제네릭 이벤트 처리기를 및 `EventArgs`:

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
