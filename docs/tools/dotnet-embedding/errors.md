---
title: .NET 포함 오류
description: 이 문서에서는 .NET 포함에 의해 생성 되는 오류에 대해 설명 합니다. 오류는 코드 별로 나열 되며 문제 해결에 도움이 되는 설명이 제공 됩니다.
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
author: davidortinau
ms.author: daortin
ms.date: 04/11/2018
ms.openlocfilehash: 9d3dff0de912455a49e9f7a66aae2640a99db746
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573913"
---
# <a name="net-embedding-errors"></a>.NET 포함 오류

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: 바인딩 오류 메시지

예를 들어 매개 변수, 환경

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000"></a>

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: 예기치 않은 오류입니다 .에서 버그 보고서를 작성해 주세요.https://github.com/mono/Embeddinator-4000/issues

예기치 않은 오류 조건이 발생 했습니다. 다음과 같이 최대한 많은 정보를 사용 하 여 [문제를 해결](https://github.com/mono/Embeddinator-4000/issues) 하세요.

* 최대 세부 정보 표시를 사용 하는 전체 빌드 로그
* 오류를 재현 하는 최소 테스트 사례
* 모든 버전 정보

정확한 버전 정보를 얻는 가장 쉬운 방법은 **Xamarin Studio** 메뉴, Xamarin Studio 항목에 **대 한** **세부 정보 표시** 단추를 사용 하 여 버전 정보를 복사/붙여 넣는 것입니다 ( **정보 복사** 단추를 사용할 수 있음).

<a name="EM0001"></a>

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: 출력 디렉터리를 만들 수 없습니다.`X`

로 지정 된 디렉터리 이름이 `-o=DIR` 존재 하지 않아 만들 수 없습니다. 파일 시스템에 대 한 잘못 된 이름일 수 있습니다.

<a name="EM0002"></a>

### <a name="em0002-option-x-is-not-supported"></a>EM0002: 옵션이 `X` 지원 되지 않습니다.

이 도구는 옵션을 지원 하지 않습니다 `X` . 다른 버전의 도구에서이를 지원 하거나 `X` 옵션이이 환경에 적용 되지 않을 수 있습니다.

<a name="EM0003"></a>

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: 플랫폼이 `X` 잘못 되었습니다.

이 도구는 플랫폼을 지원 하지 않습니다 `X` . 다른 버전의 도구에서이를 지원 하거나 `X` 플랫폼이이 환경에 적용 되지 않을 수 있습니다.

<a name="EM0004"></a>

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: 대상이 `X` 잘못 되었습니다.

이 도구는 대상을 지원 하지 않습니다 `X` . 다른 버전의 도구에서이를 지원 하거나 `X` 대상이이 환경에 적용 되지 않을 수 있습니다.

<a name="EM0005"></a>

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: 컴파일 대상이 `X` 잘못 되었습니다.

이 도구는 컴파일 대상을 지원 하지 않습니다 `X` . 이 환경에서는 다른 버전의 도구가이를 지원 하거나 `X` 컴파일 대상이 적용 되지 않을 수 있습니다.

<a name="EM0006"></a>

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode 위치를 찾을 수 없습니다.

도구에서 명령을 사용 하 여 현재 선택한 Xcode 위치를 찾을 수 없습니다 `xcode-select -p` . 이 명령을 성공적으로 실행할 수 있는지 확인 하 고 올바른 Xcode 위치를 반환 하십시오.

<a name="EM0007"></a>

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: ' {sdk} '에 대 한 sdk 버전을 가져올 수 없습니다.

도구에서 명령을 사용 하 여 SDK 버전을 가져올 수 없습니다 `xcrun --show-sdk-version --sdk {sdk}` . 이 명령을 성공적으로 실행할 수 있는지 확인 하 고 SDK 버전을 반환 합니다.

<a name="EM0008"></a>

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: ' {아치} ' 아키텍처는 ' {platform} '에 사용할 수 없습니다. ' {Platform} '에 대 한 유효한 아키텍처는 ' {아키텍처} '입니다.

오류 메시지의 아키텍처가 대상 플랫폼에 적합 하지 않습니다. 옵션에 대해 유효한 아키텍처가 전달 되었는지 확인 하세요 `--abi` .

<a name="EM0009"></a>

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: `X` 현재 생성기에서이 기능을 구현 하지 않습니다.

이는 생성기의 이후 릴리스에서 수정 하려는 알려진 문제입니다. 기여를 환영 합니다.

<a name="EM0010"></a>

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: ' {file} ' 파일이 두 파일에 모두 있으므로 ' {simulatorFramework} ' 및 ' {deviceFramework} ' 프레임 워크를 병합할 수 없습니다.

도구 간에 공통 파일이 있으므로이 도구는 오류 메시지에 언급 된 프레임 워크를 병합할 수 없습니다.

이는 .NET 포함의 버그를 나타낼 수 있습니다. [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)테스트 사례를 사용 하 여에서 버그 보고서를 파일에 입력 하세요.

<a name="EM0011"></a>

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: 어셈블리가 `X` 없습니다.

도구에서 `X` 인수에 지정 된 어셈블리를 찾을 수 없습니다.

<a name="EM0012"></a>

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: 어셈블리 이름이 `X` 고유 하지 않습니다.

제공 된 어셈블리 중 두 개 이상에 동일한 내부 이름이 있으므로 런타임에 두 어셈블리를 구분할 수 없습니다.

어셈블리는 명령줄 인수에서 두 번 이상 지정 되었기 때문에 가장 가능성이 높습니다. 그러나 이름이 바뀐 어셈블리는 원래 이름을 유지 하 고 여러 복사본을 함께 사용할 수 없습니다.

<a name="EM0013"></a>

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: ' Y '에서 참조 하는 ' X ' 어셈블리를 찾을 수 없습니다.

도구가 ' Y ' 어셈블리에서 참조 하는 ' X ' 어셈블리를 찾을 수 없습니다. 참조 되는 모든 어셈블리가 바인딩할 어셈블리와 동일한 디렉터리에 있는지 확인 하세요.

<a name="EM0014"></a>

### <a name="em0014-could-not-find-product-product-min_version-is-required"></a>EM0014: {product} ({product} {min_version}이 (가) 필요 함)를 찾을 수 없습니다.

오류 메시지에 언급 된 종속성을 시스템에서 찾을 수 없습니다.

<a name="EM0015"></a>

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-min_version-is-required"></a>EM0015: {product}의 유효한 버전을 찾을 수 없습니다 (찾은 {version} 개 이상 {min_version} 개 이상 필요).

오류 메시지에 언급 된 종속성이 시스템에서 발견 되었지만 너무 오래 되었습니다. 최신 버전으로 업데이트 하세요.

<a name="EM0016"></a>

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: symlink ' {file} ' > ' {target} '을 (를) 만들 수 없습니다. 오류 {number}

오류 메시지에 언급 된 symlink를 만들 수 없습니다.

<a name="EM0026"></a>

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026가 명령줄 인수 ' A '를 구문 분석할 수 없습니다. *

명령 줄 옵션에 지정 된 구문을 `A` 도구에서 구문 분석할 수 없습니다. 설명서에서 올바른 구문을 확인 하세요.

<a name="EM0099"></a>

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: 내부 오류 *. 테스트 사례 ()를 사용 하 여 버그 보고서를 파일에 입력 하세요 https://github.com/mono/Embeddinator-4000/issues) .

이 오류 메시지는 .NET 포함의 내부 일관성 검사가 실패할 때 보고 됩니다.

이는 .NET 포함의 버그를 나타냅니다. [https://github.com/mono/Embeddinator-4000/issues](https://github.com/mono/Embeddinator-4000/issues)테스트 사례를 사용 하 여에서 버그 보고서를 파일에 입력 하세요.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: 코드 처리

<a name="EM1010"></a>

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: `T` 은 지원 되지 않으므로 형식이 생성 되지 않습니다 `X` .

이는 지원 되지 않는 기능을 사용 하기 때문에 해당 형식이 무시 됨을 나타내는 **경고** 입니다 `T` (즉, 아무 것도 생성 되지 않음). `X`

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1011"></a>

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: 형식이 네이티브에 해당 하 `T` 는 마샬링 코드가 없으므로 형식이 생성 되지 않습니다.

이는 **warning** `T` 추가 마샬링이 필요한 .net framework의 항목을 노출 하므로 형식이 무시 됨을 나타내는 경고입니다. 즉, 아무 것도 생성 되지 않습니다.

참고: 이후 버전의 도구에서는 몇 가지 제한 사항이 있지만 지원 될 수 있는 항목입니다.

<a name="EM1020"></a>

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: `C` 매개 변수 형식이 지원 되지 않기 때문에 생성자가 생성 되지 않았습니다 `T` .

이는 형식의 매개 변수가 지원 되지 않기 때문에 생성자가 무시 됨을 나타내는 **경고** 입니다 `C` . 즉, 아무 것도 생성 되지 않습니다. `T`

형식이 지원 되지 않는 이유에 대 한 자세한 정보를 제공 하는 이전 경고가 발생 합니다 `T` .

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1021"></a>

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: 생성자에는 `C` 래퍼가 생성 되지 않는 기본값이 있습니다.

이는 생성자 **warning** 의 기본 매개 변수가 `C` 추가 코드를 생성 하지 않는 경고입니다. 가장 일반적인 원인은 기존 메서드에 이미 동일한 시그니처가 있는 경우입니다. 예를 들어 .NET에서는 다음을 사용할 수 있습니다.

```csharp
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

이 경우 두 개의 생성 된 선택기만 Mono를 호출 하는 두 개의 생성 `init` 된 선택기를 만들지만 후자에 대 한 래퍼가 존재 하지 않습니다.

<a name="EM1030"></a>

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: `M` 반환 형식이 지원 되지 않으므로 메서드가 생성 되지 않습니다 `T` .

**warning** `M` 반환 형식이 지원 되지 않기 때문에 메서드는 무시 됩니다. 즉, 반환 되는 것은 `T` 아닙니다.

형식이 지원 되지 않는 이유에 대 한 자세한 정보를 제공 하는 이전 경고가 발생 합니다 `T` .

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1031"></a>

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: `M` 매개 변수 형식이 지원 되지 않으므로 메서드가 생성 되지 않습니다 `T` .

이는 형식의 매개 변수가 지원 되지 않기 때문에 메서드가 무시 됨을 나타내는 **경고** 입니다 `M` . 즉, 아무 것도 생성 되지 않습니다. `T`

형식이 지원 되지 않는 이유에 대 한 자세한 정보를 제공 하는 이전 경고가 발생 합니다 `T` .

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1032"></a>

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: 메서드에 `M` 래퍼가 생성 되지 않는 기본값이 있습니다.

이는 메서드의 **warning** 기본 매개 변수가 `M` 추가 코드를 생성 하지 않는 경고입니다. 가장 일반적인 원인은 기존 메서드에 이미 동일한 시그니처가 있는 경우입니다. 예를 들어 .NET에서는 다음을 사용할 수 있습니다.

```csharp
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

이 경우 두 개의 생성 된 선택기만 Mono를 호출 하는 두 개의 생성 `Increment` 된 선택기를 만들지만 후자에 대 한 래퍼가 존재 하지 않습니다.

<a name="EM1033"></a>

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: `M` 다른 메서드에서는 이름으로 연산자를 노출 하므로 메서드가 생성 되지 않습니다.

이는 **warning** `M` 다른 메서드가 이름으로 연산자를 노출 하기 때문에 메서드가 생성 되지 않는 경고입니다. (https://msdn.microsoft.com/library/ms229032(v=vs.110).aspx)

<a name="EM1034"></a>

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: 확장 메서드는 `M` 기본 형식에서 만들 수 없으므로 범주 내에서 생성 되지 않습니다 `T` . 일반적인 정적 메서드가 생성 되었습니다.

이는 인스턴스 형식 (예:)에 대 한 확장 메서드를 찾을 수 있다는 **경고** 입니다 `System.Int32` . 목적-C에서는 기본 형식에 대 한 범주를 만들 수 없습니다. 대신 생성기에서 일반적인 정적 메서드를 생성 합니다.

<a name="EM1040"></a>

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: `P` 매개 변수 형식이 지원 되지 않으므로 속성이 생성 되지 않습니다 `T` .

이는 노출 **warning** 된 `P` 형식이 `T` 지원 되지 않기 때문에 (즉, 아무 것도 생성 되지 않음) 속성이 무시 됨을 나타내는 경고입니다.

형식이 지원 되지 않는 이유에 대 한 자세한 정보를 제공 하는 이전 경고가 발생 합니다 `T` .

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1041"></a>

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: `T` 여러 인덱싱된 속성이 지원 되지 않으므로의 인덱싱된 속성이 생성 되지 않습니다.

이는 인덱싱된 속성이 여러 개 지원 되지 않기 때문에의 인덱싱된 속성이 무시 됨을 나타내는 **경고** 입니다 `T` (즉, 아무 것도 생성 되지 않음).

<a name="EM1050"></a>

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: 필드 `F` 형식이 지원 되지 않으므로 필드가 생성 되지 않습니다 `T` .

이는 노출 된 형식이 지원 되지 않으므로 필드가 무시 됨을 나타내는 **경고** 입니다 `F` . 즉, 아무 것도 생성 되지 않습니다. `T`

형식이 지원 되지 않는 이유에 대 한 자세한 정보를 제공 하는 이전 경고가 발생 합니다 `T` .

참고: 지원 되는 기능은 도구의 새 버전에서 개선 될 예정입니다.

<a name="EM1051"></a>

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: `E` `F` 해당 이름이 중요 한 목표-c 선택기와 충돌 하기 때문에 요소가 대신 생성 됩니다.

이는 **warning** `E` `F` 이름이 중요 한 목표-c 선택기와 충돌 하기 때문에 요소가 생성 된다는 경고입니다.

[Nsobjectprotocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) 의 선택기는 목표 c에서 중요 한 의미를 가지 며 신중 하 게 재정의 해야 합니다.

참고: 예약 된 선택기 목록은 새 버전의 도구와 함께 개선 됩니다.

<a name="EM1052"></a>

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: 요소가 `E` 생성 되지 않습니다. 이름이 같은 클래스의 다른 요소와 충돌 합니다.

이는 **warning** `E` 이름이 동일한 클래스의 다른 요소와 충돌 하므로 요소가 생성 되지 않는 경고입니다.

<a name="EM1053"></a>

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: `E` xamarin.ios 및 xamarin.ios에 대 한 대상이 지원 되지 않습니다. `framework`옵션만 지원 되는 것으로 간주 되므로 사용 해야 합니다.

이는 **warning** `E` xamarin.ios 및 xamarin.ios 사용 사례에서 대상이 지원 되지 않는 것으로 간주 된다는 경고입니다. 

정적 또는 동적 .NET 포함 라이브러리를 사용 하려면 추가 작업 단계 또는 조정 작업이 필요할 수 있으며 대부분의 사용 사례에서 피해 야 합니다.

`--target`매개 변수를 제거 하거나 대신 전달 해 보세요 `--target=framework` .

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: 코드 생성

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
