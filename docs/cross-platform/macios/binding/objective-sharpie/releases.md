---
title: "릴리스 기록"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 3ab0c968f17f47ea298b89e626995df1f8dc857e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="release-history"></a>릴리스 기록

## <a name="34-october-11-2017"></a>3.4 (2017 년 10 월 11)

[V3.4.0 다운로드](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Xcode 9에 대 한 지원: iOS 11, macOS 10.13, 11, tvOS 및 watchOS 4
* SIMD 및 tgmath 문제가 이제 해결
* 원격 분석 완전히 제거 되었습니다.

## <a name="33-august-3-2016"></a>3.3 (2016 년 8 월 3 일)

[V3.3.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* 10와 3 watchOS Xcode 8 Beta 4, 10 iOS, macOS 10.12, tvOS에 대 한 지원.
* Clang 마스터 빌드의 (2016-08-02) 최신 버전으로 업데이트
* [원격 분석 전송 옵션 유지](https://twitter.com/Symbiatch/status/760373403878559744) 에서 `sharpie pod bind` 를 `sharpie bind`합니다.

## <a name="32-june-14-2016"></a>3.2 (2016 년 6 월 14 일)

[V3.2.3 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* 10와 3 watchOS Xcode 8 베타 1, 10 iOS, macOS 10.12, tvOS에 대 한 지원.

## <a name="31-may-31-2016"></a>3.1 (2016 년 5 월 31 일)

[V3.1.1 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* CocoaPods 1.0에 대 한 지원
* 첫 번째 네이티브를 만들어서 CocoaPods 바인딩 안정성을 향상 `.framework` 바인딩한 다음입니다
* CocoaPods 복사 `.framework` 및 바인딩 정의에 `Binding` Xamarin.iOS 및 Xamarin.Mac 바인딩 프로젝트와의 통합을 쉽게 수행할 수 있도록 디렉터리

## <a name="30-october-5-2015"></a>3.0 (2015 년 10 월 5 일)

[V3.0.8 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Xcode 7에서 도입 된 간단한 제네릭 및 null 허용 여부, 포함 하 여 새로운 Objective C 언어 기능에 대 한 지원
* 최신 iOS 및 Mac Sdk에 대 한 지원입니다.
* Xcode 프로젝트 빌드 및 구문 분석 지원을! 전체 Xcode 프로젝트 목표 Sharpie 이제 전달할 수 있습니다 및 최적화 하는 좋은 방법지 않습니다 (예: `sharpie bind Project.xcodeproj -sdk ios`).
* [CocoaPods](https://cocoapods.org) 지원! 있습니다 수 이제 구성, 빌드 및 CocoaPods 목표 Sharpie에서 직접 바인딩 (예: `sharpie pod init ios AFNetworking && sharpie pod bind`).
* 프레임 워크가 지 원하는 경우, 가장 정확한 구문 분석 하는 프레임 워크, 프레임 워크의 구조에 의해 정의 되는 이후 발생 Clang 모듈을 설정할 프레임 워크를 바인딩하는 경우 해당 `module.modulemap`합니다.
* Xcode 프레임 워크 제품을 만들고이 프로젝트에 대 한 중간 제품 대상으로 하는 대신 해당 제품을 16 대로 Xcode 프로젝트에서 대상 프레임 워크가 아닌 자동으로 확인할 수 없는 모호성이 남아 있을 수 있습니다.

## <a name="216-march-17-2015"></a>2.1.6 (2015 년 3 월 17 일)

[V2.1.6 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.6.pkg)

* 고정된 이항 연산자 식 바인딩을:는 식의 왼쪽에서 오른쪽으로 올바르게 교환 되었습니다 (예: `1 << 0` 으로 올바르게 바인딩 되었습니다 `0 << 1`). 이 검색에 대 한 Adam Kemp를 감사!
* 문제를 해결 `NSInteger` 및 `NSUInteger` 으로 바인딩하고 `int` 및 `uint` 대신 `nint` 및 `nuint` i386;에서 `-DNS_BUILD_32_LIKE_64` 은 이제 구문 분석할 수는 Clang에 전달 `objc/NSObjCRuntime.h` i386에서 예상한 대로 작동 합니다.
* Mac OS X Sdk에 대 한 기본 아키텍처 (예: `-sdk macosx10.10`)은 이제 i386, 대신 x86_64 이므로 `-arch` 필요한 기본값을 재정의 하지 않는 한이 생략할 수 있습니다.

## <a name="210-march-15-2015"></a>2.1.0 (2015 년 3 월 15 일)

[V2.1.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [bxc #27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): 확인 `using ObjCRuntime;` 때 생성 된 `ArgumentSemantic` 사용 됩니다.
* [bxc #27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): 확인 `using System.Runtime.InteropServices;` 때 생성 된 `DllImport` 사용 됩니다.
* [bxc #27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): 기본 `DllImport` 에서 기호 로드를 `__Internal`합니다.
* [bxc #27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Objective-c 컨테이너 선언 전방 선언 건너뜁니다.
* [bxc #27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): 구체적인 인터페이스로 단일 정규화 된 프로토콜 형식을 바인딩합니다 (`id<Foo>` 으로 `Foo` 대신 `Foundation.NSObject<Foo>`).
* [bxc #28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): 바인딩 `UInt32`, `UInt64`, 및 `Int64` 으로 리터럴 `Int32` 삭제 하는 `u` 및/또는 `uL` 값에 안전 하 게 맞출 수 있는 경우 접미사 `Int32`합니다.
* [bxc #28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): 열거형 이름 매핑 원래 기본 이름으로 시작 하는 경우 해결는 `k` 접두사입니다.
* `sizeof` 인수 형식이 C# 기본 형식에 매핑되지 않는 C 식 Clang에서 평가 하 고 잘못 된 C# 생성 되지 않도록 하는 정수 리터럴로으로 바인딩.
* 블록 형식의 속성에 대 한 Objective-c 구문을 수정 (Objective C 코드 바인딩된 선언 위에 설명에 표시).
* 쇠퇴 한 형식을 원래 형식으로 바인딩합니다 (`int[]` 를 감소 시킵니다 `int*` Clang에 의미 체계 분석 하는 동안 하지만 바인딩할로 작성 된 원래 `int[]` 대신).

에 매우 큰 Dave Dunkin 다양 한 포인트가 릴리스에서 수정 된 버그 보고에 대 한 감사!

## <a name="200-march-9-2015"></a>2.0.0: 2015 년 3 월 9 일

[V2.0.0 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

목표 Sharpie 2.0은 향상된 된 Clang 기반 드라이버 및 파서와 새 NRefactory 기반 바인딩 엔진 기능 주요 릴리스 합니다. 이러한 향상 된 구성 요소에 대 한 제공 _많은_ binding 유형의 주위 특히 바인딩 향상 합니다. 개선 된 여러 기능이 적용 된 내부 목표 Sharpie 이후 릴리스에서 많은 사용자가 볼 수 기능을 생성 합니다.

목표 Sharpie 2.0 3.6.1 Clang을 기반으로 합니다.

### <a name="type-binding-improvements"></a>바인딩 형식 개선

* Objective C 블록 이제 지원 됩니다. 익명/인라인 블록 여기에 및를 통해 명명 된 블록 `typedef`합니다. 익명 블록으로 바인딩됩니다 `System.Action` 또는 `System.Func` 강력 하 게 라는 명명 된 블록 연결 되는 동안 위임 `delegate` 형식입니다.

* 익명 열거형의 경우 뒤에 나오는 즉시는 향상 된 명명 추론은 `typedef` 와 같은 기본 제공 된 정수 계열 형식으로 해결 `long` 또는 `int`합니다.

* C 포인터는 이제 C#으로 바인딩됩니다 `unsafe` 대신 포인터 `System.IntPtr`합니다. 이 인해 포인터 매개 변수를 설정 하려면에 대 한 바인딩에서 더욱 명료 하 게 `out` 또는 `ref` 매개 변수입니다. 매개 변수가 되어야 하는지 여부를 항상 유추할 수 없는 `out` 또는 `ref`이므로 포인터 보다 쉽게 감사 하는 것에 대 한 허용 하도록 바인딩을에 유지 됩니다.

* Objective C 개체에 대 한 2 순위 포인터를 매개 변수로 발생 하는 경우 위의 포인터 바인딩에 예외가입니다. 이러한 경우 규칙은 주로 사용 되 고으로 매개 변수를 바인딩할 수는 `out` (예: `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>특성 확인

목표 Sharpie에서 생성 된 바인딩 이제으로 주석을 달아야 하기가 `[Verify]` 특성입니다. 이러한 특성 표시 해야 하는 _확인_ 목표 Sharpie 원래 C/Objective-c 선언 (바인딩된 선언 위에 주석에서 제공 됩니다)를 사용 하 여 바인딩을 비교 하 여 올바른 동작인을가 하 합니다.

확인 작업이 _권장_ 모든 바인딩된 선언에 대 한는 가능성이 가장 높은 _필요한_ 선언을 주석으로 처리에 대 한는 `[Verify]` 특성입니다. 즉, 대부분의 경우에 충분 한 메타 데이터에에서 없으면 가장 적합 한 바인딩을 생성 하는 방법을 유추 하 고 원래 네이티브 소스 코드. 문서 또는 최상의 바인딩 결정을 내리려면 헤더 파일 내의 코드 주석을 참조 해야 합니다.

참조는 [특성 확인](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) 자세한 세부 정보에 대 한 설명서입니다.

### <a name="other-notable-improvements"></a>주목할 만한 기타 기능 향상

* `using` 문이 이제 바인딩된 형식을 기반으로 생성 됩니다. 예를 들어, 하는 경우는 `NSURL` 바인딩 되었습니다는 `using Foundation;` 문도 생성 되지 것입니다.

* `struct` 및 `union` 선언 이제 바인딩될를 사용 하는 `[FieldOffset]` 트릭 공용 구조체에 대 한 합니다.

* 상수 식 이니셜라이저를 사용 하 여 Enum 값이 제대로 바인딩될 이제; C#에 대 한 전체 식이 변환 됩니다.

* Variadic 메서드 및 블록 이제 바인딩됩니다.

* 통해 현재 지원 되는 프레임 워크는 `-framework` 옵션입니다. 설명서를 참조 [네이티브 프레임 워크 바인딩](http://developer.xamarin.com/guides/ios/advanced_topics/binding_objective-c/objective_sharpie/#frameworks) 내용을 확인 합니다.

* Objective C 소스 코드 됩니다 자동 감지, 전달할 필요가 사라집니다 `-ObjC` 또는 `-xobjective-c` 수동으로 Clang을 합니다.

* Clang 모듈 사용 (`@import`)는 이제 자동으로 감지 전달할 필요가 사라집니다 `-fmodules` Clang의 새 모듈 지원을 활용을 라이브러리에 대해 수동으로 Clang을 합니다.

* Xamarin 통합 API는 이제 기본 바인딩 대상입니다. 사용 하 여는 `-classic` 32 비트만 클래식 API를 대상으로 하는 옵션입니다.

### <a name="notable-bug-fixes"></a>중요 한 버그 수정

* 해결 `instancetype` Objective-c 범주에서 사용 될 때 바인딩
* Objective C 범주 완벽 하 게 이름 지정
* Objective C 프로토콜을 접두사 `I` (예: `INSCopying` 대신 `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35: 2014 년 12 월 21 일

[V1.1.35 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

사소한 버그가 수정 되었습니다.

## <a name="111-december-15-2014"></a>1.1.1: 2014 년 12 월 15 일

[V 1.1.1 다운로드](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

1.1.1 내부에서 사용 및 개발 목표 Sharpie의 초기 미리 보기 2013 년 4 월에에서 따라 Xamarin에서 1.5 년 후의 첫 번째 주요 릴리스가 했습니다. 이 릴리스는 첫 번째 안정적이 고 다양 한 새로운 Clang 백 엔드를 특징으로 네이티브 라이브러리를 사용할 수 있는 일반적으로 간주 합니다.

