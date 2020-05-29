---
title: 활동 표시기Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a83885175a44f2174db343abf4591f8777041d39
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136515"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.FormsActivityIndicator
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin.Forms [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) 컨트롤은 응용 프로그램이 시간이 오래 걸리는 작업을 표시 하는 애니메이션을 표시 합니다. 와 달리 [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 는 진행률을 표시 `ActivityIndicator` 하지 않습니다. 는 `ActivityIndicator` 에서 상속 [`View`](xref:Xamarin.Forms.View) 됩니다.

다음 스크린샷에서는 `ActivityIndicator` iOS 및 Android에 대 한 컨트롤을 보여 줍니다.

![IOS 및 Android의 ActivityIndicator 스크린샷](activityindicator-images/activityindicators-default.png "IOS 및 Android의 ActivityIndicator 스크린샷")

`ActivityIndicator`컨트롤은 다음 속성을 정의 합니다.

* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)는 `Color` 의 표시 색을 정의 하는 값입니다 `ActivityIndicator` .
* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)을 `bool` `ActivityIndicator` 표시 하 고 애니메이션 효과를 적용할지 또는 숨길지를 나타내는 값입니다. 값이 이면 `false` `ActivityIndicator` 표시 되지 않습니다.

이러한 속성은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉,의 스타일을 지정 `ActivityIndicator` 하 고 데이터 바인딩의 대상이 될 수 있습니다.

## <a name="create-an-activityindicator"></a>ActivityIndicator 만들기

`ActivityIndicator`XAML에서 클래스를 인스턴스화할 수 있습니다. 해당 `IsRunning` 속성은 컨트롤이 표시 되 고 애니메이션이 적용 되는지 여부를 결정 합니다. `IsRunning`속성은 기본적으로로 설정 `false` 됩니다. 다음 예제에서는 선택적 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다 `ActivityIndicator` `IsRunning` .

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator`코드에서를 만들 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator 모양 속성

`Color`속성은 색을 정의 합니다 `ActivityIndicator` . 다음 예제에서는 속성 집합을 사용 하 여 XAML에서를 인스턴스화하는 방법을 보여 줍니다 `ActivityIndicator` `Color` .

```xaml
<ActivityIndicator Color="Orange" />
```

`Color`코드에서를 만들 때 속성을 설정할 수도 있습니다 `ActivityIndicator` .

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

다음 스크린샷에는 `ActivityIndicator` `Color` IOS 및 Android에서 속성이로 설정 된가 나와 `Color.Orange` 있습니다.

![IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷](activityindicator-images/activityindicators-styled.png "IOS 및 Android에서 스타일이 지정 된 ActivityIndicator의 스크린샷")

## <a name="related-links"></a>관련 링크

* [ActivityIndicator 데모](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
