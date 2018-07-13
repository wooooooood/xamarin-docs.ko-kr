---
title: Xamarin.Forms 편집기
description: 이 문서에서는 Xamarin.Forms 편집기 컨트롤을 사용 하 여 응용 프로그램에서 여러 줄 텍스트 입력을 허용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 4879ff88d5bbdab5aa92024bee7f50239a141e3b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995869"
---
# <a name="xamarinforms-editor"></a>Xamarin.Forms 편집기

_여러 줄 텍스트 입력_

`Editor` 컨트롤은 여러 줄 입력을 허용 하는 데 사용 합니다. 이 문서에서는 설명 합니다.

- **[사용자 지정](#customization)**  &ndash; 키보드 및 색 옵션입니다.
- **[상호 작용](#interactivity)**  &ndash; 에 대 한 상호 작용을 위해 수신 대기할 수 있는 이벤트입니다.

## <a name="customization"></a>사용자 지정

### <a name="setting-and-reading-text"></a>텍스트를 읽고 설정

`Editor`, 다른 텍스트 표시 보기를 노출 합니다 `Text` 속성입니다. 이 속성을 설정 하 고 제공한 텍스트 읽기를 사용할 수 있습니다는 `Editor`합니다. 다음 예제에서는 설정 된 `Text` XAML에서 속성:

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

### <a name="limiting-input-length"></a>입력된 길이 제한

합니다 [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 에 허용 되는 입력된 길이도 제한 속성을 사용할 수 있습니다 합니다 [ `Editor` ](xref:Xamarin.Forms.Editor)합니다. 이 속성을 양수로 설정 되어야 합니다.

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) 속성 값 0은 입력 하지 않고 사용할 수 있습니다 값 `int.MaxValue`에 대 한 기본 값인는 [ `Editor` ](xref:Xamarin.Forms.Editor), 중임을 나타냅니다 없습니다 효과적인 입력할 수 있는 문자 수가 제한 됩니다.

### <a name="keyboards"></a>키보드

사용자가 상호 작용할 때 표시 되는 키보드를 `Editor` 를 통해 프로그래밍 방식으로 설정할 수 있습니다 합니다 [ ``Keyboard`` ](xref:Xamarin.Forms.Keyboard) 속성입니다.

키보드 형식에 대 한 옵션은 같습니다.

- **기본** &ndash; 기본 키보드
- **채팅** &ndash; 문자 보내기 및 위치에 사용 되는 모 지 유용
- **전자 메일** &ndash; 전자 메일 주소를 입력할 때 사용
- **숫자** &ndash; 숫자를 입력할 때 사용
- **전화** &ndash; 전화 번호를 입력할 때 사용
- **Url** &ndash; 파일 경로 & 웹 주소를 입력 하는 데

[예제에서는 각 키보드](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) 레시피 섹션에서.

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

### <a name="completed"></a>완료

합니다 `Completed` 이벤트와의 상호 작용을 완료 하 react를 사용 하는 `Editor`합니다. `Completed` 키보드에서 return 키를 입력 하 여 필드를 사용 하 여 입력을 종료할 때 발생 합니다. 이벤트 처리기는 보낸 사람을 수행 하 여 제네릭 이벤트 처리기를 및 `EventArgs`:

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
