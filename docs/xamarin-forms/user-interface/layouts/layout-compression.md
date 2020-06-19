---
title: 레이아웃 압축
description: 레이아웃 압축은 페이지 렌더링 성능을 향상 시키기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이 문서에서는 레이아웃 압축 및 가져올 수 있는 이점을 사용 하도록 설정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 40af5aeaa51025dae70113faa6f7ff83edf43c73
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138028"
---
# <a name="layout-compression"></a>레이아웃 압축

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)

_레이아웃 압축은 페이지 렌더링 성능을 향상 시키기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이 문서에서는 레이아웃 압축 및 가져올 수 있는 이점을 사용 하도록 설정 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms두 시리즈의 재귀 메서드 호출을 사용 하 여 레이아웃을 수행 합니다.

- 레이아웃은 페이지를 사용 하 여 시각적 트리 위쪽에서 시작 하 고 시각적 트리의 모든 분기를 진행 하 여 페이지의 모든 시각적 요소를 포함 합니다. 다른 요소에 대 한 부모인 요소는 자신을 기준으로 자식 항목의 크기를 조정 하 고 위치를 지정 해야 합니다.
- 무효화는 페이지의 요소를 변경 하 여 새 레이아웃 주기를 트리거하는 프로세스입니다. 요소는 더 이상 올바른 크기나 위치가 없는 경우 유효 하지 않은 것으로 간주 됩니다. 자식 트리의 모든 요소는 자식 항목 중 하나가 크기를 변경할 때마다 경고를 표시 합니다. 따라서 시각적 트리의 요소 크기를 변경 하면 트리가 변경 될 수 있습니다.

에서 레이아웃을 수행 하는 방법에 대 한 자세한 내용은 Xamarin.Forms [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)를 참조 하세요.

레이아웃 프로세스의 결과는 네이티브 컨트롤의 계층 구조입니다. 그러나이 계층에는 추가 컨테이너 렌더러 및 플랫폼 렌더러에 대 한 래퍼가 포함 되어 있으며,이를 통해 뷰 계층 구조가 중첩 않아서. 중첩 수준이 심화 되 면 Xamarin.Forms 페이지를 표시 하기 위해 수행 해야 하는 작업의 양이 커집니다. 복합 레이아웃의 경우 뷰 계층 구조는 포괄적이 고 광범위 하 게 중첩 될 수 있습니다.

예를 들어 Facebook에 로그인 하기 위한 샘플 응용 프로그램에서 다음 단추를 살펴보겠습니다.

![](layout-compression-images/facebook-button.png "Facebook Button")

이 단추는 다음 XAML 뷰 계층 구조를 포함 하는 사용자 지정 컨트롤로 지정 됩니다.

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

결과 중첩 된 뷰 계층 구조는 [Xamarin Inspector](~/tools/inspector/index.md)를 사용 하 여 검사할 수 있습니다. Android에서 중첩 된 뷰 계층에는 17 개의 보기가 포함 되어 있습니다.

![](layout-compression-images/no-compression.png "View Hierarchy for Facebook Button")

Xamarin.FormsIOS 및 Android 플랫폼의 응용 프로그램에 사용할 수 있는 레이아웃 압축은 시각적 트리에서 지정 된 레이아웃을 제거 하 여 뷰 중첩을 평면화 하 여 페이지 렌더링 성능을 향상 시킬 수 있습니다. 제공 되는 성능 혜택은 페이지의 복잡성, 사용 중인 운영 체제의 버전 및 응용 프로그램이 실행 되는 장치에 따라 달라 집니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다.

> [!NOTE]
> 이 문서에서는 Android에서 레이아웃 압축을 적용 한 결과에 중점을 둔 반면, iOS에도 동일 하 게 적용 됩니다.

## <a name="layout-compression"></a>레이아웃 압축

XAML에서는 `CompressedLayout.IsHeadless` 레이아웃 클래스에서 연결 된 속성을로 설정 하 여 레이아웃 압축을 사용 하도록 설정할 수 있습니다 `true` .

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

또는 레이아웃 인스턴스를 메서드에 대 한 첫 번째 인수로 지정 하 여 c #에서 사용 하도록 설정할 수 있습니다 `CompressedLayout.SetIsHeadless` .

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> 레이아웃 압축은 시각적 트리에서 레이아웃을 제거 하므로 시각적으로 표시 되는 레이아웃에는 적합 하지 않거나 터치 입력을 가져옵니다. 따라서 속성 (예:,,,, 또는)을 설정 하는 레이아웃 [`VisualElement`](xref:Xamarin.Forms.VisualElement) [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 은 레이아웃 압축에 적합 하지 않습니다. 그러나 시각적 모양 속성을 설정 하거나 제스처를 허용 하는 레이아웃에서 레이아웃 압축을 사용 하도록 설정 하면 빌드 또는 런타임 오류가 발생 하지 않습니다. 대신 레이아웃 압축이 적용 되 고 시각적 모양 속성 및 제스처 인식이 자동으로 실패 합니다.

Facebook 단추의 경우 다음과 같은 세 가지 레이아웃 클래스에서 레이아웃 압축을 사용 하도록 설정할 수 있습니다.

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

Android에서는 다음의 중첩 된 뷰 계층 구조를 생성 합니다.

![](layout-compression-images/layout-compression.png "View Hierarchy for Facebook Button with Layout Compression")

17 개 보기의 원래 중첩 된 뷰 계층 구조와 비교할 때이 값은 17% 보기의 감소를 나타냅니다. 이 감소는 중요 하지 않을 수 있지만 전체 페이지에 대 한 보기 감소는 더 중요할 수 있습니다.

### <a name="fast-renderers"></a>빠른 렌더러

빠른 렌더러는 Xamarin.Forms 결과 네이티브 뷰 계층 구조를 평면화 하 여 Android에서 컨트롤의 인플레이션 및 렌더링 비용을 줄여 줍니다. 이를 통해 더 작은 개체를 만들어 성능을 향상 시킵니다. 그러면 시각적 트리를 덜 사용 하 고 메모리를 덜 사용 하 게 됩니다. 빠른 렌더러에 대 한 자세한 내용은 [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)를 참조 하세요.

예제 응용 프로그램의 Facebook 단추에 대해 레이아웃 압축 및 빠른 렌더러를 결합 하면 다음과 같은 8 개의 뷰로 중첩 된 뷰 계층 구조가 생성 됩니다.

![](layout-compression-images/layout-compression-with-fast-renderers.png "View Hierarchy for Facebook Button with Layout Compression and Fast Renderers")

17 개 보기의 원래 중첩 된 뷰 계층 구조와 비교할 때이는 52%의 감소를 나타냅니다.

샘플 응용 프로그램은 실제 응용 프로그램에서 추출 된 페이지를 포함 합니다. 레이아웃 압축 및 빠른 렌더러를 사용 하지 않으면 페이지는 Android에서 130 보기의 중첩 된 뷰 계층 구조를 생성 합니다. 적절 한 레이아웃 클래스에서 빠른 렌더러 및 레이아웃 압축을 사용 하도록 설정 하면 중첩 된 뷰 계층 구조가 70 뷰로 줄어들고 46%가 감소 합니다.

## <a name="summary"></a>요약

레이아웃 압축은 페이지 렌더링 성능을 향상 시키기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 애플리케이션이 실행 중인 디바이스에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
- [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)
