---
title: Xamarin.ios의 작업 표시기
description: ActivityIndicator 컨트롤은 진행 상황을 표시 하지 않고 응용 프로그램이 긴 활동에서 사용 중임을 사용자에 게 나타냅니다. 이 문서에서는 XAML 및 코드에서 ActivityIndicator를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: e13a46e1022f4e33ace6f9f19bb5cea5d1ac784b
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739158"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.ios ActivityIndicator
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin.ios는 응용 프로그램이[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 긴 작업에 참여 하 고 있음을 보여 주는 애니메이션을 표시 하는 컨트롤입니다. 와 달리는 `ActivityIndicator` 진행률을 표시 하지 않습니다. [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 는 `ActivityIndicator` [에서`View`](xref:Xamarin.Forms.View)상속 됩니다.

다음 스크린샷에서는 iOS 및 `ActivityIndicator` Android의 컨트롤을 보여 줍니다.

![IOS 및 Android의 ActivityIndicator 스크린샷](activityindicator-images/activityindicators-default.png "IOS 및 Android의 ActivityIndicator 스크린샷")

컨트롤 `ActivityIndicator` 은 다음 속성을 정의 합니다.

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)을 표시 하 고 애니메이션 효과를 `ActivityIndicator` 적용할지 또는 숨길지를 나타내는 값입니다.`bool` 값이 이면가 `false` `ActivityIndicator` 표시 되지 않습니다.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)는의 표시 색을 정의 하는 `Color` 값입니다 `ActivityIndicator`.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, `ActivityIndicator` 의 스타일을 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다.

## <a name="create-an-activityindicator"></a>ActivityIndicator 만들기

는 `ActivityIndicator` XAML에서 인스턴스화될 수 있습니다. 컨트롤 `IsRunning` 의 표시 여부를 결정 하기 위해 해당 속성을 설정할 수 있습니다. 속성이 설정 되지 않은 경우 기본값은로 `false` 설정 되 고 `ActivityIndicator` 는 표시 되지 않습니다. `IsRunning` 다음 예제에서는 선택적 `ActivityIndicator` `IsRunning` 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ActivityIndicator IsRunning="true" />
```

코드 `ActivityIndicator` 에서를 만들 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator 모양 속성

속성을 설정 하 여 `ActivityIndicator` 색을 정의할 수 있습니다. `Color` 다음 예제에서는 `Color` 속성 집합을 사용 하 `ActivityIndicator` 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ActivityIndicator Color="Orange" />
```

코드 `Color` `ActivityIndicator` 에서를 만들 때 속성을 설정할 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

다음 스크린샷에서는 iOS 및 `ActivityIndicator` Android에서 `Color` 속성이로 `Color.Orange` 설정 된을 보여 줍니다.

![IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷](activityindicator-images/activityindicators-styled.png "IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷")

## <a name="related-links"></a>관련 링크

* [ActivityIndicator 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
