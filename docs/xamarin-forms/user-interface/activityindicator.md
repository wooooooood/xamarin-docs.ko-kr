---
title: Xamarin.ios의 작업 표시기
description: ActivityIndicator 컨트롤은 진행 상황을 표시 하지 않고 응용 프로그램이 긴 활동에서 사용 중임을 사용자에 게 나타냅니다. 이 문서에서는 XAML 및 코드에서 ActivityIndicator를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: 0694439f5e363399e0442c9883426c0f0bf5d989
ms.sourcegitcommit: ab51d32f4ea0e0d4701f0bf2f1465c9323cd070b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70887435"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.ios ActivityIndicator
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin.ios [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 컨트롤은 응용 프로그램이 시간이 오래 걸리는 작업을 표시 하는 애니메이션을 표시 합니다. 와 달리는 `ActivityIndicator` 진행률을 표시 하지 않습니다. [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 는 `ActivityIndicator` [에서`View`](xref:Xamarin.Forms.View)상속 됩니다.

다음 스크린샷에서는 iOS 및 `ActivityIndicator` Android에 대 한 컨트롤을 보여 줍니다.

![IOS 및 Android의 ActivityIndicator 스크린샷](activityindicator-images/activityindicators-default.png "IOS 및 Android의 ActivityIndicator 스크린샷")

컨트롤 `ActivityIndicator` 은 다음 속성을 정의 합니다.

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)는의 표시 색을 정의 하는 `Color` 값입니다 `ActivityIndicator`.
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)을 표시 하 고 애니메이션 효과를 `ActivityIndicator` 적용할지 또는 숨길지를 나타내는 값입니다.`bool` 값이 이면 `false` `ActivityIndicator` 표시 되지 않습니다.

이러한 속성은 개체에 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 의해 지원 됩니다. 즉, `ActivityIndicator` 의 스타일을 지정 하 고 데이터 바인딩의 대상이 될 수 있습니다.

## <a name="create-an-activityindicator"></a>ActivityIndicator 만들기

XAML `ActivityIndicator` 에서 클래스를 인스턴스화할 수 있습니다. 해당 `IsRunning` 속성은 컨트롤이 표시 되 고 애니메이션이 적용 되는지 여부를 결정 합니다. 합니다 `IsRunning` 속성의 기본값은 `false`합니다. 다음 예제에서는 선택적 `ActivityIndicator` `IsRunning` 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ActivityIndicator IsRunning="true" />
```

코드 `ActivityIndicator` 에서를 만들 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator 모양 속성

속성 `Color` 은 색을 `ActivityIndicator` 정의 합니다. 다음 예제에서는 `Color` 속성 집합을 사용 하 `ActivityIndicator` 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다.

```xaml
<ActivityIndicator Color="Orange" />
```

코드 `Color` `ActivityIndicator` 에서를 만들 때 속성을 설정할 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

다음 스크린샷에는 iOS 및 `ActivityIndicator` Android에서 `Color` 속성이로 `Color.Orange` 설정 된가 나와 있습니다.

![IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷](activityindicator-images/activityindicators-styled.png "IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷")

## <a name="related-links"></a>관련 링크

* [ActivityIndicator 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
