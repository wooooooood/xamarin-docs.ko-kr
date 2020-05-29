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
ms.openlocfilehash: f5f0bcdc2d2c8eb1b51ad8dcd1014c649af80c90
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137763"
---
# <a name="inputview-reading-order-on-windows"></a>Windows에서 InputView 읽기 순서

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼별로 [`Entry`](xref:Xamarin.Forms.Entry) , 및 인스턴스에서 양방향 텍스트의 읽기 순서 (왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽으로)를 동적으로 검색할 수 있습니다 [`Editor`](xref:Xamarin.Forms.Editor) [`Label`](xref:Xamarin.Forms.Label) . [`InputView.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty)( `Entry` 및 `Editor` 인스턴스) 또는 [`Label.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) 연결 된 속성을 `boolean` 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. [ `InputView.SetDetectReadingOrderFromContent` ] (F: Xamarin.Forms 입니다. PlatformConfiguration. SetDetectReadingOrderFromContent ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . InputView}, system.string) 메서드는 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스에서 읽기 순서가의 콘텐츠에서 검색 되는지 여부를 제어 하는 데 사용 됩니다 [`InputView`](xref:Xamarin.Forms.InputView) . 또한 `InputView.SetDetectReadingOrderFromContent` 메서드를 사용 하 여 [ `InputView.GetDetectReadingOrderFromContent` ] (f:)를 호출 하 여 콘텐츠에서 읽기 순서가 검색 되었는지 여부를 전환할 수 있습니다 Xamarin.Forms . PlatformConfiguration. GetDetectReadingOrderFromContent ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . InputView})) 메서드를 반환 하 여 현재 값을 반환 합니다.

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

그 결과 [`Entry`](xref:Xamarin.Forms.Entry) , [`Editor`](xref:Xamarin.Forms.Editor) 및 [`Label`](xref:Xamarin.Forms.Label) 인스턴스가 동적으로 검색 된 콘텐츠의 읽기 순서를 가질 수 있습니다.

[![콘텐츠 플랫폼별에서 읽기 순서를 검색 하는 InputView](inputview-reading-order-images/editor-readingorder.png "콘텐츠 플랫폼별에서 읽기 순서를 검색 하는 InputView")](inputview-reading-order-images/editor-readingorder-large.png#lightbox "콘텐츠 플랫폼별에서 읽기 순서를 검색 하는 InputView")

> [!NOTE]
> 속성 설정과 달리 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 텍스트 콘텐츠에서 읽기 순서를 검색 하는 뷰의 논리는 뷰 내의 텍스트 맞춤에 영향을 주지 않습니다. 대신 양방향 텍스트의 블록이 배치 되는 순서를 조정 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
