---
title: Xamarin에서 tvOS 진행률 표시기 사용
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 진행률 표시기를 사용 하는 방법을 설명 합니다. 진행률 표시줄과 활동 표시기를 모두 설명 합니다.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 202ce8d674a39b06fd1b07460dff4bf573062592
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70291411"
---
# <a name="working-with-tvos-progress-indicators-in-xamarin"></a>Xamarin에서 tvOS 진행률 표시기 사용

_이 문서에서는 tvOS 앱 내에서 진행률 표시기를 디자인 하 고 사용 하는 방법을 설명 합니다._

TvOS 앱에서 새 콘텐츠를 로드 하거나 긴 처리 작업을 수행 해야 하는 경우가 있을 수 있습니다. 이러한 시간 동안에는 응용 프로그램이 여전히 실행 되 고 있음을 사용자에 게 알리는 작업 표시기 또는 진행률 표시줄을 표시 하 고 실행 중인 작업의 길이에 대 한 표시를 제공 해야 합니다.

![샘플 진행률 표시기](progress-indicators-images/intro01.png "샘플 진행률 표시기")

## <a name="about-activity-indicators"></a>활동 표시기 정보

활동 표시기는 회전 코그 표시 되 고 결정 되지 않은 길이의 작업을 나타내는 데 사용 됩니다. 작업을 시작 하 고 작업이 완료 되 면 사라질 때 표시기가 표시 됩니다.

Apple에는 활동 표시기를 사용 하기 위한 다음과 같은 제안이 있습니다.

- **가능 하면 항상 진행률** 표시줄을 사용 합니다. 작업 표시기는 실행 중인 프로세스에 걸리는 시간에 대 한 피드백을 사용자에 게 제공 하지 않으므로 항상 길이를 알 수 있는 경우 (예: 파일에서 다운로드할 바이트 수) 항상 진행률 표시줄을 사용 합니다.
- **표시기를 애니메이션으로 유지** -사용자가 고정 된 작업 표시기를 지연 된 앱에 연결 하므로 표시 되는 동안 항상 표시기에 애니메이션 효과를 적용 해야 합니다.
- **처리 중인 작업에 대해 설명** 합니다. 작업 표시기를 단독으로 표시 하는 것 만으로는 충분 하지 않습니다. 사용자는 대기 중인 프로세스에 대해 알고 있어야 합니다. 작업을 명확 하 게 정의 하는 의미 있는 레이블 (일반적으로 단일 전체 문장)을 포함 합니다.

## <a name="about-progress-bars"></a>진행률 표시줄 정보

진행률 표시줄은 시간이 많이 걸리는 작업의 상태 및 길이를 나타내기 위해 색으로 채워진 선으로 나타납니다. 작업의 길이를 알 수 있거나 계산할 수 있는 경우에는 항상 진행률 표시줄을 사용 해야 합니다.

Apple에는 진행률 표시줄을 사용 하기 위한 다음과 같은 제안이 있습니다.

- **정확히 보고 진행률** -진행률 표시줄은 작업을 완료 하는 데 필요한 시간을 정확 하 게 표시 해야 합니다. 앱을 사용 중으로 표시 하는 시간을 조작할 수 없습니다.
- **잘 정의 된 기간에 사용** -진행률 표시줄은 시간이 오래 걸리는 작업을 표시 하는 것 뿐만 아니라 완료 된 작업의 양과 남은 시간을 보여 줍니다.

## <a name="progress-indicators-and-storyboards"></a>진행률 표시기 및 storyboard

TvOS 앱에서 진행률 표시기를 사용 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 **주 storyboard** 파일을 두 번 클릭 하 여 편집용으로 엽니다.

2. **도구 상자** 에서 **활동 표시기** 를 끌어서 뷰에 놓습니다. 

    ![활동 표시기](progress-indicators-images/activity01.png "활동 표시기")

