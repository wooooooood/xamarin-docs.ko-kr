---
title: Android에서 SwipeView 살짝 밀기 전환 모드
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 SwipeView를 열 때 사용 되는 전환을 제어 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c420fe65b020067169230dd06dbcd5ce65c036ab
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128624"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Android에서 SwipeView 살짝 밀기 전환 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 특정은를 열 때 사용 되는 전환을 제어 `SwipeView` 합니다. `SwipeView.SwipeTransitionMode`바인딩 가능한 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `SwipeTransitionMode` .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `SwipeView.SetSwipeTransitionMode`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 를 열 때 사용 되는 전환을 제어 하는 데 사용 됩니다 `SwipeView` . `SwipeTransitionMode`열거형은 다음 두 가지 가능한 값을 제공 합니다.

- `Reveal`내용이 스와이프 때 살짝 밀기 항목이 표시 되 `SwipeView` 고 속성의 기본값입니다 `SwipeView.SwipeTransitionMode` .
- `Drag`내용이 스와이프 때 살짝 밀기 항목을 뷰로 끌 것임을 나타냅니다 `SwipeView` .

또한 메서드를 사용 하 여에 `SwipeView.GetSwipeTransitionMode` 적용 되는를 반환할 수 있습니다 `SwipeTransitionMode` `SwipeView` .

그러면 지정 된 `SwipeTransitionMode` 값이에 적용 되어를 `SwipeView` 열 때 사용 되는 전환을 제어 합니다 `SwipeView` .

[![Android에서 SwipeView SwipeTransitionModes의 스크린샷](swipeview-swipetransitionmode-images/swipetransitionmode.png "Android의 SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Android의 SwipeTransitionModes")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
