---
title: TvOS Xamarin에서 진행률 표시기 작업
description: 이 문서에서는 Xamarin에 내장 된 tvOS 앱에서 진행률 표시기를 사용 하는 방법을 설명 합니다. 진행률 표시줄 및 활동 표시기를 둘 다에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/25/2018
ms.openlocfilehash: cbd2b2de237a5bb22d1dc0242569b96b12bca070
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61180739"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>TvOS Xamarin에서 진행률 표시기 작업

_이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 진행률 표시기를 사용 하 여 작업을 설명 합니다._

Xamarin.tvOS 앱 새 콘텐츠를 로드 하거나 처리 시간이 긴 작업을 수행 해야 하는 경우 시간이 있을 수 있습니다. 이 시간 동안 활동 표시기 또는 사용자가 앱을 계속 실행 되 고 있는지 알 수 있도록 일부 알려 실행 중인 작업의 길이 제공 하는 진행률 표시줄을 표시 해야 있습니다.

![진행률 표시기를 샘플](progress-indicators-images/intro01.png "샘플 진행률 표시기")

## <a name="about-activity-indicators"></a>활동 표시기에 대 한

활동 표시기를 회전 코그로 제공 및의 길이 지정 하지 않은 작업을 나타내는 데 사용 됩니다. 작업이 시작 되 고 작업이 완료 되 면 사라집니다 표시기 표시 됩니다.

Apple 활동 표시기를 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **가능 하면 대신 진행률 표시줄** -길이 (예: 파일에 다운로드 하는 바이트 수) 라고 하는 경우 항상를 활동 표시기 제공 하는 방법에 대 한 피드백이 없습니다 장기 실행 중인 프로세스를 사용 하는 진행률 표시줄을 사용 합니다.
- **애니메이션 된 표시기를 유지** -사용자는 항상 애니메이션 효과 주는 표시기 표시 되 면 되므로 고정 활동 표시기를 제거한 다음 앱에 관련 됩니다.
- **처리 중인 태스크를 설명할** -충분 하지 단순히 자체로 활동 표시기를 표시 않고 사용자가 대기 중인 프로세스에 대해 숙지 해야 합니다. 명확 하 게 작업을 정의 하는 의미 있는 레이블을 (일반적으로 단일, 완전 한 문장)를 포함 합니다.

## <a name="about-progress-bars"></a>진행률 표시줄에 대 한

진행률 표시줄을 채우는 시간이 오래 걸리는 작업의 길이 나타내는 색을 사용 하 여 선으로 표시 합니다. 진행률 표시줄 항상 때 사용할 작업의 길이 알 수 없거나 계산할 수 있습니다.

Apple에는 진행률 표시줄을 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **정확 하 게 진행률을 보고** -진행률 표시줄에는 작업을 완료 하는 데 필요한 시간을 정확 하 게 표현한 항상 존재 해야 합니다. 앱 사용 중에 나타나도록 하는 시간을 조작할 하지 않습니다.
- **잘 정의 된 기간에 대 한 사용 하 여** -막대 긴 작업 소요 되는 표시 되지 않아야 하는 진행률에 있지만 사용자 및 완료 된 작업의 양을 표시 및 예상 남은 기간을 제공 합니다.

## <a name="progress-indicators-and-storyboards"></a>진행률 표시기 및 스토리 보드

Xamarin.tvOS 앱에서 진행률 표시기를 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. 에 **Solution Pad**를 두 번 클릭 합니다 **Main.storyboard** 파일을 편집용으로 엽니다.

2. 끌어서를 **활동 표시기** 에서 **도구 상자** 보기에 놓습니다. 

    ![활동 표시기](progress-indicators-images/activity01.png "활동 표시기")

3. 에 **위젯** 탭을 **Properties Pad**와 같은 활동 표시기의 여러 속성을 조정할 수 있습니다 해당 **스타일**, **동작**, 및 **이름을**: 

    ![활동 표시기에 대 한 위젯 탭](progress-indicators-images/activity02.png "활동 표시기는 위젯을 탭")
    
    합니다 **이름을** 활동 표시기를 나타내는 속성의 이름을 결정 C# 코드입니다.

