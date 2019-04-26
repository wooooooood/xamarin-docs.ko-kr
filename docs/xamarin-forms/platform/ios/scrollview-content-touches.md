---
title: IOS에서 ScrollView 콘텐츠 터치
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 ScrollView 터치 제스처를 처리 합니다. 해당 콘텐츠를 전달 하는지 여부를 제어 하는 iOS 플랫폼 전용을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 7f6ec13b1b21a1526bb53f260c4d80e881e7feba
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60952242"
---
# <a name="scrollview-content-touches-on-ios"></a>IOS에서 ScrollView 콘텐츠 터치

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

암시적 타이머를 터치 제스처 시작 되 면 트리거됩니다를 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) ios 및 `ScrollView` 타이머 범위 내의 사용자 동작을 기준으로 제스처 처리 또는 해당 콘텐츠를 전달 해야 하는지 여부를 결정 합니다. 기본적으로 iOS `ScrollView` 지연 콘텐츠 터치를 하지만 사용 하 여 일부 상황에서 문제를 일으킬 수이 있습니다는 `ScrollView` 하지 말아야 할 때 제스처를 최우선 콘텐츠입니다. 따라서이 플랫폼별 제어 하는지 여부를 `ScrollView` 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 설정 하 여 XAML에서 사용 되는 `ScrollView.ShouldDelayContentTouches` 연결 된 속성을 `boolean` 값:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `ScrollView.SetShouldDelayContentTouches` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스에는 컨트롤에 있는지 여부를 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 터치 제스처를 처리 하거나 해당 콘텐츠를 전달 합니다. 또한 합니다 `SetShouldDelayContentTouches` 메서드를 호출 하 여 콘텐츠 터치 지연 설정/해제를 사용할 수는 `ShouldDelayContentTouches` 콘텐츠 터치 지연 되 고 있는지 여부를 반환 하는 방법.

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

결과 [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) 지연 되므로 콘텐츠 터치를 수신 사용 하지 않도록 설정할 수 있습니다이 시나리오에는 [ `Slider` ](xref:Xamarin.Forms.Slider) 제스처를 받는 대신 [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) 페이지의 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView 지연 콘텐츠 건드리면 플랫폼별")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "지연을 ScrollView 콘텐츠 건드리면 플랫폼별")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
