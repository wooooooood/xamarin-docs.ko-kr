---
title: Windows에 있던 InputView 읽는 순서
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 양방향 텍스트의 읽기 순서 동적으로 검색할 수 있도록 Windows 플랫폼별을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E61BAEE0-C8B7-4F33-8DDC-FA1B9CA8E81D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d5d2e963a326b5bc750527a49008f2d2a40cac9f
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924874"
---
# <a name="inputview-reading-order-on-windows"></a>Windows에 있던 InputView 읽는 순서

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

이 유니버설 Windows 플랫폼 플랫폼별 양방향 텍스트의 읽기 순서 (왼쪽에서 오른쪽 또는 왼쪽으로) 사용 하도록 설정 [ `Entry` ](xref:Xamarin.Forms.Entry)하십시오 [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스가 동적으로 검색할 수 있습니다. 설정 하 여 XAML에서 사용 되는 [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (에 대 한 `Entry` 하 고 `Editor` 인스턴스) 또는 [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` 메서드가 플랫폼별 유니버설 Windows 플랫폼에만 실행 되도록 지정 합니다. 합니다 [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) 메서드는 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스는 사용 제어 되는지 여부를 읽는 순서 내용에서 검색 되는 [ `InputView` ](xref:Xamarin.Forms.InputView). 또한 합니다 `InputView.SetDetectReadingOrderFromContent` 메서드를 호출 하 여 읽는 순서 내용에서 검색 되는 여부를 전환 하려면 사용할 수는 [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) 현재 값을 반환 하는 방법:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

결과 [ `Entry` ](xref:Xamarin.Forms.Entry)를 [ `Editor` ](xref:Xamarin.Forms.Editor), 및 [ `Label` ](xref:Xamarin.Forms.Label) 인스턴스가 동적으로 검색 하는 콘텐츠의 읽기 순서를 가질 수 있습니다.

[![플랫폼별 콘텐츠에서 읽기 순서를 검색 하는 있던 InputView](inputview-reading-order-images/editor-readingorder.png "있던 InputView 플랫폼별 콘텐츠에서 읽기 순서를 검색")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "있던 InputView에서 읽는 순서를 검색 합니다. 플랫폼별 콘텐츠")

> [!NOTE]
> 설정할 때와 달리 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성, 텍스트 내용에서 읽는 순서 뷰 내에 있는 텍스트의 맞춤을 영향을 주지 것입니다를 검색 하는 보기에 대 한 논리입니다. 대신, 양방향 텍스트 블록을 배치 되는 순서를 조정 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
