---
title: 진행률 및 Xamarin.iOS에서 활동 표시기
description: 이 문서에서는 Xamarin.iOS에서 진행률 및 활동 표시기를 사용 하는 방법을 설명 합니다. 프로그래밍 방식으로 및 스토리 보드를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: d39170d0109d7f81d3f02ec36381ebcd46c0143d
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233525"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>진행률 및 Xamarin.iOS에서 활동 표시기

이 앱을 장기 수행할 수 있을 로드 또는 데이터 및이 지연 UI를 업데이트 하는 지연 될 수 있습니다는 처리와 같은 작업을 실행 합니다. 이 시간 동안 항상 시스템 사용량이 많은 작업을 수행 하는 사용자를 원할 것에 진행률 표시기를 사용 해야 합니다. 이렇게 하면 사용자 컨트롤의 의견을 기다리고 있지 않으면 해당 요청을에 앱이 작동을 정확 하 게 얼마나 오래 대기 해야 하는 자세히 설명 하는 수단을 제공할 수 있습니다.

iOS 앱에서이 진행률 표시를 제공 하는 두 가지를 제공 합니다. 활동 표시기 (특정을 포함 하 여 _네트워크_ 활동 표시기) 및 진행률 표시줄입니다.

## <a name="activity-indicator"></a>활동 표시기

앱이 긴 프로세스를 실행 하지만 작업 해야 하는 시간의 정확한 길이 알 수 없는 경우 활동 표시기를 표시 합니다.

Apple 활동 표시기를 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **가능 하면 대신 진행률 막대** 활동 표시기 사용자는 실행 중인 프로세스 얼마에 대 한 피드백을 제공 하기 때문에-(예: 파일에 다운로드 하는 바이트 수) 길이 알고 있는 경우 진행률 표시줄을 항상 사용 합니다.
- **표시기 애니메이션 유지** -사용자와 관련 고정 활동 표시기를 제거한 다음된 앱에 표시 되 면 애니메이션 표시기 항상 있어야 하므로 합니다.
- **처리 중인 태스크를 설명할** -충분 하지 않습니다만 단독으로 활동 표시기를 표시, 사용자에서 대기 하는 프로세스에 대해 숙지 해야 합니다. 명확 하 게 작업을 정의 하는 의미 있는 레이블을 (일반적으로 단일, 완전 한 문장)를 포함 합니다.

### <a name="implementing-an-activity-indicator"></a>활동 표시기 구현

활동 표시기를 통해 구현 됩니다는 [ `UIActivityIndictorView` ](xref:UIKit.UIActivityIndicatorView) 나타내는 클래스를 `UIActivity` 을 수행 합니다.

### <a name="activity-indicators-and-storyboards"></a>스토리 보드 및 활동 표시기

IOS 디자이너 UI를 만들려면를 사용 하는 경우 도구 상자에서 레이아웃을 활동 표시기에 추가할 수 있습니다. Properties Pad에서 다음 속성을 조정할 수 있습니다.

![Properties Pad](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>활동 표시기 동작 관리

사용 된 `StartAnimating()` 및 `StopAnimating()` 시작 및 활동 표시기 애니메이션을 중지 하는 메서드.

설정 합니다 `HidesWhenStopped` 속성을 `true` 사라집니다 활동 표시기를 확인 하려면 `StopAnimating()` 가 호출 된 합니다. 이 설정은 `true` 기본적으로 합니다. 언제 든 지 활동 표시기를 확인 하 여 해당 회전 애니메이션을 실행 중인 경우를 볼 수는 `IsAnimating` 속성입니다. 


### <a name="managing-activity-indicator-appearances"></a>관리 활동 표시기 모양

`UIActivityIndicatorViewStyle` 열거형 활동 표시기를 인스턴스화할 때 매개 변수로 전달할 수 있습니다. 비주얼 스타일을 설정 하는 데 사용할 수 있습니다 `Gray`하십시오 `White`, 또는 `WhiteLarge`예를 들어:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

제공 하는 색을 재정의할 수 있습니다 `UIActivityIndicatorViewStyle` 설정 하 여는 `Color` 속성입니다.

## <a name="progress-bar"></a>Progress Bar

진행률 표시줄을 채우는 시간이 오래 걸리는 작업의 길이 나타내는 색을 사용 하 여 선으로 표시 합니다. 진행률 표시줄 항상 때 사용할 작업의 길이 알거나 계산할 수 있습니다.

Apple에는 진행률 표시줄을 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **진행률을 보고 정확 하 게** -진행률 표시줄에 작업을 완료 하는 데 필요한 시간이의 정확한 표현 해야 합니다. 앱 사용 중에 나타나도록 하는 시간을 조작할 하지 않습니다.
- **Well-Defined 기간에 대 한 사용 하 여** -진행률 표시줄에 긴 작업을이 걸린다는 표시 되지 않아야에 있지만 사용자 및 완료 된 작업의 양을 표시 및 예상 남은 기간을 제공 합니다.

### <a name="implementing-an-progress-bar"></a>진행률 표시줄을 구현합니다.

진행률 표시줄을 인스턴스화하여 생성 되는 [`UIProgressView`](xref:UIKit.UIProgressView)

### <a name="progress-bars-and-storyboards"></a>진행률 표시줄 및 스토리 보드

또한 iOS 디자이너를 사용 하는 경우 ui 진행률 표시줄을 추가할 수 있습니다. 검색할 **진행률 보기** 에 **도구 상자** 보기로 끕니다.

Properties pad에서 다음 속성을 조정할 수 있습니다.

![Properties Pad](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>진행률 표시줄 동작 관리

진행률 표시줄을 처음 사용 하 여 설정할 수 있습니다는 `Progress` 속성:

```csharp
ProgressBar.Progress = 0f;
```

사용 하 여 진행 상태를 조정할 수 있습니다는 `SetProgress` 메서드와 여부 애니메이션 변경 하려는 경우 선언 하는 부울 값을 전달 합니다.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

진행률 표시줄을 사용 하 여에 대 한 자세한 내용은 참조는 [진행률 보고](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) 작성법 및 [UICatalog tvOS 샘플](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/)합니다.

### <a name="managing-progress-bar-appearance"></a>진행률 표시줄 모양 관리

활동 표시기 비슷합니다는 `UIProgressViewStyle` 열거형 진행률 표시줄을 인스턴스화할 때 매개 변수로 전달할 수 있습니다.

다음 속성을 사용 하 여 진행률 및 추적 이미지 색조 색을 조정할 수 있습니다.

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



