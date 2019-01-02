---
title: Xamarin.Forms 바인딩 대체
description: 이 문서에서는 바인딩에 실패하는 경우 사용할 대체 값을 정의하여 바인딩을 보다 강력하게 만드는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 505b5bfb9681e5bc30ff84aa90c8e148ed6db4b1
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53058287"
---
# <a name="xamarinforms-binding-fallbacks"></a>Xamarin.Forms 바인딩 대체

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

바인딩 소스를 확인할 수 없거나 바인딩에 성공하지만 `null` 값을 반환하기 때문에 경우에 따라 데이터 바인딩이 실패합니다. 값 변환기 또는 기타 추가 코드를 사용하여 이러한 시나리오를 처리할 수 있고 데이터 바인딩은 바인딩 프로세스에 실패하는 경우 사용할 대체 값을 정의하여 더 강력하게 만들 수 있습니다. 바인딩 식에서 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 및 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 정의하여 이 작업을 수행할 수 있습니다. 이러한 속성이 [`BindingBase`](xref:Xamarin.Forms.BindingBase) 클래스에 위치하기 때문에 바인딩, 컴파일된 바인딩 및 `Binding` 태그 확장과 함께 사용할 수 있습니다.

> [!NOTE]
> 바인딩 식에서 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 및 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 사용하는 것은 선택 사항입니다.

## <a name="defining-a-fallback-value"></a>대체 값 정의

[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 사용하면 바인딩 *소스*를 확인할 수 없을 때 사용되는 대체 값을 정의할 수 있습니다. 이 속성을 설정하는 일반적인 시나리오는 유형이 다른 바인딩된 컬렉션의 모든 개체에 존재하지 않을 수 있는 원본 속성에 바인딩된 경우입니다.

**MonkeyDetail** 페이지는 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 설정하는 방법을 보여줍니다.

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

[`Label`](xref:Xamarin.Forms.Label)에 대한 바인딩은 바인딩 소스를 확인할 수 없는 경우 대상에서 설정할 수 있는 [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 값을 정의합니다. 따라서 `FallbackValue` 속성에 정의된 값은 `Population` 속성이 바인딩된 개체에 존재하지 않는 경우 표시됩니다. 여기에서 `FallbackValue` 속성 값은 작은따옴표(아포스트로피) 문자로 구분됩니다.

[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성 값을 인라인으로 정의하는 대신 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 리소스로 정의하는 것이 좋습니다. 이 방식은 이러한 값을 단일 위치에서 한 번 정의하고 쉽게 지역화할 수 있다는 장점이 있습니다. 그런 다음, `StaticResource` 태그 확장을 사용하여 리소스를 검색할 수 있습니다.

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> 바인딩 식을 사용하여 `FallbackValue` 속성을 설정할 수 없습니다.

실행 중인 프로그램은 다음과 같습니다.

![FallbackValue 바인딩](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue 바인딩")

`FallbackValue` 속성이 바인딩 식에서 설정되어 있지 않고 바인딩 경로 또는 경로의 일부가 확인되지 않은 경우 대상에서 [`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue)를 설정합니다. 그러나 `FallbackValue` 속성이 설정되고 바인딩 경로 또는 경로의 일부가 확인되지 않은 경우 대상에서 `FallbackValue` 값 속성을 설정합니다. 따라서 바인딩된 개체에 `Population` 속성이 없기 때문에 **MonkeyDetail** 페이지에서 [`Label`](xref:Xamarin.Forms.Label)은 "알 수 없는 모집단 크기"라고 표시합니다.

> [!IMPORTANT]
> [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 설정한 경우 바인딩 식에서 정의된 값 변환기가 실행되지 않습니다.

## <a name="defining-a-null-replacement-value"></a>Null 대체 값 정의

[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 사용하면 바인딩 *소스*를 확인할 수 없지만 값이 `null`일 때 사용되는 대체 값을 정의할 수 있습니다. 이 속성을 설정하는 일반적인 시나리오는 바인딩된 컬렉션에서 `null`일 수 있는 원본 속성에 바인딩된 경우입니다.

**Monkeys** 페이지는 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 설정하는 방법을 보여줍니다.

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

[`Image`](xref:Xamarin.Forms.Image) 및 [`Label`](xref:Xamarin.Forms.Label)에 대한 바인딩은 모두 바인딩 경로가 `null`을 반환하는 경우 적용되는 [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 값을 정의합니다. 따라서 `ImageUrl` 및 `Location` 속성을 정의하지 않은 경우 컬렉션의 모든 개체에 `TargetNullValue` 속성에 정의된 값이 표시됩니다. 여기에서 `TargetNullValue` 속성 값은 작은따옴표(아포스트로피) 문자로 구분됩니다.

[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성 값을 인라인으로 정의하는 대신 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)에서 리소스로 정의하는 것이 좋습니다. 이 방식은 이러한 값을 단일 위치에서 한 번 정의하고 쉽게 지역화할 수 있다는 장점이 있습니다. 그런 다음, `StaticResource` 태그 확장을 사용하여 리소스를 검색할 수 있습니다.

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> 바인딩 식을 사용하여 `TargetNullValue` 속성을 설정할 수 없습니다.

실행 중인 프로그램은 다음과 같습니다.

[![TargetNullValue 바인딩](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue 바인딩")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue 바인딩")

바인딩 식에서 `TargetNullValue` 속성을 설정하지 않으면 `null`라는 원본 값은 값 변환기가 정의된 경우 변환되고, `StringFormat`이 정의된 경우 서식이 지정됩니다. 그런 다음, 대상에 대한 결과가 설정됩니다. 그러나 `TargetNullValue` 속성이 설정되어 있는 경우 `null`의 원본 값은 값 변환기가 정의된 경우 변환되고, 변환 후에 여전히 `null`인 경우 대상에 대한 `TargetNullValue` 속성 값이 설정됩니다.

> [!IMPORTANT]
> `TargetNullValue` 속성을 설정한 경우 바인딩 식에서 문자열 서식 지정이 적용되지 않습니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
