---
title: Android에서 SwipeView 살짝 밀기 전환 모드
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 SwipeView를 열 때 사용 되는 전환을 제어 하는 Android 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 077d4a8a9530bf074fde710dd08c1fbea49668ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490477"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Android에서 SwipeView 살짝 밀기 전환 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 특정은 `SwipeView`를 열 때 사용 되는 전환을 제어 합니다. 바인딩 가능한 속성 `SwipeView.SwipeTransitionMode` `SwipeTransitionMode` 열거형 값으로 설정 하 여 XAML에서 사용 됩니다.

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

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

`SwipeView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스의 `SwipeView.SetSwipeTransitionMode` 메서드는 `SwipeView`를 열 때 사용 되는 전환을 제어 하는 데 사용 됩니다. `SwipeTransitionMode` 열거형은 다음 두 가지 가능한 값을 제공 합니다.

- `Reveal`은 `SwipeView` 콘텐츠가 스와이프 때 살짝 밀기 항목이 표시 되 고 `SwipeView.SwipeTransitionMode` 속성의 기본값입니다.
- `Drag` `SwipeView` 내용이 스와이프 때 살짝 밀기 항목을 뷰로 끌어 오는 것을 나타냅니다.

또한 `SwipeView.GetSwipeTransitionMode` 메서드를 사용 하 여 `SwipeView`에 적용 되는 `SwipeTransitionMode`를 반환할 수 있습니다.

그러면 지정 된 `SwipeTransitionMode` 값이 `SwipeView`에 적용 되어 `SwipeView`를 열 때 사용 되는 전환을 제어 합니다.

[![Android에서 SwipeView SwipeTransitionModes의 스크린샷](swipeview-swipetransitionmode-images/swipetransitionmode.png "Android의 SwipeTransitionModes")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Android의 SwipeTransitionModes")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
