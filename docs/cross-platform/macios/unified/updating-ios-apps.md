---
title: "기존 iOS 앱을 업데이트합니다."
description: "통합 된 API를 사용 하는 기존 Xamarin.iOS 앱을 업데이트 하려면 다음이 단계를 수행 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: d5c8828557d5c5d3d3212378f94e7e0eb0902f61
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="updating-existing-ios-apps"></a>기존 iOS 앱을 업데이트합니다.

_통합 된 API를 사용 하는 기존 Xamarin.iOS 앱을 업데이트 하려면 다음이 단계를 수행 합니다._

통합 된 API를 사용 하는 기존 응용 프로그램 업데이트는 프로젝트 파일 자체에 네임 스페이스 및 응용 프로그램 코드에서 사용 되는 Api에 대 한 변경 해야 합니다.

# <a name="the-road-to-64-bits"></a>64 비트로 Road

새로운 통합 Api Xamarin.iOS 모바일 응용 프로그램에서 64 비트 장치 아키텍처를 지원 해야 합니다. 2015 년 2 월 1 일을 기준으로 Apple iTunes 앱 스토어의 새로운 모든 응용 프로그램 제출을 64 비트 아키텍처를 지원 해야 합니다.

Xamarin Mac 용 Visual Studio와 Visual Studio는 통합 API에 클래식 API에서 마이그레이션 프로세스를 자동화에 대 한 도구를 제공 하거나 수동으로 프로젝트 파일을 변환할 수 있습니다. 자동 도구를 사용 하 여는 항상 제안 하는 동안이 문서에서는 두 방법 모두를 설명 합니다.

## <a name="before-you-start"></a>시작 하기 전에 중...

기존 코드는 통합 API를 업데이트 하기 전에 가장 좋습니다 모든 제거 *컴파일 경고*합니다. 많은 *경고* 클래식 API에서 될 오류 통합으로 마이그레이션하면 됩니다. 컴파일러 메시지 클래식 API에서 종종 업데이트에 대 힌트를 제공 하기 때문에 시작 하기 전에 수정 하는 것이 더 쉽습니다.


# <a name="automated-updating"></a>자동 업데이트

경고를 해결 한 후 Mac 또는 Visual Studio에 대 한 Visual Studio에서 기존 iOS 프로젝트를 선택 하 고 선택 **Xamarin.iOS 통합 API로 마이그레이션** 에서 **프로젝트** 메뉴. 예:

![](updating-ios-apps-images/beta-tool1.png "프로젝트 메뉴에서 Xamarin.iOS 통합 API로 마이그레이션 선택")

자동화 된 마이그레이션을 실행 하기 전에이 경고 메시지에 동의 해야 합니다 (물론 확인 해야이 adventure을 시작 하기 전에 백업을/소스 제어 해야) 합니다.

![](updating-ios-apps-images/beta-tool2.png "자동화 된 마이그레이션을 실행 하기 전에이 경고에 동의")

에 설명 된 모든 단계를 기본적으로 자동화 하는 도구는 **수동으로 업데이트** 섹션 아래 제시 된 하 고 권장 되는 방법 통합 API를 기존 Xamarin.iOS 프로젝트를 변환 합니다.


# <a name="steps-to-update-manually"></a>수동으로 업데이트 하는 단계

다시, 경고를 해결 한 후 Xamarin.iOS 앱 새 통합 API를 사용 하도록 수동으로 업데이트 하려면 다음이 단계를 따르십시오.

## <a name="1-update-project-type--build-target"></a>1. 업데이트 프로젝트 형식 및 빌드 대상

에 프로젝트 버전을 변경 하면 **csproj** 에서 파일을 `6BC8ED88-2882-458C-8E55-DFD12B67127B` 를 `FEACFBD2-3405-455C-9665-78FE426C6842`합니다. 편집 된 **csproj** 첫 번째 항목을 대체 하는 텍스트 편집기에서 파일의 `<ProjectTypeGuids>` 요소와 같이:

![](updating-ios-apps-images/csproj.png "텍스트 편집기에 표시 된 것 처럼 ProjectTypeGuids 요소에 있는 첫 번째 항목을 바꾸는에서 csproj 파일을 편집")

변경 된 **가져오기** 포함 된 요소 `Xamarin.MonoTouch.CSharp.targets` 를 `Xamarin.iOS.CSharp.targets` 표시 된 것 처럼:

![](updating-ios-apps-images/csproj2.png "표시 된 것 처럼 Xamarin.MonoTouch.CSharp.targets Xamarin.iOS.CSharp.targets에 포함 된 Import 요소를 변경 합니다.")


## <a name="2-update-project-references"></a>2. 프로젝트 참조 업데이트

IOS 응용 프로그램 프로젝트의 확장 **참조** 노드. 처음으로 표시 됩니다는 * 끊어진- **monotouch** 참조 (해당 프로젝트 형식을 방금 변경) 이므로이 스크린샷과 유사 합니다.

![](updating-ios-apps-images/references.png "처음에 표시 됩니다 monotouch 끊어진 참조가이 스크린샷과 유사 하 게 프로젝트 유형이 변경 되었으므로")

IOS 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 **참조 편집**, 클릭는 **monotouch** 참조 하 고 빨간색 "X" 단추를 사용 하 여 삭제 합니다.

![](updating-ios-apps-images/references-delete-monotouch-sml.png "참조 편집에 대 한 iOS 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭 한 다음 monotouch 참조 클릭 하 고 빨간색을 사용 하 여 삭제 X 단추")

참조 목록 및 눈금의 끝으로 스크롤하여 이제는 **Xamarin.iOS** 어셈블리입니다.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "이제 참조 목록, Xamarin.iOS 어셈블리 눈금의 끝으로 스크롤하여")

