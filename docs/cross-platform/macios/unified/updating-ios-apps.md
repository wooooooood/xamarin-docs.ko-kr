---
title: 기존 iOS 앱 업데이트
description: 이 문서에서는 Xamarin.ios 앱을 Classic API에서 Unified API 업데이트 하기 위해 따라야 하는 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 9b531bd095781c80c5f3418725d57f8f6bbb06fd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015035"
---
# <a name="updating-existing-ios-apps"></a>기존 iOS 앱 업데이트

_Unified API를 사용 하도록 기존 Xamarin.ios 앱을 업데이트 하려면 다음 단계를 수행 합니다._

Unified API 사용 하도록 기존 앱을 업데이트 하려면 프로젝트 파일 자체를 변경 하 고 응용 프로그램 코드에 사용 되는 네임 스페이스와 Api를 변경 해야 합니다.

## <a name="the-road-to-64-bits"></a>64 비트로 이동

새 통합 Api는 Xamarin.ios 모바일 응용 프로그램에서 64 비트 장치 아키텍처를 지 원하는 데 필요 합니다. 2 월 1 일부 터 2015 Apple은 iTunes App Store에 대 한 모든 새 앱 제출이 64 비트 아키텍처를 지원 해야 합니다.

Xamarin은 Mac용 Visual Studio 및 Visual Studio 모두에서 Classic API Unified API 마이그레이션 프로세스를 자동화 하는 도구를 제공 합니다. 또는 프로젝트 파일을 수동으로 변환할 수 있습니다. 자동 도구를 사용 하는 것이 가장 좋습니다 .이 문서에서는 두 가지 방법을 모두 설명 합니다.

### <a name="before-you-start"></a>시작 하기 전에 ...

기존 코드를 Unified API 업데이트 하기 전에 모든 *컴파일 경고*를 제거 하는 것이 좋습니다. 통합으로 마이그레이션하면 Classic API의 많은 *경고가* 오류가 됩니다. Classic API의 컴파일러 메시지에서 업데이트할 내용에 대 한 힌트를 제공 하기 때문에 시작 하기 전에 이러한 문제를 수정 하는 것이 더 쉽습니다.

## <a name="automated-updating"></a>자동 업데이트

경고가 수정 되 면 Mac용 Visual Studio 또는 Visual Studio에서 기존 iOS 프로젝트를 선택 하 고 **프로젝트** 메뉴에서 **xamarin.ios로 마이그레이션 Unified API를** 선택 합니다. 예를 들면,

![](updating-ios-apps-images/beta-tool1.png "Choose Migrate to Xamarin.iOS Unified API from the Project menu")

자동화 된 마이그레이션을 실행 하기 전에이 경고에 동의 해야 합니다 .이 경우에는 형성 전에 백업/원본 제어를 유지 해야 합니다.

![](updating-ios-apps-images/beta-tool2.png "Agree to this warning before the automated migration will run")

이 도구는 기본적으로 아래에 표시 된 **업데이트 수동으로** 섹션에 설명 된 모든 단계를 자동화 하며 기존 xamarin.ios 프로젝트를 Unified API으로 변환 하는 데 권장 되는 방법입니다.

## <a name="steps-to-update-manually"></a>수동으로 업데이트 하는 단계

경고가 수정 되 면 다음 단계에 따라 Xamarin.ios 앱을 수동으로 업데이트 하 여 새 Unified API을 사용 합니다.

### <a name="1-update-project-type--build-target"></a>1. 프로젝트 형식 업데이트 & 빌드 대상

**.Csproj** 파일의 프로젝트 버전을 `6BC8ED88-2882-458C-8E55-DFD12B67127B`에서 `FEACFBD2-3405-455C-9665-78FE426C6842`로 변경 합니다. 텍스트 편집기에서 **.csproj** 파일을 편집 하 여 `<ProjectTypeGuids>` 요소의 첫 번째 항목을 다음과 같이 바꿉니다.

![](updating-ios-apps-images/csproj.png "Edit the csproj file in a text editor, replacing the first item in the ProjectTypeGuids element as shown")

`Xamarin.MonoTouch.CSharp.targets`를 포함 하는 **Import** 요소를 `Xamarin.iOS.CSharp.targets`에 다음과 같이 변경 합니다.

![](updating-ios-apps-images/csproj2.png "Change the Import element that contains Xamarin.MonoTouch.CSharp.targets to Xamarin.iOS.CSharp.targets as shown")

### <a name="2-update-project-references"></a>2. 프로젝트 참조 업데이트

IOS 응용 프로그램 프로젝트의 **참조** 노드를 확장 합니다. 처음에는 프로젝트 형식을 변경 했기 때문에이 스크린샷 처럼 * **monotouch.dialog** 참조를 표시 합니다.

![](updating-ios-apps-images/references.png "It will initially show a broken- monotouch reference similar to this screenshot because the project type changed")

IOS 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 하 여 **참조를 편집한**다음 **monotouch.dialog** 참조를 클릭 하 고 빨간색 "X" 단추를 사용 하 여 삭제 합니다.

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Right-click on the iOS application project to Edit References, then click on the monotouch reference and delete it using the red X button")

