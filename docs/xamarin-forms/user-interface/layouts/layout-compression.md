---
title: 레이아웃 압축
description: 레이아웃 압축 페이지 렌더링 성능을 향상 하기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이 문서에서는 레이아웃 압축 및 이점을 얻을 수 있게 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 97aa8c321362ebccc954a79f99b7bc69b5a0ad63
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657070"
---
# <a name="layout-compression"></a>레이아웃 압축

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)

_레이아웃 압축 페이지 렌더링 성능을 향상 하기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이 문서에서는 레이아웃 압축 및 이점을 얻을 수 있게 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.Forms 레이아웃 두 일련의 재귀 메서드 호출을 사용 하 여 수행 합니다.

- 레이아웃 페이지를 사용 하 여 시각적 트리의 맨 위에 있는 시작 및 페이지의 모든 시각적 요소를 포함 하도록 시각적 트리의 모든 분기를 진행 합니다. 다른 요소에는 부모 요소는 자체를 기준으로 자식을 배치 및 크기 조정 하는 일을 담당 합니다.
- 무효화는 변경 페이지에서 요소에는 새 레이아웃 주기를 트리거하는 프로세스입니다. 요소는 올바른 크기 또는 위치를 더 이상 없을 때에 잘못 된 간주 됩니다. 자식이 있는 시각적 트리의 모든 요소는 자식 중 하나의 크기를 변경 될 때마다 경고가 표시 됩니다. 따라서 시각적 트리에 있는 요소의 크기 변경에는 트리 ripple 변경 발생할 수 있습니다.

Xamarin.Forms 레이아웃 작업을 수행 하는 방법에 대 한 자세한 내용은 참조 하십시오 [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)합니다.

레이아웃 프로세스의 결과 네이티브 컨트롤 계층 구조입니다. 그러나이 계층 플랫폼 렌더러의 경우 추가 중첩 뷰 계층 구조를 값에 대 한 추가 컨테이너 렌더러 및 래퍼를 포함 합니다. 중첩 수준이 높을수록, Xamarin.Forms 페이지를 표시 하기 위해 수행 해야 하는 작업 양을 높아집니다. 복잡 한 레이아웃 뷰 계층 구조 심층적이 고 광범위 한 여러 수준의 중첩 될 수 있습니다.

예를 들어 Facebook 로그인에 대 한 샘플 응용 프로그램에서 다음 단추를 것이 좋습니다.

![](layout-compression-images/facebook-button.png "Facebook 단추")

이 단추는 다음 XAML 뷰 계층 구조를 사용 하 여 사용자 지정 컨트롤로 지정 됩니다.

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

결과 중첩된 보기 계층 구조를 사용 하 여 검사할 수 있습니다 [Xamarin Inspector](~/tools/inspector/index.md)합니다. Android에서 중첩 된 뷰 계층 구조는 17 보기가 포함 되어 있습니다.

![](layout-compression-images/no-compression.png "Facebook 단추에 대 한 뷰 계층 구조")

IOS 및 Android 플랫폼에서 Xamarin.Forms 응용 프로그램에 사용할 수 있는 레이아웃 압축은 페이지 렌더링 성능을 개선할 수 있는 시각적 트리에서 지정 된 레이아웃을 제거 하 여 중첩 보기를 평면화 하려고 합니다. 전달 되는 성능 이점은 페이지, 사용 중인 운영 체제의 버전 및 응용 프로그램이 실행 되는 장치 복잡성에 따라 달라 집니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다.

> [!NOTE]
> 이 문서에서는 android 레이아웃 압축을 적용 한 결과 하는 동안에 iOS에 동일 하 게 적용 됩니다.

## <a name="layout-compression"></a>레이아웃 압축

XAML, 레이아웃 압축을 설정 하 여 사용할 수 있습니다 합니다 `CompressedLayout.IsHeadless` 연결 속성을 `true` 레이아웃 클래스:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

또는 사용할 수 있습니다 C#에서 첫 번째 인수로 레이아웃 인스턴스를 지정 하 여는 `CompressedLayout.SetIsHeadless` 메서드:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> 레이아웃 압축 레이아웃을 시각적 트리에서 제거 하므로 레이아웃 하는 시각적 모양이 또는 터치 입력을 받기는 적합 하지 않습니다. 따라서 레이아웃을 설정 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 속성 (같은 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)하십시오 [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible), [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation), [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale)합니다 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) 하 고 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 제스처를 허용 하는 레이아웃에 대 한 후보가 아닙니다 또는 압축 합니다. 그러나 시각적 모양 속성을 설정 하는 제스처를 받아들이는 또는 레이아웃의 레이아웃 압축을 사용 하도록 설정 되지 않습니다는 빌드 또는 런타임 오류가 발생 했습니다. 대신, 레이아웃 압축 적용 되 고 시각적으로 유사한 속성과 제스처 인식, 자동으로 실패 합니다.

Facebook 단추에 대 한 세 가지 레이아웃 클래스의 레이아웃 압축을 활성화할 수 있습니다.

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

Android에서이 인해 14 뷰 중첩된 보기 계층 구조에서:

![](layout-compression-images/layout-compression.png "레이아웃 압축을 사용 하 여 Facebook 단추에 대 한 뷰 계층 구조")

17 뷰의 원래 중첩 된 뷰 계층 구조에 비해, 17% 보기 수 감소를 나타냅니다. 이 감소는 불필요 한 나타날, 전체 페이지를 통해 보기 감소 좀 더 중요 한 될 수 있습니다.

### <a name="fast-renderers"></a>빠른 렌더러

빠른 렌더러 결과 네이티브 뷰 계층 구조를 평면화 하 여 인플레이션 및 Android에서 Xamarin.Forms 컨트롤의 렌더링 비용을 줄입니다. 이 추가 되 고 그 덜 복잡 한 시각적 트리 및 메모리 사용량 결과 적은 개체를 만들어 성능을 개선 합니다. 빠른 렌더러에 대 한 자세한 내용은 참조 하세요. [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)합니다.

샘플 응용 프로그램에서 Facebook 단추의 레이아웃 압축 및 빠른 렌더러를 결합 하는 8 뷰의 중첩 된 뷰 계층 구조를 생성 합니다.

![](layout-compression-images/layout-compression-with-fast-renderers.png "레이아웃 압축 및 빠른 렌더러를 사용 하 여 Facebook 단추에 대 한 뷰 계층 구조")

17 뷰의 원래 중첩 된 뷰 계층 구조에 비해, 52% 감소를 나타냅니다.

샘플 응용 프로그램에는 실제 응용 프로그램에서 추출 된 페이지가 포함 되어 있습니다. 레이아웃 압축 및 빠른 렌더러 없이 페이지 Android에서 130 뷰의 중첩 된 뷰 계층 구조를 생성합니다. 빠른 렌더러 및 적절 한 레이아웃 클래스 레이아웃 압축 활성화 70 뷰, 46% 감소를 중첩 된 뷰 계층 구조를 줄입니다.

## <a name="summary"></a>요약

레이아웃 압축 페이지 렌더링 성능을 향상 하기 위해 시각적 트리에서 지정 된 레이아웃을 제거 합니다. 이로 인해 얻을 수 있는 성능 이점은 페이지의 복잡성, 사용 중인 운영 체제의 버전, 그리고 응용 프로그램이 실행 중인 디바이스에 따라 다릅니다. 하지만 가장 큰 성능 이점은 이전 버전의 디바이스에서 볼 수 있습니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 레이아웃 만들기](~/xamarin-forms/user-interface/layouts/custom.md)
- [빠른 렌더러](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layoutcompression)
