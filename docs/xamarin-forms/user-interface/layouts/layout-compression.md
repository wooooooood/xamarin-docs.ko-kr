---
title: "레이아웃 압축"
description: "레이아웃 압축 페이지 렌더링 성능을 향상 시키기 위해 지정 된 레이아웃 시각적 트리에서 제거 합니다. 이 문서는 가져올 수 이점과 레이아웃 압축을 사용 하도록 설정 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: e03acbbac737bffd21ee3b592ab017d227f822ad
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="layout-compression"></a>레이아웃 압축

_레이아웃 압축 페이지 렌더링 성능을 향상 시키기 위해 지정 된 레이아웃 시각적 트리에서 제거 합니다. 이 문서는 가져올 수 이점과 레이아웃 압축을 사용 하도록 설정 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 두 일련의 재귀 메서드 호출을 사용 하 여 레이아웃을 수행 합니다.

- 레이아웃 페이지를 사용 하 여 시각적 트리 맨 위에 있는 차고 페이지에서 모든 시각적 요소를 포함 하도록 시각적 트리의 모든 분기를 진행 합니다. 다른 요소에 대 한 부모 요소는 자체를 기준으로 해당 자식 항목을 배치 및 크기 조정 하는 일을 담당 합니다.
- 무효화는 페이지의 요소에 변경 되는 기준인 새 레이아웃 주기가 트리거됩니다 프로세스입니다. 요소는 올바른 크기 또는 위치 더 이상 없을 때에 잘못 된 간주 됩니다. 자식이 있는 시각적 트리의 모든 요소는 크기 중 하나가 변경 될 때마다 경고가 표시 됩니다. 따라서 시각적 트리에 있는 요소의 크기가 변경 트리를 ripple 변경 내용이 발생할 수 있습니다.

Xamarin.Forms 레이아웃을 수행 하는 방법에 대 한 자세한 내용은 참조 [사용자 지정 레이아웃을 만드는](~/xamarin-forms/user-interface/layouts/custom.md)합니다.

레이아웃 프로세스의 결과는 네이티브 컨트롤의 계층 구조입니다. 그러나이 계층 플랫폼 렌더러의 경우 중첩 된 계층 보기를 추가로 않아서 추가 컨테이너 렌더러 및 래퍼를 포함 합니다. 중첩 수준이 높을수록, Xamarin.Forms 페이지를 표시 하기 위해 수행 하는 작업 양을 커집니다. 복잡 한 레이아웃 계층 구조 보기 심층적이 고 광범위 한 여러 수준의 중첩 될 수 있습니다.

예를 들어 Facebook에 로그인 하기 위한 샘플 응용 프로그램에서 다음 단추를 것이 좋습니다.

![](layout-compression-images/facebook-button.png "Facebook 단추")

이 단추는 다음 XAML 뷰 계층 구조와 함께 사용자 지정 컨트롤로 지정 됩니다.

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

결과 중첩된 보기 계층 구조를 사용 하 여 검토 수 [Xamarin 검사기](~/tools/inspector/index.md)합니다. Android에서는 중첩된 보기 계층에는 17 뷰를 들어 있습니다.

![](layout-compression-images/no-compression.png "Facebook 단추에 대 한 계층 구조 보기")

지정 된 레이아웃 페이지 렌더링 성능을 개선할 수 있는 시각적 트리에서 제거 하 여 중첩 보기를 평면화 하는 방식, iOS 및 Android 플랫폼에서 Xamarin.Forms 응용 프로그램에 사용할 수 있는 레이아웃 압축 합니다. 성능상의 이점이 제공 되는 페이지, 사용 중인 운영 체제의 버전 및 응용 프로그램이 실행 되는 장치의 복잡성에 따라 달라 집니다. 하지만 가장 큰 성능 이점은 이전 버전의 장치에서 볼 수 있습니다.

> [!NOTE]
> 이 문서에서는 Android에서 레이아웃 압축을 적용 한 결과, 하는 동안 iOS에 동일 하 게 적용 됩니다.

## <a name="layout-compression"></a>레이아웃 압축

XAML에서는 레이아웃 압축을 설정 하 여 사용할 수 있습니다는 `CompressedLayout.IsHeadless` 연결 된 속성을 `true` 레이아웃 클래스에:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

또는 사용할 수 있습니다 C#에서 레이아웃 인스턴스는 첫 번째 인수를 지정 하 여는 `CompressedLayout.SetIsHeadless` 메서드:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> 레이아웃 압축 레이아웃 시각적 트리에서 제거 하는 이후 레이아웃, 모양 또는 터치 입력을 받기는 적합 하지 않습니다. 따라서 레이아웃 하는 설정 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 속성 (예: [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) 또는 제스처를 허용 하는, 레이아웃에 대 한 후보가 아닙니다 압축입니다. 그러나 시각적 모양 속성을 설정 하는 또는 제스처를 수락 하는 레이아웃의 레이아웃 압축 되지 않습니다 빌드 또는 런타임 오류가 발생 합니다. 대신 레이아웃 압축을 적용할을 시각적으로 유사한 속성과 제스처 인식 실패 합니다.

Facebook 단추에 대 한 레이아웃 압축은 세 가지 레이아웃 클래스에 사용할 수 있습니다.

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

Android에서는 14 뷰의 중첩된 뷰 계층 구조에서 결과이 됩니다.

![](layout-compression-images/layout-compression.png "레이아웃 압축으로 Facebook 단추에 대 한 계층 구조 보기")

17 뷰의 원래의 중첩된 보기 계층에 비해, 17% 보기 수는 감소를 나타냅니다. 이 감소는 불필요 한 나타날 전체 페이지에 대해 보기 감소 보다 중요 한 될 수 있습니다.

### <a name="fast-renderers"></a>빠른 렌더러

결과 네이티브 뷰 계층 구조를 병합 하 여 확장 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 절감 하는 빠른 렌더러 합니다. 이 성능이 더욱 향상 됩니다 되 고 그 덜 복잡 한 시각적 트리의 및 메모리 사용이 적은 결과 적은 개체를 만들어 합니다. 빠른 렌더러에 대 한 자세한 내용은 참조 [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)합니다.

샘플 응용 프로그램에서 Facebook 단추에 대해 레이아웃 압축 및 빠른 렌더러를 결합 8 뷰의 계층 구조는 중첩된 보기를 생성 합니다.

![](layout-compression-images/layout-compression-with-fast-renderers.png "레이아웃 압축 및 빠른 렌더러와 Facebook 단추에 대 한 계층 구조 보기")

17 뷰의 원래의 중첩된 보기 계층에 비해, 52% 절감 하는 역할을 나타냅니다.

샘플 응용 프로그램에는 실제 응용 프로그램에서 추출 된 페이지가 포함 되어 있습니다. 페이지는 레이아웃 압축 및 빠른 렌더러 없이 Android에서 130 뷰의 중첩된 보기 계층 구조를 생성합니다. 빠른 렌더러 및 레이아웃 압축에 적합 한 레이아웃 클래스를 사용 하도록 설정 46%의 감소 70 뷰로 중첩된 보기 계층을 감소 합니다.

## <a name="summary"></a>요약

레이아웃 압축 페이지 렌더링 성능을 향상 시키기 위해 지정 된 레이아웃 시각적 트리에서 제거 합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 응용 프로그램이 실행 중인 장치에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 장치에서 볼 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
- [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
