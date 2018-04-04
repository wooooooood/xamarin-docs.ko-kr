---
title: 진행률 및 활동 표시기
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 58a492bed81a1d96a482c1396718da1c5e4af589
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="progress-and-activity-indicators"></a>진행률 및 활동 표시기

응용 프로그램 해야 long을 수행 하는 것은 로드 하거나 데이터와이 지연 UI를 업데이트 하는 중 지연이 발생할 수 있는지를 처리 하는 등의 작업을 실행 합니다. 이 시간 동안 항상 사용자가 작업을 수행 중 원할 것을 진행률 표시기를 사용 해야 합니다. 이렇게 하면 자신의 요청의 의견을 기다리고 있지 않으면 하 여 앱이 작동 하는 사용자 정의 컨트롤을 대기 해야 정확히 기간에 대해 자세히 설명 하는 방법을 제공할 수 있습니다.

iOS 응용 프로그램에서 이러한 진행률 지표를 제공 하는 다음 두 가지가 제공: 활동 표시기 (특정을 포함 하 여 _네트워크_ 활동 표시기) 및 진행률 표시줄입니다.

## <a name="activity-indicator"></a>활동 표시기

앱에서 긴 프로세스를 실행 하 되 고 있지만 작업에서는 시간의 정확한 길이 알 수 없는 경우 활동 표시기를 표시 합니다.

Apple 활동 표시기를 사용 하기 위한 다음 제안 사항을 있습니다.

- **진행률 막대 대신에 사용 가능한 경우 항상** -활동 표시기 사용자에 게 제공 하면 프로세스가 실행 되 고 시간은 어느 정도 걸리나요에 대 한 알 수 없습니다 (예: 파일에 다운로드 하는 바이트 수)의 길이 알고 있는 경우 진행률 표시줄을 항상 사용 합니다.
- **표시기 애니메이션 유지** -사용자가 표시 되 면에 애니메이션을 표시기는 항상 있어야 하므로 고정 활동 표시기 지연 된 응용 프로그램에 연결 합니다.
- **처리 중인 작업에 설명** -충분 하지 않을 단독으로 활동 표시기를 표시에서 대기 중인 프로세스에 대 한 알림을 받을 사용자가 해야 합니다. 명확 하 게 작업을 정의 하는 의미 있는 label (일반적으로 단일, 전체 문장을)이 포함 됩니다.

### <a name="implementing-an-activity-indicator"></a>활동 표시기를 구현합니다.

활동 표시기를 통해 구현 되는 [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) 을 나타내는 클래스는 `UIActivity` 수행 되 고 있습니다.

### <a name="activity-indicators-and-storyboards"></a>스토리 보드와 활동 표시기

IOS 디자이너를 만드는 UI를 사용 하는 경우 도구 상자에서 레이아웃에 활동 표시기에 추가할 수 있습니다. 속성 패드에서 다음 속성을 조정할 수 있습니다.

![속성 채움](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>활동 표시기 동작 관리

사용 하 여는 `StartAnimating()` 및 `StopAnimating()` 메서드 시작 하 고 활동 표시기 애니메이션을 중지 합니다.

설정의 `HidesWhenStopped` 속성을 `true` 사라집니다 활동 표시기를 `StopAnimating()` 가 호출 합니다. 이 설정은 `true` 기본적으로 합니다. 언제 든 지 활동 표시기를 확인 하 여 해당 회전 애니메이션을 실행 중인지 확인할 수 있습니다는 `IsAnimating` 속성입니다. 


### <a name="managing-activity-indicator-appearances"></a>활동 표시기 모양을 관리

`UIActivityIndicatorViewStyle` 열거형 활동 표시기를 인스턴스화할 때 매개 변수로 전달할 수 있습니다. 비주얼 스타일으로 설정 하는 데 사용할 수 있습니다 `Gray`, `White`, 또는 `WhiteLarge`, 예:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

제공 하는 색을 재정의할 수 `UIActivityIndicatorViewStyle` 설정 하 여는 `Color` 속성입니다.

## <a name="progress-bar"></a>Progress Bar

진행률 표시줄 상태와 시간이 많이 걸리는 작업의 길이 나타내는 색을 채웁니다 하는 선으로 표시 합니다. 작업의 길이 모르거나 계산할 수 있는 경우에 항상 진행률 표시줄을 사용 해야 합니다.

Apple에 진행률 표시줄을 사용 하기 위한 다음 제안 사항을:

- **진행률 보고 정확 하 게** -진행률 표시줄 항상 정확 하 게 작업을 완료 하는 데 필요한 시간 표현 되어야 합니다. 응용 프로그램 사용 중 표시 하는 데 시간이 조작할 하지 않습니다.
- **Well-Defined 기간에 대 한 사용** -진행률 표시줄 해야 보여 줄 뿐만 아니라 긴 작업 소요 되을 배치 하지만 사용자 및 완료 된 작업의 양을의 표시 및 예상 남은 기간에 게 부여 합니다.

### <a name="implementing-an-progress-bar"></a>진행률 표시줄을 구현합니다.

진행률 표시줄이 인스턴스화하여 생성 되는 [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>진행률 표시줄 및 스토리 보드

또한 iOS 디자이너를 사용 하는 경우 UI를 진행률 표시줄을 추가할 수 있습니다. 검색할 **진행 보기** 에 **도구 상자** 보기로 끕니다.

속성 패드에서 다음 속성을 조정할 수 있습니다.

![속성 채움](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>진행률 표시줄 동작 관리

진행률 표시줄의 처음 사용 하 여 설정할 수 있습니다는 `Progress` 속성:

```csharp
ProgressBar.Progress = 0f;
```

사용 하 여 진행률을 조정할 수 있습니다는 `SetProgress` 메서드와 여부에 애니메이션을 변경 하려는 경우 선언 하는 부울 값을 전달 합니다.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

진행률 표시줄을 사용 하 여에 대 한 자세한 내용은 참조는 [진행률 보고](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) 레 및 [UICatalog tvOS 샘플](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/)합니다.

### <a name="managing-progress-bar-appearance"></a>진행률 표시줄 모양 관리

활동 표시기는 비슷합니다는 `UIProgressViewStyle` 열거형 진행률 표시줄을 인스턴스화할 때 매개 변수로 전달할 수 있습니다.

Tint 색과 추적 이미지 진행률 및 다음과 같은 속성을 사용 하 여 조정할 수 있습니다.

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



