---
title: Android에서 항목 입력 방법 편집기 옵션
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼 특정 입력된 메서드 항목에 대 한 소프트 키보드에 대 한 편집기 옵션을 설정 하는 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7909C738-04B2-4476-9A3B-A6D79BC3B9B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4da446cf342065ce7766f8df0c71008ab47c31c4
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926825"
---
# <a name="entry-input-method-editor-options-on-android"></a>Android에서 항목 입력 방법 편집기 옵션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 Android 플랫폼 특정 입력된 메서드 (입력기) 옵션을 설정 하에 대 한 소프트 키보드를 [ `Entry` ](xref:Xamarin.Forms.Entry)합니다. 소프트 키보드와의 상호 작용의 아래쪽 모서리에서 사용자 작업 단추를 설정 하는 것이 여기는 `Entry`합니다. 설정 하 여 XAML에서 사용 되는 [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) 연결 된 속성의 값에는 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) 메서드를를 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스에 대 한 소프트 키보드 입력된 메서드가 작업 옵션을 설정 하는 합니다 [ `Entry` ](xref:Xamarin.Forms.Entry), 사용 하 여 합니다 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 다음 값을 제공 하는 열거형:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – 나타내며 특정 작업 키는 필요 없습니다 수 내부 컨트롤 자체를 생성 합니다. 중 하나 `Next` 또는 `Done`합니다.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – 없습니다 동작 키를 사용할 수 있도록 나타냅니다.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – 동작 키 "이동" 작업을 수행할지에 입력 텍스트의 대상 사용자를 나타냅니다.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) -는 "검색" 연산을 수행 하는 동작 키, 텍스트 검색의 결과가 사용자 입력을 나타냅니다.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – 동작 키가 대상에 텍스트를 제공 "보내기" 작업을 수행 되는 것을 나타냅니다.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – 동작 키 텍스트를 허용 하는 다음 필드에 사용자를 수행 하는 "다음" 작업을 수행 되는 것을 나타냅니다.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – 동작 키 소프트 키보드 닫기를 "완료" 작업을 수행할지를 나타냅니다.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – 동작 키 텍스트를 허용 하는 이전 필드에 사용자를 이동 "이전" 작업을 수행 되는 것을 나타냅니다.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) -작업 옵션을 선택 하는 마스크입니다.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – 나타냅니다는 맞춤법 검사기는 사용자에 대해 알아봅니다 아니고 기반으로 새로운 사용자가 이전에 입력 한 수정을 제안 합니다.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – UI 화면을 이동 하지 해야 나타냅니다.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) -추출 된 텍스트에 대 한 UI가 표시를 나타냅니다.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – 사용자 지정 작업에 대 한 UI를 표시할지 나타냅니다.

결과 지정 된 [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) 값에 대 한 소프트 키보드에 적용 됩니다는 [ `Entry` ](xref:Xamarin.Forms.Entry)를 설정 하는 입력된 방법 편집기 옵션:

[![항목 입력 방법 편집기 플랫폼별](entry-ime-options-images/entry-imeoptions.png "항목 입력 방법 편집기 플랫폼별")](entry-ime-options-images/entry-imeoptions-large.png#lightbox "항목 입력 방법 편집기 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
