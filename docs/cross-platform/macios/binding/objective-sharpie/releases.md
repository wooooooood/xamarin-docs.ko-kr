---
title: 목표 Sharpie 릴리스 기록
description: 이 문서에서는의 생성을 자동화 하는 데 사용 하는 도구, 목표 Sharpie의 릴리스 기록을 설명 C# Objective-c 코드에 대 한 바인딩을 합니다.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 03e4a5ac8906d2593cbdf3c15f6b2d1f4a2c6d19
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199651"
---
# <a name="objective-sharpie-release-history"></a>목표 Sharpie 릴리스 기록

## <a name="34-october-11-2017"></a>3.4 (2017 년 10 월 11 년)

[V3.4.0 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9에 대 한 지원: iOS 11, macOS 10.13(high, 11, tvOS 및 watchOS 4
* SIMD tgmath와 문제 해결 이제
* 원격 분석 완전히 제거 되었습니다.

## <a name="33-august-3-2016"></a>3.3 (2016 년 8 월 3 일)

[V3.3.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Xcode 8 베타 4, 10 iOS, macOS 10.12 tvOS 10 및 watchOS 3 지원 합니다.
* 최신 마스터 빌드의 (2016-08-02) Clang 업데이트
* [원격 분석 전송 옵션을 유지](https://twitter.com/Symbiatch/status/760373403878559744) 에서 `sharpie pod bind` 에 `sharpie bind`입니다.

## <a name="32-june-14-2016"></a>3.2 (2016 년 6 월 14 일)

[V3.2.3 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Xcode 8 베타 1, 10 iOS, macOS 10.12 tvOS 10 및 watchOS 3 지원 합니다.

## <a name="31-may-31-2016"></a>3.1 (2016 년 5 월 31 일)

[V3.1.1 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0에 대 한 지원
* 첫 번째 네이티브를 빌드하여 CocoaPods 바인딩 안정성 향상 `.framework` 는 바인딩한 다음
* CocoaPods를 복사 `.framework` 정의를 바인딩 및는 `Binding` Xamarin.iOS 및 Xamarin.Mac 바인딩 프로젝트와의 통합을 쉽게 수행할 수 있도록 디렉터리

## <a name="30-october-5-2015"></a>3.0 (2015 년 10 월 5 년)

[V3.0.8 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7에서 도입 된 간단한 제네릭 및 null 허용 여부를 비롯 한 새 Objective C 언어 기능에 대 한 지원
* 최신 iOS 및 Mac Sdk를 지원 합니다.
* Xcode 프로젝트를 빌드하고 구문 분석 지원은! 목표 Sharpie 이제 전체 Xcode 프로젝트에 전달할 수 있습니다 및 오른쪽으로 수행할 작업을 파악 하려면 가장 잘 않습니다 (예: `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) 지원. 있습니다 수 이제 구성, 빌드 및 CocoaPods 목표 Sharpie에서 직접 바인딩 (예: `sharpie pod init ios AFNetworking && sharpie pod bind`).
* 프레임 워크를 지 원하는 경우, 가장 정확한 구문 분석에서 프레임 워크, 프레임 워크의 구조에서 정의 되므로 결과 Clang 모듈을 사용할 프레임 워크를 바인딩하는 경우 해당 `module.modulemap`합니다.
* 제품을 빌드하는 프레임 워크는 Xcode 프로젝트에 대 한 대로 구문 분석 대상 중간 제품 대신 해당 제품 Xcode 프로젝트의 대상 프레임 워크가 아닌 자동으로 확인할 수 없는 모호성이 남아 있을 수 있습니다.

## <a name="216-march-17-2015"></a>2.1.6 (2015 년 3 월 17 년)

[V2.1.6 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 고정된 이항 연산자 식 바인딩이: 왼쪽 식의 오른쪽으로 올바르게 교환 되었습니다 (예: `1 << 0` 으로 잘못 바인딩 되었습니다 `0 << 1`). 이 확인 하는 것에 대 한 Adam Kemp에 감사 드립니다!
* 사용 하 여 문제를 해결 했습니다 `NSInteger` 하 고 `NSUInteger` 으로 바인딩되 `int` 및 `uint` 대신 `nint` 및 `nuint` i386;에서 `-DNS_BUILD_32_LIKE_64` 구문 분석할 수는 Clang 전달할 이제 `objc/NSObjCRuntime.h` i386에서 예상한 대로 작동 합니다.
* Mac OS X Sdk에 대 한 기본 아키텍처 (예: `-sdk macosx10.10`)는 이제 i386, 대신 x86_64 이므로 `-arch` 필요한 기본값을 재정의 하지 않으면 생략할 수 있습니다.

## <a name="210-march-15-2015"></a>2.1.0 (2015 년 3 월 15 년)

[V2.1.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxC#27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): 확인 `using ObjCRuntime;` 때 생성 됩니다 `ArgumentSemantic` 사용 됩니다.
* [bxC#27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): 확인 `using System.Runtime.InteropServices;` 때 생성 됩니다 `DllImport` 사용 됩니다.
* [bxC#27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): 기본 `DllImport` 에서 기호를 로드 하려면 `__Internal`합니다.
* [bxC#27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Objective-c로 컨테이너 선언 전방 선언를 건너뜁니다.
* [bxC#27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): 구체적인 인터페이스로 단일 한정자를 사용 하 여 프로토콜 형식을 바인딩합니다 (`id<Foo>` 으로 `Foo` 대신 `Foundation.NSObject<Foo>`).
* [bxC#28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): 바인딩할 `UInt32`, `UInt64`, 및 `Int64` 으로 리터럴 `Int32` 삭제 하는 `u` 및/또는 `uL` 접미사가 값에 안전 하 게 맞출 수 있는 경우 `Int32`합니다.
* [bxC#28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): 열거형 이름 매핑 원래 기본 이름으로 시작 하는 경우 수정 된 `k` 접두사입니다.
* `sizeof` C 식 인수 형식이에 매핑되지 않는 경우는 C# Clang에서 평가 하 고 잘못 된 생성 되지 않도록 하는 정수 리터럴으로 바인딩 기본 형식 C#합니다.
* Objective C 구문은 블록 형식인 속성을 수정 (Objective-c 코드에 바인딩된 선언 위에 주석 표시 됨).
* 쇠퇴 한 형식을 원래 형식으로 바인딩합니다 (`int[]` 를 감소 시킵니다 `int*` Clang에서 의미 체계 분석 하는 동안 하지만 바인딩하지 기록으로 원래 `int[]` 대신).

대부분의이 지점 릴리스에서 수정 된 버그를 보고에 대 한 Dave Dunkin에는 매우 큰 주셔서 감사!

## <a name="200-march-9-2015"></a>2.0.0: 2015 년 3 월 9

[V2.0.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

목표 Sharpie 2.0은 향상된 된 Clang 기반 드라이버 및 파서와 새 NRefactory 기반 바인딩 엔진 기능 하는 주요 릴리스입니다. 이러한 향상 된 구성 요소에 대 한 제공 _대부분_ 바인딩 형식 바인딩 특히 주위를 향상 합니다. 다른 많은 향상 된 기능이 적용 된 내부 목표 Sharpie 이후 릴리스에서 많은 사용자가 볼 수 기능 생성에 있는 합니다.

목표 Sharpie 2.0 Clang 3.6.1 기반으로 합니다.

### <a name="type-binding-improvements"></a>형식 바인딩 개선

* Objective-c 블록 이제 지원 됩니다. 여기에 익명/인라인 블록 및 통해 명명 된 블록 `typedef`합니다. 익명 블록으로 바인딩될 `System.Action` 또는 `System.Func` 대리자가 강력 하 게 라는 명명 된 블록을 바인딩될 동안 `delegate` 형식입니다.

* 방법이 바로 뒤에 나오는 익명 열거형에는 향상 된 명명 추론을 `typedef` 와 같은 기본 제공 정수 계열 형식으로 해결 `long` 또는 `int`합니다.

* C 포인터 이제 바인딩된 C# `unsafe` 대신 포인터 `System.IntPtr`합니다. 이 인해 바인딩에 대 한 포인터 매개 변수를 설정 하려면에 대 한 보다 명확한 `out` 또는 `ref` 매개 변수입니다. 항상 매개 변수를 해야 하는지 여부를 유추할 수 없는 `out` 또는 `ref`이므로 포인터 바인딩에 쉽게 감사 수 있도록 유지 됩니다.

* Objective-c 개체에 대 한 2 순위 포인터 매개 변수로 발생 하는 경우 위의 포인터 바인딩에 예외가입니다. 경우 이러한 규칙은 널리 사용 및 매개 변수를로 바인딩될 `out` (예: `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>특성 확인

목표 Sharpie에서 생성 된 바인딩 지금 사용 하 여 주석을 추가할 경우가 종종 있습니다를 `[Verify]` 특성입니다. 이러한 특성 해야 하는 나타냅니다 _확인_ (바인딩된 선언 위에 주석에서 제공 됩니다)는 원래 C/Objective-c 선언 사용 하 여 바인딩을 비교 하 여 바람직한 목표 Sharpie에가 하 합니다.

인증은 _것이 좋습니다_ 모든 바인딩된 선언에 대 한 하지만 가능성이 _필요_ 선언을 주석으로 처리에 대 한는 `[Verify]` 특성. 이것이 없기 때문에 대부분의 경우에서 충분 한 메타 데이터를 유추 가장 바인딩을 생성 하는 방법을 원래 기본 소스 코드에서입니다. 설명서 또는 최상의 바인딩 결정을 내리는 데 헤더 파일 내의 코드 주석을 참조 해야 합니다.

참조를 [특성 확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 자세한 세부 정보에 대 한 설명서입니다.

### <a name="other-notable-improvements"></a>기타 주목할 만한 향상 된 기능

* `using` 문은 이제 바인딩된 형식을 기반으로 생성 됩니다. 예를 들어 경우는 `NSURL` 이 바인딩된는 `using Foundation;` 문도 생성 되지 것입니다.

* `struct` 및 `union` 선언 이제 바인딩될를 사용 하는 `[FieldOffset]` 트릭은 공용 구조체에 대 한 합니다.

* 상수 식 이니셜라이저를 사용 하 여 열거형 값 이제 제대로 바인딩될; 전체 식으로 변환 됩니다 C#입니다.

* Variadic 메서드와 블록 이제 바인딩됩니다.

* 프레임 워크를 통해 이제 지원 되는 `-framework` 옵션입니다. 설명서를 참조 [네이티브 프레임 워크 바인딩](https://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) 대 한 자세한 내용은 합니다.

* Objective C 소스 코드는 자동으로 검색할 이제 전달할 필요성을 제거 해야 하는 `-ObjC` 또는 `-xobjective-c` 수동으로 Clang을 합니다.

* Clang 모듈 사용 (`@import`)는 이제 자동으로 검색 전달할 필요성을 제거 해야 하는 `-fmodules` Clang의 새 모듈 지원을 사용 하는 라이브러리에 대해 수동으로 Clang에.

* Xamarin 통합 API는 이제 기본 바인딩 대상; 사용 된 `-classic` 32 비트만 클래식 API를 대상으로 하는 옵션입니다.

### <a name="notable-bug-fixes"></a>중요 한 버그 수정

* 해결 `instancetype` Objective-c 범주를 사용 하는 경우 바인딩
* 완벽 하 게 Objective-c 범주 이름
* 접두사와 Objective-c 프로토콜 `I` (예: `INSCopying` 대신 `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 2014 년 12 월 21 일

[V1.1.35 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

사소한 버그가 수정 되었습니다.

## <a name="111-december-15-2014"></a>1.1.1: 2014 년 12 월 15 일

[V 1.1.1 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 내부 사용 및 개발 목표 Sharpie의 초기 미리 보기의 2013 년 4 월에에서 따라 Xamarin에서 1.5 년 후 첫 번째 버전이 이었습니다. 이 릴리스는 안정적이 고 다양 한 새 Clang 백 엔드를 특징으로 네이티브 라이브러리에 대 한 사용 가능한 일반적으로 간주 되기 위해 첫 번째입니다.

