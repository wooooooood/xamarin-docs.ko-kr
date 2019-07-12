---
title: 기존 iOS 앱 업데이트
description: 이 문서에는 통합 API로 클래식 API의 Xamarin.iOS 앱을 업데이트 하려면 따라야 할 단계를 설명 합니다.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 4e471aaca2a7a5f247b21dd1c1863a01b062a716
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855748"
---
# <a name="updating-existing-ios-apps"></a>기존 iOS 앱 업데이트

_통합 API를 사용 하도록 기존 Xamarin.iOS 앱을 업데이트 하려면 다음이 단계를 수행 합니다._

프로젝트 파일 자체에 네임 스페이스 및 응용 프로그램 코드에서 사용 되는 Api에 대 한 변경 내용이 필요 통합 API를 사용 하도록 기존 앱을 업데이트 합니다.

## <a name="the-road-to-64-bits"></a>64 비트가는 길

새 통합 Api Xamarin.iOS 모바일 응용 프로그램에서 64 비트 장치 아키텍처를 지원 해야 합니다. 2015 년 2 월 1 일부 터 Apple iTunes 앱 스토어에 모든 새 응용 프로그램 제출을 64 비트 아키텍처를 지원 해야 합니다.

Xamarin은 Mac 용 Visual Studio와 Visual Studio 통합 API로 클래식 API에서 마이그레이션 프로세스를 자동화 하는 도구를 제공 하거나 프로젝트 파일을 수동으로 변환할 수 있습니다. 자동 도구를 통해이 가장 좋습니다, 하지만이 문서에서는 두 방법 모두를 설명 합니다.

### <a name="before-you-start"></a>시작 하기 전에...

기존 코드를 Unified API로 업데이트 하기 전에 것이 좋습니다 모두 제거 *컴파일 경고*합니다. 많은 *경고* 클래식 API에서 될 오류 통합 마이그레이션하면 됩니다. 클래식 API에서 컴파일러 메시지를 자주 업데이트에 힌트를 제공 하기 때문에 시작 하기 전에 해결 하는 것이 더 쉽습니다.

## <a name="automated-updating"></a>자동 업데이트

경고 해결 되었고 되 면 Mac 또는 Visual Studio에 대 한 Visual Studio에서 기존 iOS 프로젝트를 선택 하 고 선택 **Xamarin.iOS 통합 API로 마이그레이션** 에서 합니다 **프로젝트** 메뉴. 예를 들어:

![](updating-ios-apps-images/beta-tool1.png "프로젝트 메뉴에서 Xamarin.iOS 통합 API로 마이그레이션 선택")

자동화 된 마이그레이션을 실행 하기 전에이 경고 메시지에 동의 해야 합니다. (물론 해야이 adventure을 시작 하기 전에 백업/원본 제어 해야) 합니다.

![](updating-ios-apps-images/beta-tool2.png "자동화 된 마이그레이션을 실행 하기 전에이 경고에 동의")

에 설명 된 모든 단계를 기본적으로 자동화 하는 도구를 **수동으로 업데이트** 섹션 아래에 나타나고 통합 API로 기존 Xamarin.iOS 프로젝트를 변환 하는 방법으로 제안된 됩니다.

## <a name="steps-to-update-manually"></a>수동으로 업데이트 하는 단계

다시 경고를 수정한 후 새 통합 API 사용 하 여 Xamarin.iOS 앱을 수동으로 업데이트 하려면 다음이 단계를 수행 합니다.

### <a name="1-update-project-type--build-target"></a>1. 업데이트 프로젝트 형식 & 빌드 대상

프로젝트 버전을 변경 하 **csproj** 에서 파일을 `6BC8ED88-2882-458C-8E55-DFD12B67127B` 에 `FEACFBD2-3405-455C-9665-78FE426C6842`입니다. 편집 합니다 **csproj** 에서 첫 번째 항목을 바꿀 텍스트 편집기에서 파일을 `<ProjectTypeGuids>` 같이 요소:

![](updating-ios-apps-images/csproj.png "표시 된 것 처럼 ProjectTypeGuids 요소가 첫 번째 항목을 대체 텍스트 편집기에서 csproj 파일 편집")

변경 된 **가져오기** 포함 하는 요소 `Xamarin.MonoTouch.CSharp.targets` 에 `Xamarin.iOS.CSharp.targets` 같이:

![](updating-ios-apps-images/csproj2.png "표시 된 것 처럼 Xamarin.MonoTouch.CSharp.targets Xamarin.iOS.CSharp.targets 포함 하는 Import 요소를 변경 합니다.")

### <a name="2-update-project-references"></a>2. 프로젝트 참조를 업데이트 합니다.

IOS 응용 프로그램 프로젝트를 확장 **참조가** 노드. 처음에 표시 됩니다는 * 중단- **monotouch** (방금 프로젝트 형식 변경) 때문이 스크린샷과 유사한 참조:

![](updating-ios-apps-images/references.png "처음에 표시 됩니다 monotouch 끊어진 참조를이 스크린샷과 유사 하 게 프로젝트 형식 변경")

IOS 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 **참조 편집**를 클릭 합니다 **monotouch** 참조 하 고 빨간색 "X" 단추를 사용 하 여 삭제 합니다.

