---
title: IOS 11의 SiriKit 업데이트
description: 이 문서에서는 iOS 11에서 SiriKit로 작업 하는 방법에 대해 설명 합니다. 특히 작업과 메모를 사용 하는 방법 및 응용 프로그램에 대 한 대체 이름을 제공 하는 방법을 검사 합니다.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/07/2017
ms.openlocfilehash: 8983ac0c860dafb3a3a0e4c90bd82bdf87c4c4f8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70752391"
---
# <a name="sirikit-updates-in-ios-11"></a>IOS 11의 SiriKit 업데이트

SiriKit는 다양 한 서비스 도메인 (작업을 포함 하 고, 호출을 수행 하 고, 호출 하는 작업 포함)을 포함 하 여 iOS 10에서 도입 되었습니다. Sirikit 개념의 [sirikit 섹션과](~/ios/platform/sirikit/index.md) 앱에서 sirikit를 구현 하는 방법을 참조 하세요.

![Siri 작업 목록 데모](sirikit-images/sirikit.png)

IOS 11의 SiriKit은 다음과 같이 새로운 및 업데이트 된 의도 도메인을 추가 합니다.

- [**목록 및 메모**](#listsnotes) – 새로 만들기! 작업 및 메모를 처리할 앱에 대 한 API를 제공 합니다.
- **시각적 코드** – 새로 만들기! Siri는 QR 코드를 표시 하 여 연락처 정보를 공유 하거나 지불 트랜잭션에 참여할 수 있습니다.
- **지불-지불** 상호 작용에 대 한 검색 및 전송 의도를 추가 했습니다.
- 수렴 **예약** – 취소 및 피드백 의도를 추가 했습니다.

기타 새로운 기능은 다음과 같습니다.

- [**대체 앱 이름**](#alternativenames) – 고객이 대체 이름/발음를 제공 하 여 응용 프로그램을 대상으로 지정할 수 있도록 하는 별칭을 제공 합니다.
- **시작** 하기-백그라운드에서 체력을 시작할 수 있는 기능을 제공 합니다.

이러한 기능 중 일부는 아래에 설명 되어 있습니다. 다른 항목에 대 한 자세한 내용은 [Apple의 SiriKit 설명서](https://developer.apple.com/documentation/sirikit)를 참조 하세요.

<a name="listsnotes" />

## <a name="lists-and-notes"></a>목록 및 메모

새 목록 및 참고 도메인은 Siri 음성 요청을 통해 작업 및 메모를 처리 하는 앱에 대 한 API를 제공 합니다.

**작업**

- 제목 및 완료 상태를 포함 합니다.
- 필요에 따라 마감일 및 위치를 포함 합니다.

**참고**

- 제목과 내용 필드가 있습니다.

작업과 메모는 모두 그룹으로 구성할 수 있습니다. 이 섹션의 나머지 부분에서는 [sirikit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)를 사용 하 여이 새 도메인을 구현 하는 방법에 대해 설명 합니다.

### <a name="how-to-process-a-sirikit-request"></a>SiriKit 요청을 처리 하는 방법

다음 단계를 수행 하 여 SiriKit 요청을 처리 합니다.

1. **해결** – 매개 변수 유효성을 검사 하 고 필요한 경우 사용자의 추가 정보를 요청 합니다.
2. **확인** – 요청을 처리할 수 있는지 확인 하 고 최종 유효성 검사를 수행 합니다.
3. **핸들** – 작업을 수행 합니다 (데이터 업데이트 또는 네트워크 작업 수행).

처음 두 단계는 선택 사항 이지만 (권장 됨) 최종 단계가 필요 합니다.
[Sirikit 섹션](~/ios/platform/sirikit/index.md)에는 더 자세한 지침이 있습니다.

### <a name="resolve-and-confirm-methods"></a>해결 및 확인 방법

이러한 선택적 메서드를 사용 하 여 코드에서 유효성 검사를 수행 하거나 기본값을 선택 하거나 사용자에 게 추가 정보를 요청할 수 있습니다.

예를 `IINCreateTaskListIntent` 들어, 인터페이스의 경우 필수 `HandleCreateTaskList`메서드는입니다. Siri 상호 작용에 대 한 제어를 강화 하는 네 가지 선택적 메서드가 있습니다.

- `ResolveTitle`– 제목의 유효성을 검사 하거나, 기본 제목 (해당 하는 경우)을 설정 하거나, 데이터가 필요 하지 않다는 신호를 보냅니다.
- `ResolveTaskTitles`– 사용자가 말한 작업 목록의 유효성을 검사 합니다.
- `ResolveGroupName`– 그룹 이름의 유효성을 검사 하 고 기본 그룹을 선택 하거나 데이터가 필요 하지 않다는 신호를 보냅니다.
- `ConfirmCreateTaskList`– 코드에서 요청 된 작업을 수행할 수 있지만 해당 작업을 수행 하지 않는지 확인 합니다. `Handle*` (메서드만 데이터를 수정 해야 함)

### <a name="handle-the-intent"></a>의도를 처리 합니다.

목록과 메모 도메인에는 6 개의 의도가 있으며, 작업에는 세 가지, 메모는 3입니다.
이러한 의도를 처리 하기 위해 구현 해야 하는 메서드는 다음과 같습니다.

- 작업의 경우:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- 참고:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

각 메서드에는 전달 되는 특정 의도 형식이 있습니다. 여기에는 siri가 사용자 요청에서 구문 분석 한 모든 정보 (및 `Resolve*` `Confirm*` 메서드에서 업데이트 될 수 있음)가 포함 됩니다.
앱은 제공 된 데이터를 구문 분석 한 다음 데이터를 저장 하거나 처리 하는 몇 가지 작업을 수행 하 고 Siri가 사용자에 게 표시 하는 결과를 반환 해야 합니다.

### <a name="response-codes"></a>응답 코드

필수 `Handle*` 및 선택적 `Confirm*` 메서드는 완료 처리기에 전달 되는 개체의 값을 설정 하 여 응답 코드를 표시 합니다. 응답은 `INCreateTaskListIntentResponseCode` 열거에서 제공 됩니다.

- `Ready`– 확인 단계를 수행 하는 동안 반환 됩니다 ( `Confirm*` 즉, 메서드에서는 그렇지 `Handle*` 않고 메서드에서는).
- `InProgress`– 장기 실행 작업 (예: 네트워크/서버 작업)에 사용 됩니다.
- `Success`– 정상적으로 작동 하는 작업의 세부 정보를 사용 하 `Handle*` 여 응답 합니다 (메서드 에서만).
- `Failure`– 오류가 발생 하 여 작업을 완료할 수 없음을 의미 합니다.
- `RequiringAppLaunch`– 의도에 따라 처리할 수 없지만 응용 프로그램에서 작업을 수행할 수 있습니다.
- `Unspecified`– 사용 안 함: 사용자에 게 오류 메시지가 표시 됩니다.

이러한 방법 및 응답에 대 한 자세한 내용은 Apple의 [Sirikit 목록 및 참고 문서](https://developer.apple.com/documentation/sirikit/lists_and_notes)를 참조 하세요.

### <a name="implementing-lists-and-notes"></a>목록 및 메모 구현

작업 [노트 Sirikit 예](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample) 는 새 iOS 앱에 sirikit 지원을 추가 하는 다음 단계를 사용 하 여 만들어졌습니다.

먼저 SiriKit 지원을 추가 하려면 iOS 앱에 대해 다음 단계를 수행 합니다.

1. **Info.plist**의 틱 **sirikit** 입니다.
2. 고객에 대 한 메시지와 함께 **개인 정보 – Siri 사용 설명** 키를 **info.plist**에 추가 합니다.
3. 응용 프로그램에서 메서드를 호출 하 여 사용자에 게 siri 상호 작용을 허용할지 묻는 메시지를 표시 합니다. `INPreferences.RequestSiriAuthorization`
4. 개발자 포털의 앱 ID에 SiriKit를 추가 하 고 새 자격을 포함 하도록 프로 비전 프로필을 다시 만듭니다.

그런 다음 Siri 요청을 처리 하는 새 확장 프로젝트를 앱에 추가 합니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 추가 새 프로젝트 ...** 를 선택 합니다.
2. **IOS > 확장 > 의도 확장** 템플릿을 선택 합니다.
3. 두 개의 새 프로젝트가 추가 됩니다. 의도 및 IntentUI. UI 사용자 지정은 선택 사항이 며,이 샘플에는 **의도** 프로젝트의 코드만 포함 됩니다.

확장 프로젝트는 모든 SiriKit 요청이 처리 되는 위치입니다. 별도의 확장으로, 기본 앱과 통신 하는 방법이 자동으로 포함 되지 않습니다. 일반적으로 앱 그룹을 사용 하 여 공유 파일 저장소를 구현 하 여 해결 됩니다.

#### <a name="configure-the-intenthandler"></a>IntentHandler 구성

클래스 `IntentHandler` 는 siri 요청에 대 한 진입점입니다. 모든 의도는 요청을 처리할 수 `GetHandler` 있는 개체를 반환 하는 메서드에 전달 됩니다.

아래 코드에서는 간단한 구현을 보여 줍니다.

```csharp
[Register("IntentHandler")]
public partial class IntentHandler : INExtension, IINNotebookDomainHandling
{
  protected IntentHandler(IntPtr handle) : base(handle)
  {}
  public override NSObject GetHandler(INIntent intent)
  {
    // This is the default implementation.  If you want different objects to handle different intents,
    // you can override this and return the handler you want for that particular intent.
    return this;
  }
  // add intent handlers here!
}
```

클래스는에서 `INExtension`상속 해야 하며,이 샘플은 목록 및 메모 의도를 처리 하기 때문에도 구현 `IINNotebookDomainHandling`합니다.

> [!NOTE]
> - .Net에는 인터페이스에 대 한 순서를 지정할 수 `I`있는 규칙이 있습니다 .이는 Xamarin에서 iOS SDK의 프로토콜을 바인딩할 때 준수 하는 것입니다.
> - 또한 Xamarin은 iOS에서 형식 이름을 유지 하며, Apple은 형식이 속한 프레임 워크를 반영 하기 위해 형식 이름에 처음 두 문자를 사용 합니다.
> - 프레임 워크의 경우 형식에 `IN*` 접두사 (예: `Intents` `INExtension`)는 인터페이스가 _아닙니다_ .
> - 또한와 C# `I` 같이두개의를사용하여의인터페이스가되는프로토콜을따릅니다.`IINAddTasksIntentHandling`

#### <a name="handling-intents"></a>처리 의도

각 의도 (작업 추가, 작업 특성 설정 등)는 아래에 표시 된 것과 유사한 단일 메서드로 구현 됩니다. 메서드는 세 가지 주요 함수를 수행 해야 합니다.

1. **의도를 처리** 합니다. siri에서 구문 분석 한 데이터는 의도 유형과 `intent` 관련 된 개체에서 사용할 수 있습니다. 앱이 선택적 `Resolve*` 메서드를 사용 하 여 해당 데이터의 유효성을 검사 했을 수 있습니다.
2. **데이터 저장소 유효성 검사 및 업데이트** – 주 iOS 앱이 액세스할 수 있도록 파일 시스템에 데이터를 저장 하거나 네트워크 요청을 통해 데이터를 저장 합니다.
3. **응답 제공** – `completion` 처리기를 사용 하 여 사용자에 게 읽고 표시 하기 위해 siri에 응답을 다시 보냅니다.

```csharp
public void HandleCreateTaskList(INCreateTaskListIntent intent, Action<INCreateTaskListIntentResponse> completion)
{
  var list = TaskList.FromIntent(intent);
  // TODO: have to create the list and tasks... in your app data store
  var response = new INCreateTaskListIntentResponse(INCreateTaskListIntentResponseCode.Success, null)
  {
    CreatedTaskList = list
  };
  completion(response);
}
```

`null` 는 응답에 두 번째 매개 변수로 전달 됩니다 .이 매개 변수는 사용자 활동 매개 변수 이며, 제공 되지 않은 경우 기본값이 사용 됩니다.
IOS 앱이 `NSUserActivityTypes` **info.plist**의 키를 통해 지 원하는 경우 사용자 지정 활동 유형을 설정할 수 있습니다. 그러면 앱이 열릴 때이 경우를 처리 하 고 특정 작업 (예: 관련 보기 컨트롤러에 열고 Siri 작업에서 데이터를 로드 하는 등)을 수행할 수 있습니다.

또한이 예제에서는 `Success` 결과를 하드 코딩 하지만 실제 시나리오에서는 적절 한 오류 보고를 추가 해야 합니다.

### <a name="test-phrases"></a>테스트 구문

다음 테스트 구는 샘플 앱에서 작동 해야 합니다.

- "작업 노트에서 사과, bananas 및 배를 사용 하 여 식료품 목록 만들기"
- "작업 노트에 작업 WWDC 추가"
- "작업 노트의 학습 목록에 작업 WWDC 추가"
- "진행 메모에서 WWDC를 완료로 표시"를 표시 합니다.
- "를 사용할 경우 iphone 노트에서 iphone을 구입 하 라는 알림을 받습니다."
- "진행 메모에서 iPhone을 완료로 표시"를 표시 합니다.
- "오전 8 시 at in a note in a in a note note"를 입력 합니다.

![새 목록 만들기 예제](sirikit-images/createtasklist-sml.png) ![작업을 전체 예제로 설정](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 시뮬레이터는 이전 버전과 달리 Siri로 테스트를 지원 합니다.
>
> 실제 장치에서 테스트 하는 경우 SiriKit 지원에 대 한 앱 ID 및 프로 비전 프로필을 구성 하는 것을 잊지 마세요.

<a name="alternativenames" />

## <a name="alternative-names"></a>대체 이름

이 새로운 iOS 11 기능은 사용자가 Siri를 사용 하 여 올바르게 트리거할 수 있도록 앱에 대 한 대체 이름을 구성할 수 있음을 의미 합니다. IOS 앱 프로젝트의 **info.plist** 파일에 다음 키를 추가 합니다.

![Info.plist-대체 앱 이름 키 및 값을 표시 합니다.](sirikit-images/alternative-names.png)

대체 앱 이름이 설정 되 면 다음 구가 샘플 앱 (실제로는 작업 **노트**)에도 적용 됩니다.

- " _Monkeynotes_에서 사과, bananas 및 배를 사용 하 여 식료품 목록 만들기"
- " _Monkeytodo_에서 작업 wwdc 추가"

## <a name="troubleshooting"></a>문제 해결

샘플을 실행 하거나 사용자 응용 프로그램에 SiriKit를 추가 하는 동안 발생할 수 있는 몇 가지 오류는 다음과 같습니다.

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_목표-C 예외가 throw 되었습니다.  이름: NSInternalInconsistencyException 이유: 클래스 사용 < INPreferences 설정: 앱에서 > 0x60400082ff00에는 권한 부여가 필요 합니다. Xcode 프로젝트에서 Siri 기능을 사용 하도록 설정 했나요?_

- SiriKit는 info.plist에서 선택 됩니다 **.**
- **Info.plist** 는 **프로젝트 옵션 > 빌드 > iOS 번들 서명**으로 구성 됩니다.

  [![자격을 올바르게 설정 하는 프로젝트 옵션](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (장치 배포의 경우) 앱 ID에서 SiriKit를 사용 하도록 설정 하 고 프로 비전 프로필을 다운로드 했습니다.

## <a name="related-links"></a>관련 링크

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [에 대 한 정보 SiriKit 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-sirikitsample)
- [SiriKit (WWDC)의 새로운 기능 (비디오)](https://developer.apple.com/videos/play/wwdc2017/214/)
