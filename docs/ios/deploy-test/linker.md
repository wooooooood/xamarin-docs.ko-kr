---
title: "iOS에서 연결"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/24/2017
ms.openlocfilehash: 2ee5da1b2c5d4c8fbf405c7f28ed280a3286a025
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="linking-on-ios"></a>iOS에서 연결

응용 프로그램을 빌드할 때 Mac용 Visual Studio 또는 Visual Studio는 관리 코드에 대한 링커를 포함하고 있는 **mtouch**라는 도구를 호출합니다. 이 도구는 응용 프로그램에서 사용되지 않는 기능을 클래스 라이브러리에서 제거하는 데 사용됩니다. 목표는 꼭 필요한 비트만 제공하여 응용 프로그램의 크기를 줄이는 것입니다.

링커는 정적 분석을 사용하여 응용 프로그램에서 따를 수 있는 다른 코드 경로를 결정합니다. 검색 가능한 항목이 제거되지 않도록 각 어셈블리의 세부 정보를 모두 살펴보아야 하기 때문에 약간 무거운 작업입니다. 시뮬레이터 빌드에서는 디버깅하는 동안 빌드 시간을 단축하기 위해 기본적으로 설정되지 않습니다. 그러나 더 작은 응용 프로그램을 생성하므로 AOT 컴파일 및 장치에 업로드하는 시간을 줄일 수 있으며, 모든 *장치(릴리스) 빌드*가 기본적으로 링커를 사용합니다.

링커는 정적 도구이므로 리플렉션을 통해 호출되거나 동적으로 인스턴스화되는 포함 형식 및 메서드에 대해 표시할 수 없습니다. 이 제한을 해결하는 여러 가지 옵션이 있습니다.

<a name="Linker_Behavior" />

## <a name="linker-behavior"></a>링커 동작

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**프로젝트 옵션**의 링커 동작 드롭다운을 통해 연결 프로세스를 사용자 지정할 수 있습니다. 이 드롭다운에 액세스하려면 아래 그림처럼 iOS 프로젝트를 두 번 클릭하고 **iOS 빌드 > 링커 옵션**을 찾습니다.