4. 끌어서를 **진행률 보기** 에서 **도구 상자** 보기에 놓습니다. 

    ![진행률 보기](progress-indicators-images/activity03.png "진행률 보기")

5. 에 **위젯** 탭의 **속성 탐색기**와 같은 진행률 보기의 여러 속성을 조정할 수 있습니다 해당 **스타일**, **진행률**(%)를 완료 하 고 **이름을**: 

    ![진행률 보기에 대 한 위젯 탭](progress-indicators-images/activity04.png "진행률 보려면 The 위젯을 탭")
    
    합니다 **이름을** 의 진행률 뷰를 나타내는 속성의 이름을 결정 하는 C# 코드입니다.

6. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 **Main.storyboard** 파일을 편집용으로 엽니다.

2. 끌어서를 **활동 표시기** 에서 **도구 상자** 보기에 놓습니다. 

    ![활동 표시기](progress-indicators-images/activity01-vs.png
    "활동 표시기")

3. 에 **위젯** 탭의 **속성 탐색기**와 같은 활동 표시기의 여러 속성을 조정할 수 있습니다 해당 **스타일**, **동작**, 및 **이름을**: 

    ![활동 표시기에 대 한 위젯 탭](progress-indicators-images/activity02-vs.png "활동 표시기는 위젯을 탭")

    합니다 **이름을** 활동 표시기를 나타내는 속성의 이름을 결정 C# 코드입니다.

4. 끌어서를 **진행률 보기** 에서 **도구 상자** 보기에 놓습니다. 

   ![진행률 보기](progress-indicators-images/activity03-vs.png "진행률 보기")

5. 에 **위젯** 탭의 **속성 탐색기**와 같은 진행률 보기의 여러 속성을 조정할 수 있습니다 해당 **스타일**, **진행률**(%)를 완료 하 고 **이름을**: 

    ![진행률 보기에 대 한 위젯 탭](progress-indicators-images/activity04-vs.png "진행률 보려면 The 위젯을 탭")
    
    합니다 **이름을** 의 진행률 뷰를 나타내는 속성의 이름을 결정 하는 C# 코드입니다.

6. 변경 내용을 저장합니다.

-----

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

## <a name="working-with-activity-indicators"></a>활동 표시기 작업

위에서 설명한 대로 앱이 비활성화 길이의 긴 프로세스를 실행 하는 경우 활동 표시기 표시 됩니다.

언제 든 지 표시 활동 표시기를 확인 하 여 애니메이션 효과 하는 경우 해당 `IsAnimating` 속성입니다. 경우는 `HidesWhenStopped` 속성은 `true`, 활동 표시기 애니메이션이 중지 되 면 자동으로 숨겨지지 것입니다.

애니메이션을 시작 하려면 다음 코드를 사용할 수 있습니다. 

```csharp
ActivityIndicator.StartAnimating();
```

및 다음 애니메이션을 중지 됩니다.

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> 이러한 코드 조각은 가정 활동 표시기 **이름을** 로 설정 된 **ActivityIndicator** 에 **위젯** iOS 디자이너의 탭 합니다.

## <a name="working-with-progress-bars"></a>진행률 표시줄을 사용 하 여 작업

마찬가지로 앱의 알려진된 기간 장기 실행 작업을 실행 하 든 지 진행률 표시줄에 사용 되어야 합니다. 

`Progress` 속성은 0%에서 100% (0.0 ~ 1.0)로 완료 된 작업의 양을 설정 하려면 사용 합니다. 사용 합니다 `ProgressTintColor` 완료 하는 시간 표시줄의 색을 설정 하는 속성 및 `TrackTintColor` 배경색 (완료 되지 않은 크기)를 설정 하는 속성입니다.

## <a name="summary"></a>요약

이 문서에서는 디자인과 Xamarin.tvOS 앱 내에서 진행률 표시기를 사용 하 여 작업 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
