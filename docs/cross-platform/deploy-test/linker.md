---
title: 사용자 지정 링커 구성
description: 이 문서에서는 필요한 코드가 연결된 애플리케이션에서 제거되지 않았음을 명시적으로 확인하는 링커를 구성하는 데 사용할 수 있는 XML 파일을 설명합니다.
ms.prod: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7d9b7dfc12c27a195e3cb797167690cded348803
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488909"
---
# <a name="custom-linker-configuration"></a>사용자 지정 링커 구성

기본 옵션 집합으로 충분하지 않은 경우 링커에서 원하는 내용을 설명하는 XML 파일을 사용하여 연결 프로세스를 운영할 수 있습니다.

애플리케이션에서 형식, 메서드 및/또는 필드가 제거되지 않도록 링커에 추가 정의를 제공할 수 있습니다. 사용자 고유의 코드에서 사람들이 선호하는 방법은 [iOS에서 연결](~/ios/deploy-test/linker.md) 및 [Android에서 연결](~/android/deploy-test/linker.md) 가이드에 설명된 것처럼 `[Preserve]` 사용자 지정 특성을 사용하는 것입니다.
그러나 SDK 또는 제품 어셈블리의 일부 정의가 필요한 경우 링커가 필요한 항목을 제거하지 않도록 방지하는 코드를 추가하는 것보다 XML 파일을 사용하는 것이 더 좋은 해결책일 수 있습니다.

이렇게 하려면 최상위 요소 `<linker>`를 사용하여 *어셈블리* 노드를 포함하는 XML 파일을 정의합니다. 그러면 어셈블리 노드가 *형식* 노드를 포함하고, 형식 노드는 *메서드* 및 *필드* 노드를 포함합니다.

이 링커 설명 파일이 생겼으면 프로젝트에 추가하고 다음 작업을 수행합니다.

- **Android의 경우** : **빌드 작업**을 **LinkDescription**으로 설정
- **iOS의 경우** : **빌드 작업**을 **LinkDescription**으로 설정

다음 예에서는 XML 파일의 모습을 보여줍니다.

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

위의 예에서 링커는 `mscorlib.dll`(Android용 Mono와 함께 제공) 및 `My.Own.Assembly`(사용자 코드) 어셈블리에 대한 지침을 읽고 적용합니다.

`mscorlib.dll`에 대한 첫 번째 섹션은 `System.Environment` 형식이 `mono_corlib_version` 필드 및 `get_StackTrace` 메서드를 유지하도록 보장합니다.
링커가 IL에서 작동하고 C# 속성을 이해하지 못하기 때문에 getter 및/또는 setter 메서드 이름을 사용해야 합니다.

`My.Own.Assembly.dll`에 대한 두 번째 섹션은 `Foo` 형식이 모든 필드(즉, `preserve="fields"` 특성) 및 모든 생성자(즉, IL에 있는 `.ctor`이라는 이름의 모든 메서드)를 유지하도록 보장합니다. `Bar` 형식은 한 생성자(단일 문자열 매개 변수를 허용하는) 및 특정 문자열 필드 `_blah`에 대한 특정 서명(이름이 아닌)을 유지합니다.
`My.Own.Namespace` 네임스페이스는 포함된 모든 형식을 유지합니다.
마지막으로, 전체 이름(네임스페이스 포함)이 와일드카드 패턴 "My.Other\*"와 일치하는 형식은 모든 필드와 메서드를 유지합니다. 와일드카드 문자 `*`는 "type fullname" 패턴에 여러 번 포함할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [iOS에서 연결](~/ios/deploy-test/linker.md)
- [Android에서 연결](~/android/deploy-test/linker.md)
- [예제가 포함된 Linker GitHub 리포지토리](https://github.com/mono/linker)
