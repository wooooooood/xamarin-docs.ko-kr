---
title: Xamarin.Forms에서 활동 표시기
description: 응용 프로그램에에서 참여 하는 시간이 오래 걸리는 작업을 진행의 표시가 전혀 제공 하지 않고도 사용자에 게 ActivityIndicator 컨트롤을 나타냅니다. 이 문서는 ActivityIndicator XAML 및 코드에서 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: abd8150e3aa4ec347c8d956004993340630208bf
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837063"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.Forms ActivityIndicator
[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)

Xamarin.Forms[ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) 응용 프로그램 작업 시간이 오래 걸리는 작업에 포함 되어 표시할 애니메이션을 표시 하는 컨트롤입니다. 달리 합니다 [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar)는 `ActivityIndicator` 제공 진행률 표시 되지 않습니다. 합니다 `ActivityIndicator` 에서 상속 [ `View` ](xref:Xamarin.Forms.View)합니다.

다음 스크린샷은 `ActivityIndicator` iOS 및 Android에서 제어 합니다.

![IOS 및 Android에서 ActivityIndicator의 스크린 샷](activityindicator-images/activityindicators-default.png "iOS 및 Android에서 ActivityIndicator의 스크린 샷")

`ActivityIndicator` 컨트롤에는 다음 속성을 정의 합니다.

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) `bool` 나타내는 값 여부를 `ActivityIndicator` 및 애니메이션을 표시할지 숨길지 이어야 합니다. 값이 `false` 는 `ActivityIndicator` 표시 되지 않습니다.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color) `Color` 의 표시 색을 정의 하는 값을 `ActivityIndicator`입니다.

이러한 속성에 의해 지원 됩니다 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) 의미 하는 개체는 `ActivityIndicator` 데이터 바인딩의 대상 및 스타일을 지정할 수 있습니다.

## <a name="create-an-activityindicator"></a>ActivityIndicator 만들기

`ActivityIndicator` XAML에서 인스턴스화할 수 있습니다. 해당 `IsRunning` 컨트롤이 표시 되 고 애니메이션 인지 확인 하려면 속성을 설정할 수 있습니다. 경우는 `IsRunning` 속성을 설정 하지 않으면, 기본값은 `false` 고 `ActivityIndicator` 표시 되지 않습니다. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ActivityIndicator` 선택적 XAML에서 `IsRunning` 속성 집합:

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator` 코드에서 만들 수도 있습니다.

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator 모양 속성

합니다 `Color` 정의에 속성을 설정할 수 있습니다는 `ActivityIndicator` 색입니다. 다음 예제에서는 인스턴스화하는 방법을 보여 줍니다는 `ActivityIndicator` 사용 하 여 XAML에서는 `Color` 속성 집합:

```xaml
<ActivityIndicator Color="Orange" />
```

합니다 `Color` 속성을 만들 때 설정할 수도 있습니다는 `ActivityIndicator` 코드에서:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

다음 스크린 샷에 표시 된 `ActivityIndicator` 사용 하 여는 `Color` 속성으로 설정 `Color.Orange` iOS 및 Android에서:

![스타일이 적용 된 ActivityIndicator ios 및 Android의 스크린 샷](activityindicator-images/activityindicators-styled.png "스타일이 적용 된 ActivityIndicator ios 및 Android의 스크린 샷")

## <a name="related-links"></a>관련 링크

* [ActivityIndicator 데모](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