키를 눌러 **확인** 프로젝트 참조 변경 내용을 저장 합니다.

## <a name="3-remove-monotouch-from-namespaces"></a>3. 네임 스페이스에서 MonoTouch 제거

제거는 **MonoTouch** 접두사의 네임 스페이스에서 `using` 문이나 아무 곳에 나는 응용 프로그램 이름에 된 정규화 된 않음 (예: `MonoTouch.UIKit` 방금 됩니다 `UIKit`).

## <a name="4-remap-types"></a>4. 형식을 다시 매핑

[네이티브 형식](~/cross-platform/macios/nativetypes.md) 된의 인스턴스와 같은 이전에 사용 된 일부 형식 대체를 도입 `System.Drawing.RectangleF` 와 `CoreGraphics.CGRect` (예:). 형식의 전체 목록이에서 확인할 수 있습니다는 [네이티브 형식](~/cross-platform/macios/nativetypes.md) 페이지.

## <a name="5-fix-method-overrides"></a>5. 메서드 재정의 수정 합니다.

일부 `UIKit` 메서드 서명을 새을 사용 하도록 변경 했을 [네이티브 형식](~/cross-platform/macios/nativetypes.md) (예: `nint`). 사용자 지정 하위 클래스는 이러한 메서드를 재정의 하는 경우는 더 이상 일치 하지 서명과 오류가 발생 합니다. 네이티브 형식을 사용 하 여 새 서명과 일치 시키는 하위 클래스를 변경 하 여 이러한 메서드 재정의 수정 합니다.

예를 들어 변경 `public override int NumberOfSections (UITableView tableView)` 반환할 `nint` 둘 다 반환 형식과에서 매개 변수 형식을 변경 하 고 `public override int RowsInSection (UITableView tableView, int section)` 를 `nint`합니다.

# <a name="considerations"></a>고려 사항

해당 응용 프로그램 구성 요소 또는 NuGet 패키지를 하나 이상에 의존 하는 경우 새 통합 API 클래식 API에서 기존 Xamarin.iOS 프로젝트 변환할 때 다음 사항을 고려 고려해 취해야 합니다.

## <a name="components"></a>구성 요소

응용 프로그램에 포함 된 모든 구성 요소는 통합 API를 업데이트 해야 합니다 또는 컴파일하려고 할 때 충돌이 발생 합니다. 포함 된 모든 구성 요소에 대 한 현재 버전에서 지 원하는 통합 API Xamarin 구성 요소 저장소에서 새 버전으로 교체 하 고 깨끗 한 빌드를 수행 합니다. 작성자가 아직 변환 하지 않은 모든 구성 요소는 32 비트 구성 요소 저장소에만 경고를 표시 합니다.


## <a name="nuget-support"></a>NuGet 지원

NuGet 통합 API 지원을 사용 하도록 변경을 제공한 것 동안 없었습니다 NuGet의 새로운 릴리스가 하므로 새 Api를 인식 하는 NuGet을 가져오는 방법을 평가 하는 것입니다.

그 전 까지는 구성 요소와 마찬가지로 통합 Api를 지 원하는 버전으로 프로젝트에 포함 한 모든 NuGet 패키지를 전환 하 고 깨끗 한 빌드를 나중에 실행 해야 합니다.

> [!IMPORTANT]
> **참고:** 형태로 오류가 있는 경우 _"오류 3 동일한 Xamarin.iOS 프로젝트에 'monotouch.dll'와 'Xamarin.iOS.dll'를 포함할 수 없습니다-'monotouch.dll'에서 참조 하는 동안 'Xamarin.iOS.dll' 명시적으로 참조 되 ' xxx, 버전 = 0.0.000, Culture = neutral, PublicKeyToken = null'"_ 후 통합 Api에 응용 프로그램으로 변환에 일반적으로 인해 구성 요소 또는 NuGet 패키지는 통합 API에 업데이트 되지 않은 프로젝트에서. 제거 하려면 기존 구성 요소/NuGet 통합 Api를 지 원하는 버전으로 업데이트 고 깨끗 한 빌드를 수행 해야 합니다.




# <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS 앱의 빌드를 64 비트를 사용 하도록 설정

Xamarin.iOS 모바일 응용 프로그램의 통합 API로 변환 된 경우 개발자는 여전히 응용 프로그램의 옵션 중에서 64 비트 컴퓨터에 대 한 응용 프로그램의 빌드를 사용 하도록 설정 해야 합니다. 참조 하십시오는 **활성화 64 비트 빌드의 Xamarin.iOS 앱** 의 [32/64 비트 플랫폼 고려 사항](~/cross-platform/macios/32-and-64.md#enable-64) 64 비트를 사용 하도록 설정 하면 자세한 지침에 대 한 문서를 작성 합니다.

# <a name="finishing-up"></a>마무리

자동 또는 수동 방법을 사용 하 여 통합 Api에는 클래식에서 Xamarin.iOS 응용 프로그램을 변환 하려는 여부 수동 작업을 추가 해야 하는 여러 인스턴스가 있습니다. 참조 하십시오 우리의 [통합 API에 코드를 업데이트 하기 위한 팁](~/cross-platform/macios/unified/updating-tips.md) 알려진된 문제에 대 한 문서화 하 고 문제를 해결 합니다.



## <a name="related-links"></a>관련 링크

- [통합 된 API에 코드를 업데이트 하기 위한 팁](~/cross-platform/macios/unified/updating-tips.md)
- [플랫폼 간 앱에서 네이티브 형식 사용](~/cross-platform/macios/native-types-cross-platform.md)
- [클래식 vs 통합 API 차이점](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [통합 API (비디오)로 마이그레이션](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
