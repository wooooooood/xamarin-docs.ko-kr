---
title: SiriKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/07/2017
ms.openlocfilehash: 0240dd5e381694a31ba9ebb12dd166ca0ef54750
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="sirikit"></a>SiriKit

SiriKit은 서비스 도메인 (체력 단련, 안에서, 예약 및 호출 포함)의 수를 10, iOS에서 도입 되었습니다. 참조는 [SiriKit 섹션](~/ios/platform/sirikit/index.md) SiriKit 개념과 SiriKit 응용 프로그램에서 구현 하는 방법에 대 한 합니다.

![Siri 작업 목록 데모](sirikit-images/sirikit.png)

IOS 11에서에서 SiriKit 이러한 새로운 기능과 업데이트 의도 도메인을 추가합니다.

- [**나열 하 고 정보** ](#listsnotes) – 새로운! 작업, 메모를 처리 하는 앱에 대 한 API를 제공 합니다.
- **시각적 코드** – 새로운! Siri 연락처 정보를 공유 하거나 지불 transaction에 참여시 QR 코드를 표시할 수 있습니다.
- **지불** – 지불 상호 작용에 대 한 검색 및 전송 의도 추가 합니다.
- **예약 자전거로** 추가 – 안 및 피드백 의도 취소 합니다.

기타 새 기능은 다음과 같습니다.

- [**다른 응용 프로그램 이름** ](#alternativenames) – Siri 대체 이름/발음을 제공 하 여 응용 프로그램에 알 수 있도록 도와주는 별칭을 제공 합니다.
- **시작 체력 단련** – 워크 아웃은 백그라운드에서 시작 하는 기능을 제공 합니다.

이러한 기능 중 일부는 아래 설명 합니다. 다른 항목에 대 한 자세한 내용은 참조 [Apple의 SiriKit 설명서](https://developer.apple.com/documentation/sirikit)합니다.

<a name="listsnotes" />

## <a name="lists-and-notes"></a>목록 및 참고 사항

새 목록 및 메모 도메인 처리 작업, 메모 Siri 음성 요청을 통해 앱에 대 한 API를 제공 합니다.

**작업**

- 제목 및 완료 상태에 있어야 합니다.
- 필요에 따라 마감일 및 위치를 포함 합니다.

**참고**

- 제목 및 콘텐츠 필드 있어야 합니다.

작업, 메모 모두 그룹으로 구성할 수 있습니다. 이 섹션의 나머지 부분 설명 SiriKit와이 새 도메인을 구현 하는 방법을 사용 하 여는 [TasksNotes SiriKit 예제](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)합니다.

### <a name="how-to-process-a-sirikit-request"></a>SiriKit 요청을 처리 하는 방법

다음이 단계를 수행 하 여 SiriKit 요청을 처리 합니다.

1. **해결** – 매개 변수 유효성 검사 하 고 추가 정보를 요청할 사용자 로부터 (필요한 경우).
2. **확인** – 최종 유효성 검사 및 요청을 처리할 수 있는지 확인 합니다.
3. **처리** – (데이터를 업데이트 또는 네트워크 작업 수행) 작업을 수행 합니다.

처음 두 단계는 선택적 (권장 됨)는 없지만, 및 최종 단계는 필요 합니다.
처리 하는 보다 자세한 명령에는 [SiriKit 섹션](~/ios/platform/sirikit/index.md)합니다.

### <a name="resolve-and-confirm-methods"></a>메서드를 확인 및 해결

이러한 선택적 메서드 코드에서 수행 하는 사용자 로부터 유효성 검사, 선택 기본값 또는 요청 추가 정보를 사용 합니다.

예를 들어,에 대 한는 `IINCreateTaskListIntent` 인터페이스는 필수 메서드는 `HandleCreateTaskList`합니다. Siri 상호 작용을 통해 더 많은 제어를 제공 하는 4 개의 선택적 메서드는

- `ResolveTitle` – 제목의 유효성을 검사, (필요한 경우)는 기본 제목을 설정 또는 데이터가 필요 하지 않음을 알리는 합니다.
- `ResolveTaskTitles` – 사용자가 읽을 작업 목록을 확인 합니다.
- `ResolveGroupName` – 그룹 이름의 유효성을 검사 하 고, 기본 그룹을 선택 또는 데이터가 필요 하지 않음을 알립니다.
- `ConfirmCreateTaskList` – 유효성을 검사 코드에 요청한 작업을 수행할 수 있지만 수행 하지 않습니다 (만 `Handle*` 메서드 데이터를 수정 해야) 합니다.

### <a name="handle-the-intent"></a>의도 처리 합니다.

목록 및 메모 도메인에서 6 개의 의도 작업에 대 한 3과 메모와 관련해 서 3 개가 됩니다.
이러한 의도 처리 하기 위해 구현 해야 하는 방법은 다음과 같습니다.

- 작업:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- 에 대 한 참고 사항:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

각 방법에 특정 의도 형식이, 전달 된 Siri이 사용자의 요청에서 구문 분석 하는 모든 정보를 포함 (에서 업데이트 가능 하 고는 `Resolve*` 및 `Confirm*` 메서드).
앱 제공 된 데이터를 구문 분석 한 다음 수행 일부를 저장 하는 작업 또는 데이터를 처리 하 고 Siri 말하는 및 사용자에 게 표시 되는 결과 반환 합니다.

### <a name="response-codes"></a>응답 코드

필요한 `Handle*` 및 선택적 `Confirm*` 메서드 자신의 완료 처리기에 전달 하는 개체의 값을 설정 하 여 응답 코드를 나타냅니다. 응답에서 제공 된 `INCreateTaskListIntentResponseCode` 열거형:

- `Ready` – 확인 단계에서 반환 되는 (ie.에서 `Confirm*` 메서드를에서 아니라 한 `Handle*` 메서드).
- `InProgress` -(예: 네트워크/서버 작업) 장기 실행 작업에 사용 합니다.
- `Success` – 성공적인 작업의 세부 정보로 응답 합니다. (에서만 `Handle*` 메서드).
- `Failure` – 하면 오류가 발생 하지 않으며 작업을 완료할 수 없습니다.
- `RequiringAppLaunch` – 의미를 가지는에서 처리할 수 없는 하지만 작업을 앱에서 가능 합니다.
- `Unspecified` – 사용 하지 마십시오: 사용자에 게 오류 메시지가 표시 됩니다.

이러한 방법 및 Apple의 응답에 대 한 자세한 [SiriKit 나열 하 고 설명서 참고 사항](https://developer.apple.com/documentation/sirikit/lists_and_notes)합니다.

### <a name="implementing-lists-and-notes"></a>목록 및 메모를 구현합니다.

[TasksNotes SiriKit 예제](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) 비어 있는 iOS 앱에 SiriKit 지원을 추가 하려면 다음 단계를 사용 하 여 만들어졌습니다.

첫째, SiriKit 지원을 추가할 iOS 앱에 대 한 다음이 단계를 수행 합니다.

1. 눈금 **SiriKit** 에 **Entitlements.plist**합니다.
2. 추가 **개인 정보 – Siri 사용 설명** 키를 **Info.plist**, 고객에 대 한 메시지와 함께 합니다.
3. 호출 된 `INPreferences.RequestSiriAuthorization` Siri 상호 작용을 허용 하 라는 메시지를 응용 프로그램에서 메서드.
4. SiriKit 개발자 포털에서 응용 프로그램 ID에 추가 하 고 다시 새 권한을 포함 하 여 프로 비전 프로필을 만듭니다.

Siri 요청을 처리 하도록 응용 프로그램에 새 확장 프로젝트를 추가 합니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** .
2. 선택 된 **iOS > 확장 > 의도 확장** 템플릿.
3. 두 개의 새 프로젝트를 추가할: 의도 및 IntentUI 합니다. UI를 사용자 지정 선택 사항 이므로 샘플의 코드에만 포함 되어는 **의도** 프로젝트.

확장 프로젝트가 SiriKit에 대 한 모든 요청을 처리할 수는 있습니다. 별도 확장으로 것은 아닙니다 자동으로 앱 그룹을 사용 하 여 공유 파일 저장소를 구현 하 여이 오류를 해결 일반적으로 주 응용 프로그램 –와 통신 하는 방법이 있습니다.

#### <a name="configure-the-intenthandler"></a>IntentHandler 구성

`IntentHandler` Siri 요청 – 모든 의도에 전달 됩니다에 대 한 클래스는의 진입점은 `GetHandler` 메서드를 요청을 처리할 수 있는 개체를 반환 합니다.

다음 코드에서는 간단한 구현을 보여 줍니다.

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

클래스에서 상속 해야 `INExtension`, 고도 구현 샘플 목록 처리 하려는 의도 인식 하므로, `IINNotebookDomainHandling`합니다.

> [!NOTE]
> **이름 지정에 대 한 참고:** 인터페이스에 대 한.net 대문자로 접두사를 사용 하는 규칙은 `I`, Xamarin iOS SDK에서에서 프로토콜을 바인딩할 때 준수입니다.
>
> Xamarin iOS, 형식 이름 유지 및 Apple를 사용 하 여 처음 두 문자 형식 이름에는 형식에 속해 있는 프레임 워크를 반영 합니다.
>
> 에 대 한는 `Intents` 프레임 워크 형식이 접두사가 추가 되며 `IN*` 않음 (예: `INExtension`)는 하지만 _하지_ 인터페이스입니다.
> 또한 프로토콜 (C#에서 인터페이스 될)을 두 개의 결국 뒤 `I`s와 같은 `IINAddTasksIntentHandling`합니다.

#### <a name="handling-intents"></a>의도 처리합니다.

각 의도 (추가 작업, 작업 특성 설정 등)는 아래에 나와 있는 것과 유사한 단일 메서드에 구현 됩니다. 메서드는 세 가지 주요 기능을 수행 해야 합니다.

1. **의도 처리** – Siri에서 구문 분석 데이터에 사용할 수는 `intent` 개체 의도의 유형에 따라 다릅니다. 앱 옵션을 사용 하 여 데이터를 검사 한 수 `Resolve*` 메서드.
2. **유효성을 검사 하 고 데이터 저장소 업데이트** – (기본 iOS 앱도 액세스할 수 있도록 앱 그룹을 사용) 파일 시스템 또는 네트워크 요청을 통해 데이터를 저장 합니다.
3. **응답을 제공** – 사용은 `completion` 처리기를 사용자에 게 읽기/디스플레이로 Siri에 응답을 보냅니다.

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

다음에 유의 `null` 전달 이것이 사용자 활동 매개 변수-응답에 대 한 두 번째 매개 변수로 하 고 기본값은 제공 되지 않은 경우 사용 됩니다.
IOS 앱을 통해 지원으로 사용자 지정 활동 유형을 설정할 수 있습니다는 `NSUserActivityTypes` 키 **Info.plist**합니다. 그런 다음 앱을 열면이 경우를 처리 하 고 특정 작업 (예: 관련 뷰 컨트롤러 열고 Siri 작업에서 데이터를 로드)를 수행할 수 있습니다.

이 예제에서는 또한 달라 지도록은 `Success` 결과 실제 시나리오에서 적절 한 오류 보고 추가 해야 하지만 합니다.

### <a name="test-phrases"></a>테스트 구

다음 테스트 구 샘플 응용 프로그램에서 작동 해야 합니다.

- "TasksNotes에 배, 사과 및 바나나와 식료품 목록 만들기"
- "TasksNotes에 WWDC 작업을 추가 합니다."
- "작업 WWDC TasksNotes에 학습 목록에 추가"
- "표시 TasksNotes에서 완료 된 것으로 WWDC 참석"
- "TasksNotes에 알림 메시지가 나타나면 홈 iphone 구매"
- 표시 "구입" iPhone TasksNotes에서 완료 된 것으로
- "미리 알림 홈 TasksNotes 오전 8 시에 두려면"

![새 목록 예제 만들기](sirikit-images/createtasklist-sml.png) ![전체 예제로 작업 설정](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> 11 iOS 시뮬레이터에서는 이전 버전) (달리 Siri를 사용 하 여 테스트 됩니다.
>
> 실제 장치에서 테스트를 응용 프로그램 ID 구성에 반드시 및 SiriKit에 대 한 프로 비전 프로필을 지원 합니다.

<a name="alternativenames" />

## <a name="alternative-names"></a>대체 이름

이 새로운 iOS 11 기능 Siri와 올바르게 트리거는 사용자가을 앱에 대 한 대체 이름을 구성할 수 있다는 것을 의미 합니다. 다음 키를 추가 하는 **Info.plist** iOS 앱 프로젝트의 파일:

![다른 응용 프로그램 이름 키와 값을 보여 주는 Info.plist](sirikit-images/alternative-names.png)

다른 응용 프로그램 이름이 설정 되 면 다음 구 에서도 작동 샘플 응용 프로그램에 대 한 (실제로 라는 **TasksNotes**):

- "에서 배, 사과 및 바나나와 식료품 목록을 만들 _MonkeyNotes_"
- "추가 작업에 WWDC _MonkeyTodo_"


## <a name="troubleshooting"></a>문제 해결

샘플을 실행 하거나 SiriKit 사용자 고유의 응용 프로그램에 추가 하는 동안 발생할 수 있는 몇 가지 오류:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective C 예외가 발생 했습니다.  이름: NSInternalInconsistencyException 이유: 클래스의 사용 < INPreferences: 0x60400082ff00 > 앱에서 자격 com.apple.developer.siri 필요 합니다. Xcode 프로젝트에서 Siri 기능을 사용?_

- 에 선택 되어 SiriKit **Entitlements.plist**합니다.
- **Entitlements.plist** 에 구성 된 **프로젝트 옵션 > 빌드 > iOS 번들 서명**합니다.

  [![프로젝트 옵션 자격 올바르게 설정 표시](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png)

- (배포를 위한 장치) 응용 프로그램 ID에 SiriKit 사용 하도록 설정 하 고 다운로드 프로비저닝 프로필 키를 누릅니다.


## <a name="related-links"></a>관련 링크

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit 샘플](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [새로 만들기에 SiriKit (WWDC) (비디오)는 무엇입니까](https://developer.apple.com/videos/play/wwdc2017/214/)
