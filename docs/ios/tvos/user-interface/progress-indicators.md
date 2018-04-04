---
title: 진행률 표시기 작업
description: 이 문서에서는 디자인 하 고 진행률 표시기 Xamarin.tvOS 앱 내에서 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 96fc3ea0aa802f62bd697b34f7bd504eb445a4f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-progress-indicators"></a>진행률 표시기 작업

_이 문서에서는 디자인 하 고 진행률 표시기 Xamarin.tvOS 앱 내에서 작업을 설명 합니다._


새 콘텐츠를 로드 하거나 시간이 오래 걸리는 처리 작업을 수행 하려면 Xamarin.tvOS 앱 필요한 시간이 있을 수 있습니다. 이 시간 동안 활동 표시기 또는 진행률 표시줄 사용자에 게 알리는 앱이 여전히 실행 하 고 실행 중인 작업의 길이 대 한 일부 원인을 제공을 제공 해야 합니다.

[![](progress-indicators-images/intro01.png "샘플 진행률 표시기")](progress-indicators-images/intro01.png#lightbox)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>활동 표시기에 대 한

활동 표시기 회전 톱니바퀴로 시각적으로 제공 하 고는 결정 되지 않은 길이의 작업을 나타내는 데 사용 됩니다. 작업이 시작 되 고 작업이 완료 되 면 사라집니다 표시기 표시 됩니다.

Apple 활동 표시기를 사용 하기 위한 다음 제안 사항을 있습니다.

- **진행률 막대 대신에 사용 가능한 경우 항상** -활동 표시기 사용자에 게 제공 하면 프로세스가 실행 되 고 시간은 어느 정도 걸리나요에 대 한 알 수 없습니다 (예: 파일에 다운로드 하는 바이트 수)의 길이 알고 있는 경우 진행률 표시줄을 항상 사용 합니다.
- **표시기 애니메이션 유지** -사용자가 표시 되 면에 애니메이션을 표시기는 항상 있어야 하므로 고정 활동 표시기 지연 된 응용 프로그램에 연결 합니다.
- **처리 중인 작업에 설명** -충분 하지 않을 단독으로 활동 표시기를 표시에서 대기 중인 프로세스에 대 한 알림을 받을 사용자가 해야 합니다. 명확 하 게 작업을 정의 하는 의미 있는 label (일반적으로 단일, 전체 문장을)이 포함 됩니다.

<a name="Summary" />

## <a name="about-progress-bars"></a>진행률 표시줄에 대 한

진행률 표시줄 상태와 시간이 많이 걸리는 작업의 길이 나타내는 색을 채웁니다 하는 선으로 표시 합니다. 작업의 길이 모르거나 계산할 수 있는 경우에 항상 진행률 표시줄을 사용 해야 합니다.

Apple에 진행률 표시줄을 사용 하기 위한 다음 제안 사항을:

- **진행률 보고 정확 하 게** -진행률 표시줄 항상 정확 하 게 작업을 완료 하는 데 필요한 시간 표현 되어야 합니다. 응용 프로그램 사용 중 표시 하는 데 시간이 조작할 하지 않습니다.
- **Well-Defined 기간에 대 한 사용** -진행률 표시줄 해야 보여 줄 뿐만 아니라 긴 작업 소요 되을 배치 하지만 사용자 및 완료 된 작업의 양을의 표시 및 예상 남은 기간에 게 부여 합니다.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>진행률 표시기 및 스토리 보드

진행률 표시기 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. 에 **솔루션 패드**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **활동 표시기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](progress-indicators-images/activity01.png "활동 표시기")](progress-indicators-images/activity01.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 패드**와 같은 활동 표시기의 여러 속성을 조정할 수 있습니다는 **스타일** 및 **동작**: 

    [![](progress-indicators-images/activity02.png "위젯 탭 ")](progress-indicators-images/activity02.png#lightbox)
1. 끌어서는 **진행 보기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](progress-indicators-images/activity03.png "진행률 보기")](progress-indicators-images/activity03.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 탐색기**와 같은 진행 보기의 여러 속성을 조정할 수 있습니다는 **스타일** 및 **진행률**(완료율): 

    [![](progress-indicators-images/activity04.png "위젯 탭")](progress-indicators-images/activity04.png#lightbox)
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예를 들어: 

    [![](progress-indicators-images/activity05.png "이름을 할당합니다")](progress-indicators-images/activity05.png#lightbox)
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **활동 표시기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](progress-indicators-images/activity01-vs.png "활동 표시기")](progress-indicators-images/activity01-vs.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 탐색기**와 같은 활동 표시기의 여러 속성을 조정할 수 있습니다는 **스타일** 및 **동작**: 

    [![](progress-indicators-images/activity02-vs.png "위젯 탭")](progress-indicators-images/activity02-vs.png#lightbox)
1. 끌어서는 **진행 보기** 에서 **도구 상자** 보기에 놓습니다. 

    [![](progress-indicators-images/activity03-vs.png "진행률 보기")](progress-indicators-images/activity03-vs.png#lightbox)
1. 에 **위젯을 탭** 의 **속성 탐색기**와 같은 진행 보기의 여러 속성을 조정할 수 있습니다는 **스타일** 및 **진행률**(완료율): 

    [![](progress-indicators-images/activity04-vs.png "위젯 탭")](progress-indicators-images/activity04-vs.png#lightbox)
1. 마지막으로 할당 **이름** 컨트롤에 C# 코드에서에 응답할 수 있도록 합니다. 예를 들어: 

    [![](progress-indicators-images/activity05-vs.png "이름을 할당합니다")](progress-indicators-images/activity05-vs.png#lightbox)
1. 변경 내용을 저장합니다.

-----

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>활동 표시기 작업

위에서 설명한 대로 응용 프로그램에서 긴 프로세스를 실행 하 되 고 있지만 작업에서는 시간의 정확한 길이 알 수 없는 경우 활동 표시기를 표시 되어야 합니다.

언제 든 지 활동 표시기를 확인 하 여 해당 회전 애니메이션을 실행 중인지 확인할 수 있습니다는 `IsAnimating` 속성입니다. 경우는 `HidesWhenStopped` 속성은 `true`, 활동 표시기 애니메이션이 중지 되 면에 자동으로 숨길 수 있습니다.

애니메이션을 시작 하려면 다음 코드를 사용할 수 있습니다. 

```csharp
ActivityIndicator.StartAnimating();
```

및 다음는 애니메이션 중지 됩니다.

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>작업 진행률 표시줄

다시, 진행률 표시줄의 앱 알려진 기간의 장기 실행 작업을 실행 하 든 지 사용 되어야 합니다. 

`Progress` 속성은 0%에서 100% (0.0 ~ 1.0)로 완료 된 작업의 크기를 설정 하는 데 사용 합니다. 사용 하 여는 `ProgressTintColor` 완료 시간 표시줄의 색을 설정 하는 속성 및 `TrackTintColor` 속성 (완료 되지 않은 양) 배경색을 설정 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 진행률 표시기 Xamarin.tvOS 앱 내에서 작업 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