[![](linker-images/image1.png "링커 옵션")](linker-images/image1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio의 **프로젝트 속성**에서 찾을 수 있는 링커 동작 드롭다운을 통해 연결 프로세스를 사용자 지정할 수 있습니다.

다음을 수행합니다.

1. **솔루션 탐색기**에서 **프로젝트 이름**을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다.

    ![](linker-images/linking01w.png "솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 속성 선택")
2. **프로젝트 속성**에서 **IOS 빌드**를 선택합니다.

    ![](linker-images/linking02w.png "IOS 빌드 선택")
3. 연결 옵션을 변경하려면 아래 지침을 따릅니다.

-----

제공되는 세 가지 기본 옵션은 아래 설명과 같습니다.


### <a name="dont-link"></a>연결하지 않음

연결을 사용하지 않으면 어떤 어셈블리도 수정되지 않습니다. IDE의 대상이 iOS 시뮬레이터인 경우 성능상의 이유로 이것이 기본 설정입니다. 장치 빌드의 경우 링커에 응용 프로그램 실행을 방해하는 버그가 있는 경우에만 이 옵션을 해결 방법으로 사용해야 합니다. 응용 프로그램이 *-nolink*에서만 작동하는 경우 [버그 보고서](http://bugzilla.xamarin.com)를 제출하세요.

명령줄 도구 mtouch를 사용하는 경우 이것은 *-nolink* 옵션에 해당합니다.

<a name="Link_SDK_assemblies_only" />

### <a name="link-sdk-assemblies-only"></a>SDK 어셈블리만 연결

이 모드에서는 링커가 어셈블리를 건들지 않으며, 응용 프로그램에서 사용하지 않는 모든 항목을 제거하여 SDK 어셈블리(즉, Xamarin.iOS와 함께 제공되는 항목) 크기를 줄입니다. IDE의 대상이 iOS 장치인 경우 이것이 기본 설정입니다.

이 옵션은 코드를 변경할 필요가 없으므로 가장 간단합니다. 모두 연결하는 옵션과 다른 점으로, 이 옵션에서는 링커가 몇 가지 최적화를 수행할 수 없으므로 모두 연결하기 위해 필요한 작업과 최종 응용 프로그램 크기는 서로 반비례 관계에 있습니다.

명령줄 도구 mtouch를 사용하는 경우 이것은 *-linksdk* 옵션에 해당합니다.

<a name="Link_all_assemblies" />

### <a name="link-all-assemblies"></a>모든 어셈블리 연결

모든 항목을 연결할 때 링커는 모든 최적화 기능을 사용하여 응용 프로그램을 최대한 작게 만들 수 있습니다. 링커가 사용자 코드를 수정하게 되고, 코드가 기능을 링커의 정적 분석에서 감지할 수 없는 방식으로 사용할 경우 중단될 수 있습니다. 이 경우(예: 웹 서비스, 리플렉션 또는 serialization) 모두 연결하려면 응용 프로그램에서 일부 조정이 필요할 수 있습니다.

명령줄 도구 **mtouch**를 사용하는 경우 이것은 *-linkall* 옵션에 해당합니다.

<a name="Controlling_the_Linker" />

## <a name="controlling-the-linker"></a>링커 제어

링커를 사용하는 경우 동적으로 호출한, 심지어 간접적으로 호출한 코드까지 제거되는 경우가 가끔 있습니다. 이러한 상황을 해결하기 위해 링커는 동작을 세밀하게 제어할 수 있는 몇 가지 기능과 옵션을 제공합니다.

<a name="Preserving_Code" />

### <a name="preserving-code"></a>코드 유지

링커를 사용하는 경우 System.Reflection.MemberInfo.Invoke를 사용하여, 또는 `[Export]` 특성을 사용하여 메서드를 Objective-C로 내보낸 후 선택기를 수동으로 호출하여 동적으로 호출한 코드가 가끔 삭제될 수 있습니다.

이러한 경우 클래스 수준 또는 멤버 수준에서 `[Xamarin.iOS.Foundation.Preserve]` 특성을 적용하여 전체 클래스를 사용하거나 개별 멤버를 보존하는 방안을 고려하도록 링커에 지시할 수 있습니다. 응용 프로그램이 정적으로 연결하지 않은 모든 멤버는 제거됩니다. 따라서 이 특성은 정적으로 참조되지 않지만 여전히 응용 프로그램에 필요한 멤버를 표시하는 데 사용됩니다.

예를 들어 형식을 동적으로 인스턴스화하는 경우 형식의 기본 생성자를 유지하는 것이 좋습니다. XML serialization을 사용하는 경우 형식의 속성을 유지하는 것이 좋습니다.

같은 형식의 모든 멤버 또는 형식 자체에 이 특성을 적용할 수 있습니다. 전체 형식을 유지하려는 경우 해당 형식에 `[Preserve
(AllMembers = true)]` 구문을 사용하면 됩니다.

특정 멤버를 유지하려는 경우가 있습니다(포함 형식이 유지된 경우에만 해당). 이 경우 `[Preserve (Conditional=true)]`를 사용합니다.

Xamarin 라이브러리에 대한 종속성을 유지하지 않으려는 경우(예를 들어 플랫폼 간에 이식 가능한 클래스 라이브러리(PCL)를 빌드하는 경우)에도 이 특성을 사용할 수 있습니다.

이렇게 하려면 PreserveAttribute 클래스를 선언하고 코드에서 다음과 같이 사용해야 합니다.

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

링커는 이 특성을 형식 이름 기준으로 보기 때문에 어떤 네임스페이스에 정의되는지는 전혀 중요하지 않습니다.

 <a name="Skipping_Assemblies" />

### <a name="skipping-assemblies"></a>어셈블리 건너뛰기

링커 프로세스에서 제외할 어셈블리를 지정하고, 다른 어셈블리는 정상적으로 연결되도록 허용할 수 있습니다. 이는 일부 어셈블리에서 `[Preserve]`를 사용할 수 없거나(예: 타사 코드) 버그에 대한 임시 방편으로 사용할 수 없는 경우에 도움이 됩니다.

명령줄 도구 mtouch를 사용하는 경우 이것은 `--linkskip` 옵션에 해당합니다.

**모든 어셈블리 연결** 옵션을 사용하는 경우 링커에 전체 어셈블리를 건너뛰라고 지시하려면 최상위 어셈블리의 **추가 mtouch 인수** 옵션에 다음을 추가합니다.

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

링커가 여러 어셈블리를 건너뛰게 하려면 여러 `linkskip` 인수를 포함합니다.

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

이 옵션을 사용하는 사용자 인터페이스는 없지만 Mac용 Visual Studio 프로젝트 옵션 대화 상자 또는 Visual Studio 프로젝트 속성 창의 **추가 mtouch 인수** 텍스트 필드에서 제공할 수 있습니다. (예: *--linkskip=mscorlib*는 mscorlib.dll을 연결하지 않지만 솔루션의 다른 어셈블리를 연결합니다).

<a name="Disabling_Link_Away" />

### <a name="disabling-link-away"></a>"연결 제거" 해제

링커는 장치에서 거의 사용되지 않은 코드, 즉 지원되지 않거나 허용되지 않는 코드를 제거합니다. 드물기는 하지만, 응용 프로그램 또는 라이브러리가 이 코드에(작동 여부에 관계없이) 의존하는 경우가 있습니다. Xamarin.iOS 5.0.1부터 이 최적화를 건너뛰라고 링커에 지시할 수 있습니다.

명령줄 도구 mtouch를 사용하는 경우 이것은 *-nolinkaway* 옵션에 해당합니다.

이 옵션을 사용하는 사용자 인터페이스는 없지만 Mac용 Visual Studio 프로젝트 옵션 대화 상자 또는 Visual Studio 프로젝트 속성 창의 **추가 mtouch 인수** 텍스트 필드에서 제공할 수 있습니다. (예: *--nolinkaway*는 추가 코드(약 100kb)를 제거하지 않습니다).

### <a name="marking-your-assembly-as-linker-ready"></a>어셈블리를 링커 준비 완료로 표시

사용자는 SDK 어셈블리만 연결하고 코드는 연결하지 않도록 선택할 수 있습니다.  이는 Xamarin의 코어 SDK에 포함되지 않은 타사 라이브러리는 연결되지 않는다는 의미입니다.

이것이 일반적인데, `[Preserve]` 특성을 코드에 수동으로 추가하려는 사용자는 없기 때문입니다.  타사 라이브러리가 연결되지 않는다는 부작용이 있는데, 타사 라이브러리가 링커에 호의적인지 알 수 없으므로 일반적으로 볼 때 이것은 좋은 기본값입니다.

프로젝트에 라이브러리가 있거나 재사용 가능한 라이브러리를 개발하는 개발자이고 링커가 어셈블리를 연결 가능으로 처리하게 하려면 다음과 같이 어셈블리 수준 특성 [`LinkerSafe`](https://developer.xamarin.com/api/type/Foundation.LinkerSafeAttribute/)를 추가하기만 하면 됩니다.

```csharp
[assembly:LinkerSafe]
```

라이브러리가 실제로 이에 대한 Xamarin 라이브러리를 참조할 필요는 없습니다.  예를 들어 다른 플랫폼에서 실행될 이식 가능한 클래스 라이브러리를 빌드하는 경우 여전히 `LinkerSafe` 특성을 사용할 수 있습니다.
Xamarin 링커는 실제 형식이 아닌 이름을 기준으로 `LinkerSafe` 특성을 조회합니다.  이 코드를 작성하면 역시 작동한다는 의미입니다.

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {
    public LinkerSafeAttribute : System.base {}
}
```

## <a name="custom-linker-configuration"></a>사용자 지정 링커 구성

[링커 구성 파일을 만드는 방법에 대한 지침](~/cross-platform/deploy-test/linker.md)을 따릅니다.


## <a name="related-links"></a>관련 링크

- [사용자 지정 링커 구성](~/cross-platform/deploy-test/linker.md)
- [Mac에서 연결](~/mac/deploy-test/linker.md)
- [Android에서 연결](~/android/deploy-test/linker.md)
