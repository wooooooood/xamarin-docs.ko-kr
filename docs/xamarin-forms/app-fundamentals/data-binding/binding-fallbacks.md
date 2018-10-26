---
title: Xamarin.Forms 바인딩 대체
description: 이 문서에서는 바인딩 바인딩 실패 하는 경우 사용할 대체 값을 정의 하 여 보다 강력 하 게 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: fa375720730630065609e328b343e16578c6f1df
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131993"
---
# <a name="xamarinforms-binding-fallbacks"></a>Xamarin.Forms 바인딩 대체

경우에 따라 데이터 바인딩이 실패할 바인딩 소스를 확인할 수 없기 때문에 바인딩에 성공 하지만 반환을 `null` 값입니다. 값 변환기 또는 기타 추가 코드를 사용 하 여 이러한 시나리오를 처리할 수 있지만 데이터 바인딩은 만들 수 있습니다 더 강력한 바인딩 프로세스가 실패 하는 경우 사용할 대체 값을 정의 하 여. 정의 하 여이 작업을 수행할 수 있습니다 합니다 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 하 고 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 바인딩 식에서 속성입니다. 이러한 속성에 상주 하기 때문에 합니다 [ `BindingBase` ](xref:Xamarin.Forms.BindingBase) 클래스 및 컴파일된 바인딩과 바인딩을 사용 하 여 사용할 수는 `Binding` 태그 확장 합니다.

> [!NOTE]
> 사용 된 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 하 고 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 바인딩 식에서 속성은 선택 사항입니다.

## <a name="defining-a-fallback-value"></a>대체 (fallback) 값을 정의합니다.

합니다 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 사용 하면 사용 되는 대체 (fallback) 값을 정의할 수 때 바인딩을 *소스* 확인할 수 없습니다. 바인딩 소스 속성에 유형이 다른 바인딩된 컬렉션의 모든 개체에 존재 하지 않을 경우이 속성을 설정 하는 것에 대 한 일반적인 시나리오입니다.

합니다 **MonkeyDetail** 설정을 보여 줍니다 합니다 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성:

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

바인딩에서 합니다 [ `Label` ](xref:Xamarin.Forms.Label) 정의 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 바인딩 소스를 확인할 수 없는 경우 대상에서 설정할 수 있는 값입니다. 에 정의 된 값에 따라서 합니다 `FallbackValue` 속성을 표시 하는 경우는 `Population` 속성이 바인딩된 개체에서 존재 하지 않습니다. 사용자에 게 여기는 `FallbackValue` 속성 값은 작은따옴표 (아포스트로피) 문자로 구분 됩니다.

정의 하는 대신 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성 값을 인라인으로 것이 좋습니다에 리소스로 정의 하는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 이 방식의 장점은 이러한 값을 단일 위치에서 한 번 정의 된을 쉽게 지역화할입니다. 사용 하 여 리소스를 검색할 수 있습니다는 `StaticResource` 태그 확장:

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> 설정 하는 것이 불가능 합니다 `FallbackValue` 바인딩 식 사용 하 여 속성입니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램이입니다.

![FallbackValue 바인딩을](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue 바인딩")

경우는 `FallbackValue` 속성이 설정 되어 있지 않은 바인딩 식 및 바인딩 경로 또는 경로의 일부를 해결 하지 않습니다 [ `BindableProperty.DefaultValue` ](xref:Xamarin.Forms.BindableProperty.DefaultValue) 대상에서 설정 됩니다. 그러나 경우는 `FallbackValue` 속성이 설정 되어 바인딩 경로 또는 경로의 일부를 해결 하지 않습니다 값은 `FallbackValue` 대상의 value 속성을 설정 합니다. 따라서 합니다 **MonkeyDetail** 페이지의 [ `Label` ](xref:Xamarin.Forms.Label) 바인딩된 개체에 없으므로 "알 수 없는 모집단 크기"를 표시를 `Population` 속성입니다.

> [!IMPORTANT]
> 바인딩 식에서 정의 된 값 변환기가 실행 되지 않습니다 경우는 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue) 속성을 설정 합니다.

## <a name="defining-a-null-replacement-value"></a>Null 대체 값을 정의합니다.

합니다 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성을 사용 하면 사용 되는 정의 대체 값 때 바인딩을 *소스* 해결 되 값 이지만 `null`합니다. 이 속성을 설정 하는 것에 대 한 일반적인 시나리오는 될 수 있는 원본 속성에 바인딩할 때 `null` 바인딩된 컬렉션에 있습니다.

합니다 **원숭이** 설정을 보여 줍니다 합니다 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성:

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

에 대 한 바인딩 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 하 고 [ `Label` ](xref:Xamarin.Forms.Label) 정의 둘 다 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 바인딩 경로 반환하는경우적용되는값`null`. 따라서에 정의 된 값을 `TargetNullValue` 컬렉션의 모든 개체에 대 한 속성이 표시 됩니다 여기서는 `ImageUrl` 및 `Location` 속성에 정의 되어 있지 않은. 사용자에 게 여기는 `TargetNullValue` 속성 값은 작은따옴표 (아포스트로피) 문자로 구분 됩니다.

정의 하는 대신 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue) 속성 값을 인라인으로 것이 좋습니다에 리소스로 정의 하는 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)합니다. 이 방식의 장점은 이러한 값을 단일 위치에서 한 번 정의 된을 쉽게 지역화할입니다. 사용 하 여 리소스를 검색할 수 있습니다는 `StaticResource` 태그 확장:

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> 설정 하는 것이 불가능 합니다 `TargetNullValue` 바인딩 식 사용 하 여 속성입니다.

다음은 세 플랫폼 모두에서 실행 중인 프로그램이입니다.

[![TargetNullValue 바인딩을](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue 바인딩")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue 바인딩")

경우는 `TargetNullValue` source 값이 바인딩 식에서 속성이 설정 되지 않습니다 `null` 경우 형식이 지정 된 값 변환기가 정의 되어 있으면 변환 됩니다는 `StringFormat` 정의 된 대상에서 결과 설정한 후 합니다. 그러나 경우는 `TargetNullValue` 속성이 설정 되어 원본 값 `null` 값 변환기를 정의 하는 경우 및이 여전히 변환 됩니다 `null` 값 변환 후를 `TargetNullValue` 대상 속성이 설정.

> [!IMPORTANT]
> 바인딩 식에서 문자열 서식 지정을 적용 되지 않습니다 경우는 `TargetNullValue` 속성을 설정 합니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
