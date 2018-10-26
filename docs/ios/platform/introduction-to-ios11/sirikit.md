---
title: IOS 11의에서 SiriKit 업데이트
description: 이 문서에는 iOS 11의에서 SiriKit을 사용 하는 방법을 설명 합니다. 특히, 작업 및 정보를 사용 하는 방법 및 응용 프로그램에 대 한 대체 이름을 제공 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8F75300B-B591-42ED-9D17-001992A5C381
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/07/2017
ms.openlocfilehash: 7e895dc2865880ec2789a40f8cdf047a20f8693b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111040"
---
# <a name="sirikit-updates-in-ios-11"></a>IOS 11의에서 SiriKit 업데이트

SiriKit은 service 도메인 (달리기, 승객, 예약 및 호출 포함)의 수를 사용 하 여 iOS 10에서에서 도입 되었습니다. 참조 된 [SiriKit 섹션](~/ios/platform/sirikit/index.md) SiriKit 개념 및 앱에서 SiriKit 구현 하는 방법에 대 한 합니다.

![Siri 작업 목록 데모](sirikit-images/sirikit.png)

SiriKit iOS 11의에서 새롭고 업데이트 된 이러한 의도 도메인을 추가합니다.

- [**나열 하 고 메모** ](#listsnotes) -새! 작업 및 메모를 처리 하는 데 앱에 대 한 API를 제공 합니다.
- **시각적 코드** -새! Siri 연락처 정보를 공유 하거나 지불 트랜잭션을 참여 QR 코드를 표시할 수 있습니다.
- **지불** – 결제 상호 작용에 대 한 검색 및 전송 의도 추가 합니다.
- **예약 재정의** 추가 – 승객 및 피드백 의도 취소 합니다.

기타 새로운 기능은 다음과 같습니다.

- [**대체 앱 이름** ](#alternativenames) -Siri 대체 이름/발음을 제공 하 여 앱을 대상으로 알 수 있도록 도와주는 별칭을 제공 합니다.
- **달리기 시작** – 만납니다 백그라운드에서 시작 하는 기능을 제공 합니다.

이러한 기능 중 일부는 아래 설명 되어 있습니다. 다른 항목에 대 한 자세한 내용은 참조 [Apple SiriKit 설명서](https://developer.apple.com/documentation/sirikit)합니다.

<a name="listsnotes" />

## <a name="lists-and-notes"></a>목록 및 정보

새 목록 및 메모 도메인 작업과 Siri 음성 요청을 통해 정보를 처리 하는 데 앱에 대 한 API를 제공 합니다.

**작업**

- 제목 및 완료 상태를 갖습니다.
- 필요에 따라 마감일 및 위치를 포함 합니다.

**참고**

- 제목 및 콘텐츠 필드 있어야 합니다.

작업 및 메모를 모두 그룹으로 구성할 수 있습니다. 이 섹션의 나머지 부분 SiriKit 사용 하 여이 새 도메인을 구현 하는 방법에 설명 합니다를 사용 하는 [TasksNotes SiriKit 예제](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)합니다.

### <a name="how-to-process-a-sirikit-request"></a>SiriKit 요청을 처리 하는 방법

다음이 단계를 수행 하 여 SiriKit 요청을 처리 합니다.

1. **해결** – 매개 변수 유효성 검사 하 고 추가 정보를 요청할 사용자 로부터 (필요한 경우).
2. **확인** – 최종 유효성 검사 및 확인 요청을 처리할 수 있습니다.
3. **처리** -(데이터 업데이트 또는 네트워크 작업 수행) 작업을 수행 합니다.

처음 두 단계는 선택적 (권장) 하지만, 마지막 단계 이며 반드시 지정 해야 합니다.
에 대 한 자세한 지침이 있습니다 합니다 [SiriKit 섹션](~/ios/platform/sirikit/index.md)합니다.

### <a name="resolve-and-confirm-methods"></a>해결 방법을 확인 하 고

이러한 선택적 메서드 코드에서 사용자의 유효성 검사, 선택 기본값 또는 요청에 대 한 자세한 정보를 수행 하를 사용 합니다.

예를 들어에 대 한 합니다 `IINCreateTaskListIntent` 인터페이스는 필수 메서드는 `HandleCreateTaskList`합니다. Siri 상호 작용을 통해 더 많은 제어를 제공 하는 네 개의 선택적 메서드 가지

- `ResolveTitle` – 제목의 유효성을 검사, 기본 제목 (해당 하는 경우)를 설정 또는 데이터가 필요 하지 않음을 알립니다.
- `ResolveTaskTitles` – 사용자가 음성으로 변환 하는 작업 목록을 확인 합니다.
- `ResolveGroupName` – 그룹 이름의 유효성을 검사 하 고는 기본 그룹을 선택 또는 데이터가 필요 하지 않음을 알립니다.
- `ConfirmCreateTaskList` –의 유효성을 검사는 코드에 요청한 작업을 수행할 수 있지만를 수행 하지 않습니다 하는 (만 `Handle*` 메서드 데이터를 수정 해야) 합니다.

### <a name="handle-the-intent"></a>의도 처리 합니다.

목록 및 메모에 6 인 텐트와 작업에 대 한 세 정보에 대 한 세 가지 있습니다.
이러한 의도 처리 하기 위해 구현 해야 하는 방법은 다음과 같습니다.

- 작업:
  - `HandleAddTasks`
  - `HandleCreateTaskList`
  - `HandleSetTaskAttribute`
- 정보:
  - `HandleCreateNote`
  - `HandleAppendToNote`
  - `HandleSearchForNotebookItems`

각 메서드가 특정 의도 전달 되는 형식, Siri 사용자의 요청에서 구문 분석에 모든 정보가 포함 된 (업데이트 수 및 합니다 `Resolve*` 및 `Confirm*` 메서드).
앱 제공 된 데이터를 구문 분석 한 다음 일부를 저장 하는 작업을 수행할지 그렇지 않으면 프로세스 데이터를 고 Siri 강연 및 사용자에 게 표시 되는 결과 반환 해야 합니다.

### <a name="response-codes"></a>응답 코드

필수 `Handle*` 및 선택적 `Confirm*` 메서드는 완료 처리기에 전달 하는 개체의 값을 설정 하 여 응답 코드를 나타냅니다. 응답에서 제공 되는 `INCreateTaskListIntentResponseCode` 열거형:

- `Ready` – 확인 단계에서 반환 되는 (ie.에서 `Confirm*` 메서드를 아닌를 `Handle*` 메서드).
- `InProgress` -장기 실행 작업 (예: 네트워크/서버 작업)을 사용합니다.
- `Success` – 성공 작업의 세부 정보를 사용 하 여 응답 (에서만 `Handle*` 메서드).
- `Failure` – 의미 오류가 발생 하는 작업을 완료할 수 없습니다.
- `RequiringAppLaunch` – 의도에서 처리할 수 없습니다 하지만 작업 앱에서 가능 합니다.
- `Unspecified` – 넣지 않습니다: 사용자에 게 오류 메시지가 표시 됩니다.

이러한 메서드 및 apple의 응답에 자세히 알아보려면 [SiriKit 나열 하 고 설명서 참고 사항](https://developer.apple.com/documentation/sirikit/lists_and_notes)합니다.

### <a name="implementing-lists-and-notes"></a>목록 및 메모를 구현합니다.

합니다 [TasksNotes SiriKit 예제](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/) SiriKit 지원을 빈 iOS 앱을 추가 하려면 다음 단계를 사용 하 여 만든 합니다.

먼저에 SiriKit 지원을 추가 하려면 iOS 앱에 대 한 다음이 단계를 수행 합니다.

1. 눈금 **SiriKit** 에 **Entitlements.plist**합니다.
2. 추가 된 **개인 정보-Siri 사용 설명** 키를 **Info.plist**, 고객에 게 메시지와 함께 합니다.
3. 호출 된 `INPreferences.RequestSiriAuthorization` Siri 상호 작용을 허용 하 라는 메시지를 앱에서 메서드.
4. SiriKit 개발자 포털에서 앱 ID에 추가 하 고 다시 새 자격을 포함 하려면 프로 비전 프로필을 만듭니다.

Siri 요청을 처리할 앱에 새 확장 프로젝트를 추가 합니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** .
2. 선택 된 **iOS > 확장 > 인 텐트 확장** 템플릿.
3. 새 프로젝트가 추가 됩니다: 의도 및 IntentUI 합니다. UI를 사용자 지정 선택 사항 이므로 샘플 코드에 포함 된 **의도** 프로젝트입니다.

확장 프로젝트는 모든 SiriKit 요청 처리할입니다. 별도 확장으로 자동으로 없는 앱 그룹을 사용 하 여 공유 파일 저장소를 구현 하 여이 오류를 해결 일반적으로 기본 앱 –와 통신 하는 방식으로 합니다.

#### <a name="configure-the-intenthandler"></a>IntentHandler 구성

합니다 `IntentHandler` Siri 요청 – 모든 의도에 전달 됩니다에 대 한 클래스는 진입점을 `GetHandler` 요청을 처리할 수 있는 개체를 반환 하는 메서드.

아래 코드에 간단한 구현을 보여 줍니다.

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

클래스에서 상속 해야 합니다 `INExtension`, 고도 구현 샘플 목록 처리 하려는 의도 정보 때문에 `IINNotebookDomainHandling`입니다.

> [!NOTE]
> - .NET 인터페이스에 대 한 대문자를 사용 하 여 접두사를 사용 하는 규칙은 `I`, Xamarin iOS SDK에서에서 프로토콜을 바인딩하는 경우 준수입니다.
> - Xamarin iOS에서 형식 이름을 유지 하 고 Apple 형식 이름에는 형식이 속해 있는 프레임 워크를 반영 하도록 처음 두 문자를 사용 합니다.
> - 에 대 한 합니다 `Intents` 프레임 워크 형식 접두사로 `IN*` (예: `INExtension`) 이러한 되지만 _되지_ 인터페이스입니다.
> - 해당 프로토콜을 따르는 것 (인터페이스를 될 하는 C#) 두 개를 사용 하 여 결국 `I`s와 같은 `IINAddTasksIntentHandling`.

#### <a name="handling-intents"></a>의도 처리합니다.

각 의도 (추가 작업, 작업 특성 설정 등)는 아래에 표시 된 것과 비슷한 단일 메서드에 구현 됩니다. 메서드는 세 가지 주요 기능이 수행 해야 합니다.

1. **의도 처리** -Siri에서 구문 분석 하는 데이터에 사용할 수는 `intent` 개체 의도의 유형에 따라 다릅니다. 앱 옵션을 사용 하 여 데이터를 검사 한 있습니다 `Resolve*` 메서드.
2. **유효성 검사 및 데이터 저장소를 업데이트** – 네트워크 요청을 통해 데이터를 파일 시스템 (기본 iOS 앱도 액세스할 수 있도록 앱 그룹 사용)를 저장 합니다.
3. **응답 제공** – 사용을 `completion` 처리기 Siri 사용자에 게 읽기/표시를 다시 응답을 보내도록 하려면:

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

`null` 전달 되는 응답에는 두 번째 매개 변수로 사용자 활동 매개 변수 이며 지정 하지 않으면 기본값이 사용 됩니다.
으로 iOS 앱을 통해 지 원하는 사용자 지정 활동 유형을 설정할 수 있습니다 합니다 `NSUserActivityTypes` 키 **Info.plist**합니다. 그런 다음 앱을 열면 이러한 경우를 처리 하 고 특정 작업 (예: 관련 뷰 컨트롤러를 열고 Siri 작업에서 데이터를 로드)를 수행할 수 있습니다.

예제도 달라 지도록 합니다 `Success` 결과 실제 시나리오에서 적절 한 오류 보고 추가 해야 하지만 합니다.

### <a name="test-phrases"></a>테스트 구

다음 테스트 구 샘플 앱에서 작동 해야 합니다.

- "TasksNotes에서 사과, bananas, 배와 식료품 목록의 확인 합니다."
- "TasksNotes에서 WWDC 작업을 추가 합니다."
- "작업 WWDC TasksNotes 학습 목록에 추가"
- "표시 TasksNotes에서 완료 된 것으로 WWDC 참가"
- "TasksNotes에서 알림 메시지가 나타나면 홈 iphone을 구입 하"
- 표시 "구입" iPhone TasksNotes에서 완료 된 것으로
- "미리 알림 관리 TasksNotes에서 오전 8 시 홈를 그대로 둡니다."

![만들 새 목록 예제](sirikit-images/createtasklist-sml.png) ![전체 예제 집합 작업](sirikit-images/settaskattribute-sml.png)

> [!NOTE]
> IOS 11 시뮬레이터 지원 (이전 버전)와 달리 Siri를 사용 하 여 테스트 합니다.
>
> 실제 장치에서 테스트 하는 경우 앱 ID를 구성 해야 하 고 프로 비전 프로필 SiriKit 지원 합니다.

<a name="alternativenames" />

## <a name="alternative-names"></a>대체 이름

이 새로운 iOS 11 기능 Siri를 사용 하 여 올바르게 트리거할 수 있도록 앱에 대 한 대체 이름을 구성할 수 있는 것을 의미 합니다. 다음 키를 추가 합니다 **Info.plist** iOS 앱 프로젝트의 파일:

![Info.plist 대체 앱 이름 키 및 값을 표시](sirikit-images/alternative-names.png)

대체 앱 이름 집합을 사용 하 여 다음 구 에서도 작동 샘플 앱 (실제로 이라는 **TasksNotes**):

- "사과, bananas, 및에서 배 식료품 목록을 확인 하십시오 _MonkeyNotes_"
- "추가 작업에 WWDC _MonkeyTodo_"


## <a name="troubleshooting"></a>문제 해결

샘플을 실행 하거나 사용자 고유의 응용 프로그램에 SiriKit을 추가 하는 동안 발생할 수 있는 일부 오류:

### <a name="nsinternalinconsistencyexception"></a>NSInternalInconsistencyException

_Objective C 예외를 throw 됩니다.  이름: NSInternalInconsistencyException 이유: 클래스의 사용 하 여 < INPreferences: 0x60400082ff00 > 앱에서 자격 com.apple.developer.siri 필요 합니다. Xcode 프로젝트에서 Siri 기능을 사용?_

- SiriKit에서 선택 됩니다 **Entitlements.plist**합니다.
- **Entitlements.plist** 에 구성 된 **프로젝트 옵션 > 빌드 > iOS 번들 서명**합니다.

  [![프로젝트 자격 올바르게 설정 표시 옵션](sirikit-images/set-entitlements-sml.png)](sirikit-images/set-entitlements.png#lightbox)

- (장치 배포용) 앱 ID에 사용 하도록 설정 하는 SiriKit 및 프로 비전 프로필 다운로드 합니다.


## <a name="related-links"></a>관련 링크

- [SiriKit (Apple)](https://developer.apple.com/documentation/sirikit)
- [TasksNotes SiriKit 샘플](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
- [새로운 기능에 SiriKit (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/214/)