이제 참조 목록의 끝으로 스크롤하고 **xamarin.ios** 어셈블리를 tick.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Now scroll to the end of the references list and tick the Xamarin.iOS assembly")

**확인** 을 눌러 프로젝트 참조 변경 내용을 저장 합니다.

### <a name="3-remove-monotouch-from-namespaces"></a>3. 네임 스페이스에서 Monotouch.dialog 제거

`using` 문에서 네임 스페이스의 **monotouch.dialog** 접두사를 제거 하거나 classname이 정규화 된 모든 위치 (예: `MonoTouch.UIKit`만 `UIKit`).

### <a name="4-remap-types"></a>4. 형식 다시 매핑

이전에 사용 된 일부 형식 (예: `CoreGraphics.CGRect` `System.Drawing.RectangleF`의 인스턴스)을 대체 하는 [네이티브 형식이](~/cross-platform/macios/nativetypes.md) 도입 되었습니다. 형식에 대 한 전체 목록은 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 페이지에서 찾을 수 있습니다.

### <a name="5-fix-method-overrides"></a>5. 메서드 재정의 수정

일부 `UIKit` 메서드의 서명이 새 [네이티브 형식](~/cross-platform/macios/nativetypes.md) (예: `nint`)을 사용 하도록 변경 되었습니다. 사용자 지정 하위 클래스가 이러한 메서드를 재정의 하면 서명이 더 이상 일치 하지 않아 오류가 발생 합니다. 네이티브 형식을 사용 하 여 새 시그니처와 일치 하도록 하위 클래스를 변경 하 여 이러한 메서드 재정의를 수정 합니다.

예를 들어 `public override int NumberOfSections (UITableView tableView)`를 변경 하 여 `nint`을 반환 하 고 `public override int RowsInSection (UITableView tableView, int section)`의 반환 형식과 매개 변수 형식을 모두 `nint`로 변경 합니다.

## <a name="considerations"></a>고려 사항

응용 프로그램이 하나 이상의 구성 요소나 NuGet 패키지를 사용 하는 경우 기존 Xamarin.ios 프로젝트를 Classic API에서 새 Unified API로 변환할 때 다음 사항을 고려해 야 합니다.

### <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소를 Unified API 업데이트 해야 합니다. 그렇지 않으면 컴파일을 시도할 때 충돌이 발생 합니다. 포함 된 모든 구성 요소에 대해 현재 버전을 Unified API를 지 원하는 Xamarin 구성 요소 저장소의 새 버전으로 바꾸고 새로 빌드를 수행 합니다. 작성자가 아직 변환 하지 않은 구성 요소는 구성 요소 저장소에 32 비트만 표시 합니다.

### <a name="nuget-support"></a>NuGet 지원

Unified API 지원 작업을 수행 하기 위해 NuGet에 변경 내용을 적용 하는 동안 nuget의 새로운 릴리스가 출시 되지 않았기 때문에 NuGet을 가져와 새 Api를 인식 하는 방법을 평가 하 고 있습니다.

이 시점까지 구성 요소와 마찬가지로 프로젝트에 포함 된 NuGet 패키지를 통합 Api를 지 원하는 버전으로 전환 하 고 나중에 정리 빌드를 수행 해야 합니다.

> [!IMPORTANT]
> _"오류 3"에 ' monotouch.dialog ' 및 ' monotouch.dialog '가 모두 포함 될 수 없습니다. ' xamarin.ios '는 명시적으로 참조 되는 반면 ' '는 ' xxx, Version = 0.0.000, Culture =에 의해 참조 됩니다. 중립, PublicKeyToken = null ' "_ 응용 프로그램을 통합 된 api로 변환한 후에는 일반적으로 Unified API로 업데이트 되지 않은 구성 요소 또는 NuGet 패키지가 프로젝트에 포함 되어 있기 때문입니다. 기존 component/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.ios 앱의 64 비트 빌드 사용

Unified API로 변환 된 Xamarin.ios 모바일 응용 프로그램의 경우 개발자는 응용 프로그램의 옵션에서 64 비트 컴퓨터용 응용 프로그램 빌드를 계속 사용 하도록 설정 해야 합니다. 64 비트 빌드를 사용 하는 방법에 대 한 자세한 내용은 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#enable-64) 문서의 **64 비트 빌드 사용** 을 참조 하세요.

## <a name="finishing-up"></a>완료

자동 또는 수동 방법을 사용 하 여 Xamarin.ios 응용 프로그램을 클래식에서 통합 된 Api로 변환 하는지 여부에 관계 없이 더 많은 인스턴스가 필요 합니다. 알려진 문제 및 해결 방법에 대해서 [는 Unified API 문서에 대 한 코드 업데이트에 대 한 팁](~/cross-platform/macios/unified/updating-tips.md) 을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [코드를 Unified API로 업데이트하는 팁](~/cross-platform/macios/unified/updating-tips.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs Unified API 차이점](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
