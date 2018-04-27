---
title: 편집기
description: 여러 줄 텍스트 입력
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: dc83defcb3eb69cf53c205793ce77029c0947c2f
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/27/2018
---
# <a name="editor"></a>편집기

_여러 줄 텍스트 입력_

`Editor` 컨트롤은 여러 줄 입력을 허용 하는 데 사용 됩니다. 이 문서에서는 설명 합니다.

- **[사용자 지정](#Customization)**  &ndash; 키보드 및 색 옵션입니다.
- **[상호 작용](#Interactivity)**  &ndash; 대화형 작업을 제공 하기에 대 한 수신 대기할 수 있는 이벤트입니다.

## <a name="customization"></a>사용자 지정

### <a name="setting-and-reading-text"></a>설정 하 고 텍스트 읽기

편집기에서 텍스트 표시를 다른 뷰와 마찬가지로 노출 된 `Text` 속성입니다. `Text` 설정 하 고 제공한 텍스트를 읽는 데 사용할 수는 `Editor`합니다. 다음 예제에서는 XAML에서는 텍스트를 설정을 보여 줍니다.

```xaml
<Editor Text="I am an Editor" />
```

C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

텍스트를 읽기 액세스는 `Text` C#의 속성:

```csharp
var text = MyEditor.Text;
```

### <a name="keyboards"></a>키보드

사용자가 상호 작용할 때 표시 되는 키보드는 `Editor` 를 통해 프로그래밍 방식으로 설정할 수는 [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) 속성입니다.

키보드 종류에 대 한 옵션이 있습니다.

- **기본** &ndash; 기본 키보드
- **채팅** &ndash; 문자 보내기 & 장소에 사용 되는 emoji는 유용
- **전자 메일** &ndash; 전자 메일 주소를 입력할 때 사용
- **숫자** &ndash; 숫자를 입력할 때 사용
- **전화** &ndash; 전화 번호를 입력할 때 사용
- **Url** &ndash; 파일 경로 및 웹 주소를 입력 하는 데

한 [각 키보드의 예](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) 레시피 섹션에서.

### <a name="colors"></a>색

`Editor` 통해 사용자 지정 배경 색을 사용 하도록 설정할 수 있습니다는 `BackgroundColor` 속성입니다. 특별 한 주의 되 색 각 플랫폼에서 사용할 수 있는지 확인 해야 합니다. 각 플랫폼에 대 한 텍스트 색은 다른 기본값을 사용 하므로 각 플랫폼에 대 한 사용자 지정 배경 색을 설정 해야 합니다. 참조 [플랫폼 Tweaks 작업할](~/xamarin-forms/platform/device.md) 각 플랫폼에 대 한 UI를 최적화 하는 방법에 대 한 자세한 내용은 합니다.

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

Xaml의 경우:

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

![](editor-images/textbackgroundcolor.png "편집기 BackgroundColor 예제로")

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 없습니다 하며 모든 자리 표시자 텍스트를가 려 서 하지 않습니다 확인 합니다.

## <a name="interactivity"></a>대화형 작업

`Editor` 두 개의 이벤트를 노출합니다.

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; 텍스트 편집기에서 변경 될 때 발생 합니다. 변경 전후의 텍스트를 제공 합니다.
- [완료](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; 키보드의 return 키를 눌러 사용자가 입력 종료 된 경우 발생 합니다.

### <a name="completed"></a>완료

`Completed` 이벤트와 상호 작용의 완료에 대응 하는 프로그램 `Editor`합니다. `Completed` 사용자가 키보드를 return 키를 입력 하 여 필드를 사용 하 여 입력 끝날 때 발생 합니다. 이벤트에 대 한 처리기가 보낸 라인으로 전환 하는 일반 이벤트 처리기를 및 `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

코드와 XAML에서 완료 이벤트를 구독할 수 있습니다.

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

Xaml의 경우:

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

`TextChanged` 될 때마다 발생 하는 `Text` 의 `Editor` 변경 합니다. 인스턴스를 사용 하는 이벤트에 대 한 처리기 `TextChangedEventArgs`합니다. `TextChangedEventArgs` 이전 및 새 값에 액세스할 수는 `Editor` `Text` 통해는 `OldTextValue` 및 `NewTextValue` 속성:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

코드와 XAML에서 완료 이벤트를 구독할 수 있습니다.

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

Xaml의 경우:

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
- [편집기 API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
