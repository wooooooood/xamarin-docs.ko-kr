---
title: 입력
description: 한 줄 텍스트 또는 암호 입력
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 2e40effa7bc54b7b7cf73edaa882256fed521a95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="entry"></a>입력

_한 줄 텍스트 또는 암호 입력_

Xamarin.Forms `Entry` 한 줄 텍스트 입력에 사용 됩니다. `Entry`를 편집기 보기와 같은 다양 한 키보드 형식을 지원 합니다. 또한 `Entry` 암호 필드로 사용할 수 있습니다.

## <a name="display-customization"></a>사용자 지정 표시

### <a name="setting-and-reading-text"></a>설정 하 고 텍스트 읽기

항목 텍스트 표시를 다른 뷰와 마찬가지로 노출 된 `Text` 속성입니다. `Text` 설정 하 고 제공한 텍스트를 읽는 데 사용할 수는 `Entry`합니다. 다음 예제에서는 XAML의 텍스트를 설정 하는 방법을 보여 줍니다.

```xaml
<Entry Text="I am an Entry" />
```

C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

텍스트를 읽기 액세스는 `Text` C#의 속성:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> 너비는 `Entry` 설정 하 여 정의할 수는 `WidthRequest` 속성입니다. 너비에 의존 하지 않는 한 `Entry` 의 값에 따라 정의 되 고 해당 `Text` 속성입니다.

### <a name="keyboards"></a>키보드

사용자가 상호 작용할 때 표시 되는 키보드는 `Entry` 를 통해 프로그래밍 방식으로 설정할 수는 `Keyboard` 속성입니다.

키보드 종류에 대 한 옵션이 있습니다.

- **기본** &ndash; 기본 키보드
- **채팅** &ndash; 문자 보내기 & 장소에 사용 되는 emoji는 유용
- **전자 메일** &ndash; 전자 메일 주소를 입력할 때 사용
- **숫자** &ndash; 숫자를 입력할 때 사용
- **전화** &ndash; 전화 번호를 입력할 때 사용
- **Url** &ndash; 파일 경로 웹 주소를 입력 하는 데

한 [각 키보드의 예](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) 레시피 섹션에서.

### <a name="placeholders"></a>자리 표시자

`Entry` 사용자 입력을 저장 하지 않는 경우 개체 틀 텍스트를 표시 하도록 설정할 수 있습니다. 실제로이 종종 발견 됩니다 forms 된 특정된 필드에 적절 한 콘텐츠를 명확 하 게 합니다. 자리 표시자 텍스트 색을 사용자 지정할 수 없습니다 및 것에 관계 없이 동일한는 `TextColor` 설정 합니다. 으로 대체 해야 디자인을 사용자 지정 개체 틀 색에 대 한 호출을 [사용자 지정 렌더러]()합니다. 다음을 만듭니다는 `Entry` XAML에서 자리 표시자로 "username":

```xaml
<Entry Placeholder="Username" />
```

C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "항목 자리 표시자 예")

### <a name="password-fields"></a>암호 필드

`Entry` 제공 된 `IsPassword` 속성입니다. 때 `IsPassword` 은 `true`, 필드의 내용을 검정 원으로 표시 됩니다.

Xaml의 경우:

```xaml
<Entry IsPassword="true" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "항목 IsPassword 예")

자리 표시자의 인스턴스와 함께 사용할 수 있습니다 `Entry` 암호 필드로 구성 된:

Xaml의 경우:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "항목 IsPassword 및 자리 표시자 예제")


### <a name="colors"></a>색

항목은 사용자 지정 배경 및 텍스트 색이 다음과 같은 바인딩 가능 속성을 통해 사용 하도록 설정할 수 있습니다.

- **TextColor** &ndash; 텍스트의 색을 설정 합니다.
- **BackgroundColor** &ndash; 텍스트 뒤에 표시 된 색을 설정 합니다.

특별 한 주의 되 색 각 플랫폼에서 사용할 수 있는지 확인 해야 합니다. 각 플랫폼에 대 한 텍스트 색과 배경색은 다른 기본값을 사용 하므로 하나를 설정 하는 경우 모두 설정 해야 경우가 많습니다.

다음 코드를 사용 하 여 항목의 텍스트 색을 설정 하려면:

Xaml의 경우:

```xaml
<Entry TextColor="Green" />
```

C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "항목 TextColor 예")

지정 된 영향을 자리 표시자 하지 않습니다 `TextColor`합니다.

설정 하려면 명령 프롬프트 창의 배경색 XAML에서:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "항목 BackgroundColor 예")

배경색과 텍스트 색을 선택 하면 각 플랫폼에서 사용할 수 있고 모든 자리 표시자 텍스트를가 려 서 하지 있는지 주의 해야 합니다.

## <a name="events-and-interactivity"></a>이벤트와 상호 작용

두 개의 이벤트를 노출 하는 항목:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; 텍스트 항목에 변경 될 때 발생 합니다. 변경 전후의 텍스트를 제공 합니다.
- [완료](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; 키보드의 return 키를 눌러 사용자가 입력 종료 된 경우 발생 합니다.

### <a name="completed"></a>완료

`Completed` 이벤트 항목과 상호 작용의 완료에 반응 하는 데 사용 됩니다. `Completed` 사용자가 키보드를 return 키를 입력 하 여 필드를 사용 하 여 입력 끝날 때 발생 합니다. 이벤트에 대 한 처리기가 보낸 라인으로 전환 하는 일반 이벤트 처리기를 및 `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

XAML에서 완료 된 이벤트를 구독할 수 있습니다.

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

`TextChanged` 될 때마다 발생 하는 `Text` 의 `Entry` 변경 합니다. 인스턴스를 사용 하는 이벤트에 대 한 처리기 `TextChangedEventArgs`합니다. `TextChangedEventArgs` 이전 및 새 값에 액세스할 수는 `Entry` `Text` 통해는 `OldTextValue` 및 `NewTextValue` 속성:

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
- [API 항목](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