3. **Properties Pad**의 **위젯** 탭에서 작업 표시기의 **스타일**, **동작**및 **이름과**같은 몇 가지 속성을 조정할 수 있습니다. 

    ![활동 표시기의 위젯 탭](progress-indicators-images/activity02.png "활동 표시기의 위젯 탭")
    
    **이름** 은 코드의 C# 작업 표시기를 나타내는 속성의 이름을 결정 합니다.

4. **도구 상자** 에서 **진행률 보기** 를 끌어서 뷰에 놓습니다. 

    ![진행률 보기](progress-indicators-images/activity03.png "진행률 보기")

5. **속성 탐색기**의 **위젯** 탭에서 진행률 보기의 **스타일**, **진행률** (완료율), **이름**등의 몇 가지 속성을 조정할 수 있습니다. 

    ![진행률 보기의 위젯 탭](progress-indicators-images/activity04.png "진행률 보기의 위젯 탭")
    
    **이름** 은 코드에서 C# 진행률 뷰를 나타내는 속성의 이름을 결정 합니다.

6. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 **주 storyboard** 파일을 두 번 클릭 하 여 편집용으로 엽니다.

2. **도구 상자** 에서 **활동 표시기** 를 끌어서 뷰에 놓습니다. 

    ![활동 표시기](progress-indicators-images/activity01-vs.png
    "활동 표시기")

3. **속성 탐색기**의 **위젯** 탭에서 작업 표시기의 **스타일**, **동작**및 **이름과**같은 몇 가지 속성을 조정할 수 있습니다. 

    ![활동 표시기의 위젯 탭](progress-indicators-images/activity02-vs.png "활동 표시기의 위젯 탭")

    **이름** 은 코드의 C# 작업 표시기를 나타내는 속성의 이름을 결정 합니다.

4. **도구 상자** 에서 **진행률 보기** 를 끌어서 뷰에 놓습니다. 

   ![진행률 보기](progress-indicators-images/activity03-vs.png "진행률 보기")

5. **속성 탐색기**의 **위젯** 탭에서 진행률 보기의 **스타일**, **진행률** (완료율), **이름**등의 몇 가지 속성을 조정할 수 있습니다. 

    ![진행률 보기의 위젯 탭](progress-indicators-images/activity04-vs.png "진행률 보기의 위젯 탭")
    
    **이름** 은 코드에서 C# 진행률 뷰를 나타내는 속성의 이름을 결정 합니다.

6. 변경 내용을 저장합니다.

-----

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요. 

## <a name="working-with-activity-indicators"></a>활동 표시기 작업

위에서 설명한 것 처럼 앱이 길이가 정해 지지 않은 긴 프로세스를 실행 하는 경우 활동 표시기가 표시 되어야 합니다.

언제 든 지 `IsAnimating` 속성을 확인 하 여 활동 표시기가 애니메이션 효과를 주는 지 확인할 수 있습니다. @No__t_0 속성이 `true` 이면 애니메이션이 중지 될 때 작업 표시기가 자동으로 숨겨집니다.

애니메이션을 시작 하려면 다음 코드를 사용할 수 있습니다. 

```csharp
ActivityIndicator.StartAnimating();
```

그리고 다음을 수행 하면 애니메이션이 중지 됩니다.

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> 이러한 코드 조각은 iOS 디자이너의 **위젯** 탭에서 활동 표시기의 **이름이** **activityindicator** 로 설정 된 것으로 가정 합니다.

## <a name="working-with-progress-bars"></a>진행률 표시줄 작업

앱이 알려진 기간의 장기 실행 작업을 실행할 때 언제 든 지 진행률 표시줄을 사용 해야 합니다. 

@No__t_0 속성은 완료 된 작업의 양을 0%에서 100% (0.0에서 1.0)로 설정 하는 데 사용 됩니다. @No__t_0 속성을 사용 하 여 완료 된 막대의 색을 설정 하 고 `TrackTintColor` 속성을 사용 하 여 배경색을 설정 합니다 (완료 되지 않은 금액).

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 진행률 표시기를 디자인 하 고 작업 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
