---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bb77e9fafe39bf76a7d4290dba0bc658cd15094f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140038"
---
# <a name="entry-input-method-editor-options-on-android"></a>Android에서 입력 방법 편집기 옵션 입력

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼별는에 대 한 소프트 키보드의 IME (입력기) 옵션을 설정 합니다 [`Entry`](xref:Xamarin.Forms.Entry) . 여기에는 소프트 키보드의 하단 모서리에 있는 사용자 작업 단추와의 상호 작용을 설정 하는 작업이 포함 됩니다 `Entry` . [`Entry.ImeOptions`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty)연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `Entry.SetImeOptions` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. AndroidSpecific. SetImeOptions ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . 입력}, Xamarin.Forms . ImeFlags) 메서드는 네임 스페이스에서 다음 값을 제공 하는 열거형을 사용 하 여에 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 대 한 소프트 키보드의 입력 메서드 작업 옵션을 설정 하는 데 사용 됩니다. [`Entry`](xref:Xamarin.Forms.Entry) [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default)– 특정 작업 키가 필요 하지 않으며, 가능한 경우 기본 컨트롤에서 자체를 생성 한다는 것을 나타냅니다. 이는 또는 중 `Next` 하나 `Done` 입니다.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None)– 사용할 수 있는 작업 키가 없음을 나타냅니다.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go)– 작업 키가 사용자를 입력 한 텍스트의 대상으로 가져오는 "go" 작업을 수행 함을 나타냅니다.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search)– 작업 키가 사용자가 입력 한 텍스트를 검색 한 결과를 가져오는 "검색" 작업을 수행 함을 나타냅니다.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send)– 작업 키가 해당 대상에 텍스트를 배달 하는 "보내기" 작업을 수행 함을 나타냅니다.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next)– 작업 키가 "다음" 작업을 수행 하 여 텍스트를 허용 하는 다음 필드를 사용자에 게 표시 함을 나타냅니다.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done)– 작업 키가 소프트 키보드를 닫아 "완료" 작업을 수행 함을 나타냅니다.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous)– 작업 키가 "이전" 작업을 수행 하 여 텍스트를 허용 하는 이전 필드를 사용자에 게 표시 함을 나타냅니다.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction)– 동작 옵션을 선택 하는 마스크입니다.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning)– 맞춤법 검사기 사용자 로부터 정보를 제공 하지 않으며 사용자가 이전에 입력 한 내용에 따라 수정 사항을 제안 하지 않음을 나타냅니다.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen)– UI가 전체 화면으로 이동 하지 않아야 함을 나타냅니다.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi)– 추출 된 텍스트에 대 한 UI가 표시 되지 않음을 나타냅니다.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction)– 사용자 지정 동작에 대 한 UI가 표시 되지 않음을 나타냅니다.

그러면 지정 된 [`ImeFlags`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 값이의 소프트 키보드에 적용 되어 [`Entry`](xref:Xamarin.Forms.Entry) 입력 메서드 편집기 옵션이 설정 됩니다.

[![입력 방법 편집기 플랫폼별](entry-ime-options-images/entry-imeoptions.png "입력 방법 편집기 플랫폼별")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "입력 방법 편집기 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