![](updating-ios-apps-images/references-delete-monotouch-sml.png "참조 편집 iOS 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 monotouch 참조를 클릭 하 고 빨간색을 사용 하 여 삭제할 X 단추")

이제 참조 목록 및 눈금의 끝으로 스크롤하여 합니다 **Xamarin.iOS** 어셈블리입니다.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "이제 참조 목록 및 Xamarin.iOS 어셈블리 눈금의 끝으로 스크롤하여")

키를 눌러 **확인** 프로젝트 참조 변경 내용을 저장 합니다.

### <a name="3-remove-monotouch-from-namespaces"></a>3. 네임 스페이스에서 MonoTouch 제거

제거 된 **MonoTouch** 접두사에 네임 스페이스를 `using` 문이나 아무 곳에 나는 classname가 된 정규화 된 (예: `MonoTouch.UIKit` 방금 됩니다 `UIKit`).

### <a name="4-remap-types"></a>4. 형식을 다시 매핑

[네이티브 형식](~/cross-platform/macios/nativetypes.md) 된 대체 하는 등 이전에 사용 된 일부 형식 도입 `System.Drawing.RectangleF` 사용 하 여 `CoreGraphics.CGRect` (예). 형식의 전체 목록에서 찾을 수 있습니다 합니다 [네이티브 형식을](~/cross-platform/macios/nativetypes.md) 페이지입니다.

### <a name="5-fix-method-overrides"></a>5. 메서드 재정의 수정 합니다.

일부 `UIKit` 메서드를 사용 하도록 변경 하는 서명 했 [네이티브 형식](~/cross-platform/macios/nativetypes.md) (같은 `nint`). 사용자 지정 서브 클래스에서 이러한 메서드를 재정의 하는 경우는 더 이상 일치 하지 서명과 오류가 발생 합니다. 네이티브 형식을 사용 하 여 새 시그니처를 일치 하도록 서브 클래스를 변경 하 여 이러한 메서드 재정의 수정 합니다.

변경 하는 것이 예로 `public override int NumberOfSections (UITableView tableView)` 반환할 `nint` 둘 다 반환 형식과 매개 변수 형식에서 변경 `public override int RowsInSection (UITableView tableView, int section)` 에 `nint`입니다.

## <a name="considerations"></a>고려 사항

다음 고려 사항이 하나 이상의 구성 요소 또는 NuGet 패키지에서 해당 앱이 의존 하는 경우 새 통합 API로 기존 Xamarin.iOS 프로젝트에서 클래식 API를 변환 하는 경우 계정에 취해야 합니다.

### <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소는 통합 API로 업데이트 해야 할 또는 컴파일 하려고 할 때 충돌을 받습니다. 모든 포함 된 구성 요소에 대 한 통합 API를 지 원하는 Xamarin Component Store에서 새 버전을 사용 하 여 현재 버전을 교체 하 고 클린 빌드를 수행 합니다. 작성자에 의해 아직 전환 되지 않은 모든 구성 요소는 32 비트 구성 요소 저장소에만 경고를 표시 합니다.

### <a name="nuget-support"></a>NuGet 지원

Unified API 지원을 통해 작동 하도록 NuGet에 변경 내용을 제공한 것 동안 없었습니다 NuGet의 새 릴리스 하므로 새 Api를 인식 하는 NuGet을 가져오는 방법을 평가 하는 것입니다.

구성 요소와 마찬가지로 통합 Api를 지 원하는 버전으로 프로젝트에 포함 된 모든 NuGet 패키지를 전환 하는 클린 빌드를 나중에 수행 해야 합니다.

> [!IMPORTANT]
> 폼에 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에서 'monotouch.dll' 및 'Xamarin.iOS.dll'를 포함할 수 없습니다-'Xamarin.iOS.dll'는 'monotouch.dll'에서 참조 하는 동안 명시적으로 참조 됩니다 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ Unified Api로 응용 프로그램으로 변환 후 일반적으로 통합 API로 업데이트 되지 않은 프로젝트에서 구성 요소 또는 NuGet 패키지 사용 하지 때문입니다. 기존 구성 요소/NuGet을 제거 하 고, 통합 Api를 지 원하는 버전으로 업데이트 하 고, 클린 빌드를 수행 해야 합니다.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자는 여전히 앱의 옵션에서 64 비트 컴퓨터에 대 한 응용 프로그램을 작성 하는 사용 하도록 설정 해야 합니다. 참조 하세요 합니다 **사용 하도록 설정 하면 64 비트 빌드의 Xamarin.iOS 앱** 의 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64/index.md#enable-64) 64 비트를 사용 하도록 설정 하는 자세한 지침은 문서 작성 합니다.

## <a name="finishing-up"></a>마무리

자동 또는 수동 메서드를 사용 하 여 클래식에서 Xamarin.iOS 응용 프로그램 통합 Api로 변환할 하려는 여부 또는 수동 작업을 추가 해야 하는 여러 인스턴스가 있습니다. 하십시오 우리의 [통합 API로 코드 업데이트에 대 한 팁](~/cross-platform/macios/unified/updating-tips.md) 알려진된 문제에 대 한 문서 및 해결 방법입니다.

## <a name="related-links"></a>관련 링크

- [코드를 Unified API로 업데이트하는 팁](~/cross-platform/macios/unified/updating-tips.md)
- [플랫폼 간 앱에서의 네이티브 형식 작업](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
