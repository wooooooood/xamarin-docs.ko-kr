---
title: Xamarin.ios의 CallKit
description: 이 문서에서는 iOS 10에서 Apple에서 릴리스된 새 CallKit API와 Xamarin.ios VOIP 앱에서이 API를 구현 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: 8f8b92e48578c08e491f92bcc7e2a9add67ee0cd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032607"
---
# <a name="callkit-in-xamarinios"></a>Xamarin.ios의 CallKit

IOS 10의 새로운 CallKit API는 VOIP 앱이 iPhone UI와 통합 되 게 하 고 최종 사용자에 게 친숙 한 인터페이스와 경험을 제공할 수 있는 방법을 제공 합니다. 이 API를 사용 하면 사용자가 iOS 장치의 잠금 화면에서 VOIP 호출을 확인 하 고 상호 작용 하 고 Phone 앱의 **즐겨찾기** 및 **최근** 보기를 사용 하 여 연락처를 관리할 수 있습니다.

## <a name="about-callkit"></a>CallKit 정보

Apple에 따라 CallKit는 타사 VOIP (Voice Over IP) 앱을 iOS 10의 자사 환경으로 승격 하는 새로운 프레임 워크입니다. CallKit API를 사용 하 여 VOIP 앱을 iPhone UI와 통합 하 고 최종 사용자에 게 친숙 한 인터페이스 및 환경을 제공할 수 있습니다. 기본 제공 전화 앱과 마찬가지로 사용자는 iOS 장치의 잠금 화면에서 VOIP 통화를 보고 상호 작용 하 고 휴대폰 앱의 **즐겨찾기** 및 **최근** 보기를 사용 하 여 연락처를 관리할 수 있습니다.

또한 CallKit API는 전화 번호를 이름 (호출자 ID)과 연결 하는 앱 확장을 만들 수 있는 기능을 제공 하거나, 번호를 차단 해야 할 경우 시스템에 알릴 수 있습니다 (호출 차단).

### <a name="the-existing-voip-app-experience"></a>기존 VOIP 앱 환경

새 CallKit API 및 해당 기능에 대해 설명 하기 전에 MonkeyCall 이라는 가상의 VOIP 앱을 사용 하 여 iOS 9에서 타사 VOIP 앱을 사용 하 여 현재 사용자 환경을 살펴보세요. MonkeyCall은 사용자가 기존 iOS Api를 사용 하 여 VOIP 호출을 보내고 받을 수 있도록 하는 간단한 앱입니다.

현재 사용자가 MonkeyCall에서 들어오는 호출을 받고 해당 iPhone이 잠기면 잠금 화면에서 수신 된 알림을 다른 유형의 알림 (예: 메시지 또는 메일 앱 등)에서 구분할 수 없습니다.

사용자가 호출에 응답 하려면 MonkeyCall 알림을 밀어 앱을 열고 해당 암호 (또는 사용자 Touch ID)를 입력 하 여 통화를 수락 하 고 대화를 시작 하기 전에 전화를 잠금 해제 해야 합니다.

휴대폰의 잠금이 해제 된 경우에도 동일한 작업을 수행할 수 있습니다. 또한 들어오는 MonkeyCall 호출은 화면 위쪽에서 슬라이드를 표시 하는 표준 알림 배너로 표시 됩니다. 알림은 임시 알림이 되므로 사용자가 알림 센터를 열고 응답을 위해 특정 알림을 찾은 다음 호출 하거나,이를 통해 MonkeyCall 앱을 수동으로 시작 하는 것을 사용자가 쉽게 놓치지 않을 수 있습니다.

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP 앱 환경

MonkeyCall 앱에서 새 CallKit Api를 구현 하면 iOS 10에서 들어오는 VOIP 통화에 대 한 사용자 환경이 크게 향상 될 수 있습니다. 휴대폰을 위에서 잠근 경우 VOIP 통화를 수신 하는 사용자의 예를 들어 보겠습니다. CallKit를 구현 하면 호출이 기본 제공 Phone 앱에서 수신 되는 경우와 마찬가지로 iPhone의 잠금 화면에 표시 되 고, 전체 화면, 네이티브 UI 및 표준 살짝 밀기 응답 기능을 사용 합니다.

또한 MonkeyCall VOIP 호출이 수신 될 때 iPhone의 잠금이 해제 된 경우 기본 제공 Phone 앱의 동일한 전체 화면, 기본 UI 및 표준 살짝 밀기-응답 및 탭-거절 기능이 제공 되 고 MonkeyCall에 사용자 지정 벨 소리를 재생할 수 있는 옵션이 제공 됩니다. .

CallKit는 MonkeyCall에 추가 기능을 제공 하 여 VOIP 호출이 다른 형식의 호출과 상호 작용할 수 있도록 하 고 기본 제공 최근 및 즐겨찾기 목록에 표시 되 고, 기본 제공 되는 기능 및 차단 기능을 사용 하 고, Siri의 MonkeyCall 호출을 시작 합니다. 사용자가 연락처 앱에서 사용자에 게 MonkeyCall 호출을 할당할 수 있는 기능을 제공 합니다.

다음 섹션에서는 CallKit 아키텍처, 들어오고 나가는 호출 흐름 및 CallKit API에 대해 자세히 설명 합니다.

## <a name="the-callkit-architecture"></a>CallKit 아키텍처

IOS 10에서 Apple은 모든 시스템 서비스에서 callkit를 채택 하 여, 예를 들어 CarPlay에서 발생 하는 호출을 CallKit를 통해 시스템 UI에 알고 있습니다. 아래에 제공 된 예제에서 MonkeyCall은 CallKit를 채택 했으므로 이러한 기본 제공 시스템 서비스와 동일한 방식으로 시스템에 알려져 있으며 동일한 모든 기능을 가져옵니다.

[![](callkit-images/callkit01.png "The CallKit Service Stack")](callkit-images/callkit01.png#lightbox)

위 다이어그램의 MonkeyCall 앱에 대해 자세히 살펴보겠습니다. 앱은 자체 네트워크와 통신 하는 모든 코드를 포함 하며 자체 사용자 인터페이스를 포함 합니다. 다음은 시스템과 통신 하는 CallKit의 링크입니다.

[![](callkit-images/callkit02.png "MonkeyCall App Architecture")](callkit-images/callkit02.png#lightbox)

앱에서 사용 하는 CallKit에는 두 가지 주요 인터페이스가 있습니다.

- `CXProvider`-MonkeyCall 앱에서 발생할 수 있는 대역 외 알림을 시스템에 알릴 수 있습니다.
- `CXCallController`-MonkeyCall 앱이 로컬 사용자 동작을 시스템에 알릴 수 있습니다.

### <a name="the-cxprovider"></a>CXProvider

위에서 설명한 것 처럼 `CXProvider`는 응용 프로그램에서 대역 외 알림이 발생할 수 있음을 시스템에 알릴 수 있도록 허용 합니다. 이러한 알림은 로컬 사용자 동작으로 인해 발생 하지 않지만 들어오는 호출과 같은 외부 이벤트로 인해 발생 합니다.

앱은 다음에 대 한 `CXProvider`를 사용 해야 합니다.

- 시스템에 대 한 들어오는 호출을 보고 합니다.
- 발신 전화가 시스템에 연결 되어 있음을 보고 합니다.
- 원격 사용자가 시스템에 대 한 호출을 종료 하는 것을 보고 합니다.

앱은 시스템과 통신 하려고 할 때 `CXCallUpdate` 클래스를 사용 하 고 시스템이 앱과 통신 해야 하는 경우 `CXAction` 클래스를 사용 합니다.

[![](callkit-images/callkit03.png "Communicating with the system via a CXProvider")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController`를 사용 하면 앱에서 VOIP 통화를 시작 하는 사용자와 같은 로컬 사용자 작업을 시스템에 알릴 수 있습니다. `CXCallController`를 구현 하 여 시스템에서 다른 형식의 호출로 상호 작용를 가져옵니다. 예를 들어 진행 중인 활성 전화 통신 호출이 이미 있는 경우 VOIP 앱이 해당 호출을 대기 중으로 설정 하 고 VOIP 호출을 시작 하거나 답변할 수 `CXCallController` 수 있습니다.

앱은 다음에 대 한 `CXCallController`를 사용 해야 합니다.

- 사용자가 시스템에 대 한 나가는 호출을 시작 했을 때 보고 합니다.
- 사용자가 시스템에 대 한 들어오는 호출에 응답 하는 경우 보고 합니다.
- 사용자가 시스템에 대 한 호출을 종료 하는 경우 보고 합니다.

응용 프로그램은 로컬 사용자 동작을 시스템에 전달 하려는 경우 `CXTransaction` 클래스를 사용 합니다.

[![](callkit-images/callkit04.png "Reporting to the system using a CXCallController")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit 구현

다음 섹션에서는 Xamarin.ios VOIP 앱에서 CallKit를 구현 하는 방법을 보여 줍니다. 예를 들어이 문서에서는 가상 MonkeyCall VOIP 앱의 코드를 사용 합니다. 여기에 제공 되는 코드는 몇 가지 지원 클래스를 나타내며, CallKit 특정 부분은 다음 섹션에서 자세히 설명 합니다.

### <a name="the-activecall-class"></a>ActiveCall 클래스

`ActiveCall` 클래스는 MonkeyCall 앱에서 현재 활성 상태인 VOIP 호출에 대 한 모든 정보를 다음과 같이 저장 하는 데 사용 됩니다.

```csharp
using System;
using CoreFoundation;
using Foundation;

namespace MonkeyCall
{
    public class ActiveCall
    {
        #region Private Variables
        private bool isConnecting;
        private bool isConnected;
        private bool isOnhold;
        #endregion

        #region Computed Properties
        public NSUuid UUID { get; set; }
        public bool isOutgoing { get; set; }
        public string Handle { get; set; }
        public DateTime StartedConnectingOn { get; set;}
        public DateTime ConnectedOn { get; set;}
        public DateTime EndedOn { get; set; }

        public bool IsConnecting {
            get { return isConnecting; }
            set {
                isConnecting = value;
                if (isConnecting) StartedConnectingOn = DateTime.Now;
                RaiseStartingConnectionChanged ();
            }
        }

        public bool IsConnected {
            get { return isConnected; }
            set {
                isConnected = value;
                if (isConnected) {
                    ConnectedOn = DateTime.Now;
                } else {
                    EndedOn = DateTime.Now;
                }
                RaiseConnectedChanged ();
            }
        }

        public bool IsOnHold {
            get { return isOnhold; }
            set {
                isOnhold = value;
            }
        }
        #endregion

        #region Constructors
        public ActiveCall ()
        {
        }

        public ActiveCall (NSUuid uuid, string handle, bool outgoing)
        {
            // Initialize
            this.UUID = uuid;
            this.Handle = handle;
            this.isOutgoing = outgoing;
        }
        #endregion

        #region Public Methods
        public void StartCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call starting successfully
            completionHandler (true);

            // Simulate making a starting and completing a connection
            DispatchQueue.MainQueue.DispatchAfter (new DispatchTime(DispatchTime.Now, 3000), () => {
                // Note that the call is starting
                IsConnecting = true;

                // Simulate pause before connecting
                DispatchQueue.MainQueue.DispatchAfter (new DispatchTime (DispatchTime.Now, 1500), () => {
                    // Note that the call has connected
                    IsConnecting = false;
                    IsConnected = true;
                });
            });
        }

        public void AnswerCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call being answered
            IsConnected = true;
            completionHandler (true);
        }

        public void EndCall (ActiveCallbackDelegate completionHandler)
        {
            // Simulate the call ending
            IsConnected = false;
            completionHandler (true);
        }
        #endregion

        #region Events
        public delegate void ActiveCallbackDelegate (bool successful);
        public delegate void ActiveCallStateChangedDelegate (ActiveCall call);

        public event ActiveCallStateChangedDelegate StartingConnectionChanged;
        internal void RaiseStartingConnectionChanged ()
        {
            if (this.StartingConnectionChanged != null) this.StartingConnectionChanged (this);
        }

        public event ActiveCallStateChangedDelegate ConnectedChanged;
        internal void RaiseConnectedChanged ()
        {
            if (this.ConnectedChanged != null) this.ConnectedChanged (this);
        }
        #endregion
    }
}
```

`ActiveCall`에는 호출 상태를 정의 하는 여러 속성과 호출 상태가 변경 될 때 발생할 수 있는 이벤트 두 개가 포함 됩니다. 이는 예제 이기 때문에 시뮬레이션 시작, 응답 및 종료를 시뮬레이션 하는 데 사용 되는 세 가지 메서드가 있습니다.

### <a name="the-startcallrequest-class"></a>StartCallRequest 클래스

`StartCallRequest` 정적 클래스는 나가는 호출로 작업할 때 사용 되는 몇 가지 도우미 메서드를 제공 합니다.

```csharp
using System;
using Foundation;
using Intents;

namespace MonkeyCall
{
    public static class StartCallRequest
    {
        public static string URLScheme {
            get { return "monkeycall"; }
        }

        public static string ActivityType {
            get { return INIntentIdentifier.StartAudioCall.GetConstant ().ToString (); }
        }

        public static string CallHandleFromURL (NSUrl url)
        {
            // Is this a MonkeyCall handle?
            if (url.Scheme == URLScheme) {
                // Yes, return host
                return url.Host;
            } else {
                // Not handled
                return null;
            }
        }

        public static string CallHandleFromActivity (NSUserActivity activity)
        {
            // Is this a start call activity?
            if (activity.ActivityType == ActivityType) {
                // Yes, trap any errors
                try {
                    // Get first contact
                    var interaction = activity.GetInteraction ();
                    var startAudioCallIntent = interaction.Intent as INStartAudioCallIntent;
                    var contact = startAudioCallIntent.Contacts [0];

                    // Get the person handle
                    return contact.PersonHandle.Value;
                } catch {
                    // Error, report null
                    return null;
                }
            } else {
                // Not handled
                return null;
            }
        }
    }
}
```

`CallHandleFromURL` 및 `CallHandleFromActivity` 클래스는 나가는 호출에서 호출 되는 사람의 연락처 핸들을 가져오기 위해 AppDelegate에 사용 됩니다. 자세한 내용은 아래의 [발신 호출 처리](#handling-outgoing-calls) 섹션을 참조 하십시오.

### <a name="the-activecallmanager-class"></a>ActiveCallManager 클래스

`ActiveCallManager` 클래스는 MonkeyCall 앱에서 열려 있는 모든 호출을 처리 합니다.

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using CallKit;

namespace MonkeyCall
{
    public class ActiveCallManager
    {
        #region Private Variables
        private CXCallController CallController = new CXCallController ();
        #endregion

        #region Computed Properties
        public List<ActiveCall> Calls { get; set; }
        #endregion

        #region Constructors
        public ActiveCallManager ()
        {
            // Initialize
            this.Calls = new List<ActiveCall> ();
        }
        #endregion

        #region Private Methods
        private void SendTransactionRequest (CXTransaction transaction)
        {
            // Send request to call controller
            CallController.RequestTransaction (transaction, (error) => {
                // Was there an error?
                if (error == null) {
                    // No, report success
                    Console.WriteLine ("Transaction request sent successfully.");
                } else {
                    // Yes, report error
                    Console.WriteLine ("Error requesting transaction: {0}", error);
                }
            });
        }
        #endregion

        #region Public Methods
        public ActiveCall FindCall (NSUuid uuid)
        {
            // Scan for requested call
            foreach (ActiveCall call in Calls) {
                if (call.UUID.Equals(uuid)) return call;
            }

            // Not found
            return null;
        }

        public void StartCall (string contact)
        {
            // Build call action
            var handle = new CXHandle (CXHandleType.Generic, contact);
            var startCallAction = new CXStartCallAction (new NSUuid (), handle);

            // Create transaction
            var transaction = new CXTransaction (startCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void EndCall (ActiveCall call)
        {
            // Build action
            var endCallAction = new CXEndCallAction (call.UUID);

            // Create transaction
            var transaction = new CXTransaction (endCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void PlaceCallOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, true);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }

        public void RemoveCallFromOnHold (ActiveCall call)
        {
            // Build action
            var holdCallAction = new CXSetHeldCallAction (call.UUID, false);

            // Create transaction
            var transaction = new CXTransaction (holdCallAction);

            // Inform system of call request
            SendTransactionRequest (transaction);
        }
        #endregion
    }
}
```

이는 시뮬레이션 전용 이므로 `ActiveCallManager` `ActiveCall` 개체의 컬렉션을 유지 관리 하 고 `UUID` 속성에 의해 지정 된 호출을 찾기 위한 루틴이 있습니다. 또한 나가는 호출의 보류 상태를 시작, 종료 및 변경 하는 메서드도 포함 합니다. 자세한 내용은 아래의 [발신 호출 처리](#handling-outgoing-calls) 섹션을 참조 하십시오.

### <a name="the-providerdelegate-class"></a>ProviderDelegate 클래스

위에서 설명한 것 처럼 `CXProvider`는 대역 외 알림을 위해 앱과 시스템 간의 양방향 통신을 제공 합니다. 개발자는 대역 외 CallKit 이벤트를 처리 하기 위해 사용자 지정 `CXProviderDelegate`를 제공 하 고 앱에 대 한 `CXProvider`에 연결 해야 합니다. MonkeyCall은 다음 `CXProviderDelegate`를 사용 합니다.

```csharp
using System;
using Foundation;
using CallKit;
using UIKit;

namespace MonkeyCall
{
    public class ProviderDelegate : CXProviderDelegate
    {
        #region Computed Properties
        public ActiveCallManager CallManager { get; set;}
        public CXProviderConfiguration Configuration { get; set; }
        public CXProvider Provider { get; set; }
        #endregion

        #region Constructors
        public ProviderDelegate (ActiveCallManager callManager)
        {
            // Save connection to call manager
            CallManager = callManager;

            // Define handle types
            var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };

            // Get Image Template
            var templateImage = UIImage.FromFile ("telephone_receiver.png");

            // Setup the initial configurations
            Configuration = new CXProviderConfiguration ("MonkeyCall") {
                MaximumCallsPerCallGroup = 1,
                SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
                IconTemplateImageData = templateImage.AsPNG(),
                RingtoneSound = "musicloop01.wav"
            };

            // Create a new provider
            Provider = new CXProvider (Configuration);

            // Attach this delegate
            Provider.SetDelegate (this, null);

        }
        #endregion

        #region Override Methods
        public override void DidReset (CXProvider provider)
        {
            // Remove all calls
            CallManager.Calls.Clear ();
        }

        public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
        {
            // Create new call record
            var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

            // Monitor state changes
            activeCall.StartingConnectionChanged += (call) => {
                if (call.isConnecting) {
                    // Inform system that the call is starting
                    Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
                }
            };

            activeCall.ConnectedChanged += (call) => {
                if (call.isConnected) {
                    // Inform system that the call has connected
                    provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
                }
            };

            // Start call
            activeCall.StartCall ((successful) => {
                // Was the call able to be started?
                if (successful) {
                    // Yes, inform the system
                    action.Fulfill ();

                    // Add call to manager
                    CallManager.Calls.Add (activeCall);
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.AnswerCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Attempt to answer call
            call.EndCall ((successful) => {
                // Was the call successfully answered?
                if (successful) {
                    // Remove call from manager's queue
                    CallManager.Calls.Remove (call);

                    // Yes, inform system
                    action.Fulfill ();
                } else {
                    // No, inform system
                    action.Fail ();
                }
            });
        }

        public override void PerformSetHeldCallAction (CXProvider provider, CXSetHeldCallAction action)
        {
            // Find requested call
            var call = CallManager.FindCall (action.CallUuid);

            // Found?
            if (call == null) {
                // No, inform system and exit
                action.Fail ();
                return;
            }

            // Update hold status
            call.isOnHold = action.OnHold;

            // Inform system of success
            action.Fulfill ();
        }

        public override void TimedOutPerformingAction (CXProvider provider, CXAction action)
        {
            // Inform user that the action has timed out
        }

        public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // Start the calls audio session here
        }

        public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
        {
            // End the calls audio session and restart any non-call
            // related audio
        }
        #endregion

        #region Public Methods
        public void ReportIncomingCall (NSUuid uuid, string handle)
        {
            // Create update to describe the incoming call and caller
            var update = new CXCallUpdate ();
            update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

            // Report incoming call to system
            Provider.ReportNewIncomingCall (uuid, update, (error) => {
                // Was the call accepted
                if (error == null) {
                    // Yes, report to call manager
                    CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
                } else {
                    // Report error to user here
                    Console.WriteLine ("Error: {0}", error);
                }
            });
        }
        #endregion
    }
}
```

이 대리자의 인스턴스를 만들면 호출 활동을 처리 하는 데 사용할 `ActiveCallManager` 전달 됩니다. 그런 다음 `CXProvider` 응답할 핸들 유형 (`CXHandleType`)을 정의 합니다.

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

또한 호출이 진행 중일 때 앱 아이콘에 적용 되는 템플릿 이미지를 가져옵니다.

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

이러한 값은 `CXProvider`를 구성 하는 데 사용 되는 `CXProviderConfiguration`에 제공 됩니다.

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

그런 다음이 대리자는 이러한 구성을 사용 하 여 새 `CXProvider`을 만들어 자신에 게 연결 합니다.

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit를 사용 하는 경우 앱은 더 이상 자체 오디오 세션을 만들고 처리 하지 않으며, 대신 시스템에서 만들고 처리할 오디오 세션을 구성 하 고 사용 해야 합니다. 

실제 앱 인 경우 `DidActivateAudioSession` 메서드는 시스템에서 제공 하는 미리 구성 된 `AVAudioSession`를 사용 하 여 호출을 시작 하는 데 사용 됩니다.

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

또한 `DidDeactivateAudioSession` 메서드를 사용 하 여 시스템 제공 오디오 세션에 대 한 연결을 마무리 하 고 해제 합니다.

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

나머지 코드에 대해서는 다음 섹션에서 자세히 설명 합니다.

### <a name="the-appdelegate-class"></a>AppDelegate 클래스

MonkeyCall은 AppDelegate를 사용 하 여 앱 전체에서 사용 되는 `ActiveCallManager` 및 `CXProviderDelegate`의 인스턴스를 유지 합니다.

```csharp
using Foundation;
using UIKit;
using Intents;
using System;

namespace MonkeyCall
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        #region Constructors
        public override UIWindow Window { get; set; }
        public ActiveCallManager CallManager { get; set; }
        public ProviderDelegate CallProviderDelegate { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Initialize the call handlers
            CallManager = new ActiveCallManager ();
            CallProviderDelegate = new ProviderDelegate (CallManager);

            return true;
        }

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            // Get handle from url
            var handle = StartCallRequest.CallHandleFromURL (url);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from URL: {0}", url); 
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
        {
            var handle = StartCallRequest.CallHandleFromActivity (userActivity);

            // Found?
            if (handle == null) {
                // No, report to system
                Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
                return false;
            } else {
                // Yes, start call and inform system
                CallManager.StartCall (handle);
                return true;
            }
        }

        ...
        #endregion
    }
}
```

`OpenUrl` 및 `ContinueUserActivity` 재정의 메서드는 응용 프로그램이 나가는 호출을 처리할 때 사용 됩니다. 자세한 내용은 아래의 [발신 호출 처리](#handling-outgoing-calls) 섹션을 참조 하십시오.

## <a name="handling-incoming-calls"></a>들어오는 호출 처리

들어오는 VOIP 호출에서 다음과 같은 일반적인 수신 호출 워크플로를 진행할 수 있는 몇 가지 상태와 프로세스가 있습니다.

- 들어오는 호출이 존재 한다는 것을 사용자 (및 시스템)에 게 알립니다.
- 사용자가 호출에 응답 하 고 다른 사용자와의 호출을 초기화 하려고 할 때 알림을 받습니다.
- 사용자가 현재 호출을 종료 하려는 경우 시스템 및 통신 네트워크에 알립니다.

다음 섹션에서는 앱이 CallKit를 사용 하 여 들어오는 호출 워크플로를 처리 하는 방법에 대해 자세히 살펴보고 MonkeyCall VOIP 앱을 예로 사용 합니다.

### <a name="informing-user-of-incoming-call"></a>들어오는 호출을 사용자에 게 알립니다.

원격 사용자가 로컬 사용자를 사용 하 여 VOIP 대화를 시작 하면 다음 작업이 수행 됩니다.

[![](callkit-images/callkit05.png "A remote user has started a VOIP conversation")](callkit-images/callkit05.png#lightbox)

1. 앱은 들어오는 VOIP 호출이 있는 통신 네트워크에서 알림을 가져옵니다.
2. 앱은 `CXProvider`를 사용 하 여 호출을 알리는 `CXCallUpdate`를 시스템에 보냅니다.
3. 시스템은 CallKit를 사용 하 여 시스템 UI, 시스템 서비스 및 기타 모든 VOIP 앱에 대 한 호출을 게시 합니다.

예를 들어 `CXProviderDelegate`에서 다음을 수행 합니다.

```csharp
public void ReportIncomingCall (NSUuid uuid, string handle)
{
    // Create update to describe the incoming call and caller
    var update = new CXCallUpdate ();
    update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);

    // Report incoming call to system
    Provider.ReportNewIncomingCall (uuid, update, (error) => {
        // Was the call accepted
        if (error == null) {
            // Yes, report to call manager
            CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
        } else {
            // Report error to user here
            Console.WriteLine ("Error: {0}", error);
        }
    });
}
```

이 코드는 새 `CXCallUpdate` 인스턴스를 만들고 호출자를 식별할 핸들을 연결 합니다. 그런 다음 `CXProvider` 클래스의 `ReportNewIncomingCall` 메서드를 사용 하 여 호출을 시스템에 알립니다. 성공적으로 실행 되 면 앱의 활성 호출 컬렉션에 호출이 추가 됩니다. 그렇지 않으면 사용자에 게 오류를 보고 해야 합니다.

### <a name="user-answering-incoming-call"></a>들어오는 호출에 대 한 사용자 응답

사용자가 들어오는 VOIP 통화에 응답 하려는 경우 다음 작업이 수행 됩니다.

[![](callkit-images/callkit06.png "The user answers the incoming VOIP call")](callkit-images/callkit06.png#lightbox)

1. 시스템 UI는 사용자가 VOIP 호출에 응답 하려고 함을 시스템에 알립니다.
2. 시스템은 응답 의도를 알려 주는 `CXAnswerCallAction`를 앱 `CXProvider` 보냅니다.
3. 앱은 통신 네트워크에 사용자가 통화에 응답 하 고 있으며 VOIP 호출은 평소와 같이 진행 됩니다.

예를 들어 `CXProviderDelegate`에서 다음을 수행 합니다.

```csharp
public override void PerformAnswerCallAction (CXProvider provider, CXAnswerCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.AnswerCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

이 코드는 먼저 활성 호출 목록에서 지정 된 호출을 검색 합니다. 호출을 찾을 수 없는 경우 시스템에 알림이 표시 되 고 메서드가 종료 됩니다. 이 항목이 발견 되 면 호출을 시작 하기 위해 `ActiveCall` 클래스의 `AnswerCall` 메서드가 호출 되 고 성공 하거나 실패 하는 경우 시스템이 정보를 제공 합니다.

### <a name="user-ending-incoming-call"></a>사용자가 들어오는 호출을 종료 합니다.

사용자가 앱의 UI 내에서 호출을 종료 하려는 경우 다음 작업이 수행 됩니다.

[![](callkit-images/callkit07.png "The user terminates the call from within the app's UI")](callkit-images/callkit07.png#lightbox)

1. 앱은 호출을 종료 하 고 있음을 알리기 위해 시스템에 전송 되는 `CXTransaction`에 번들로 제공 되는 `CXEndCallAction`을 만듭니다.
2. 시스템은 최종 호출 의도를 확인 하 고 `CXProvider`를 통해 `CXEndCallAction`을 다시 앱으로 보냅니다.
3. 그러면 앱은 통신 네트워크에 호출이 종료 되었음을 알립니다.

예를 들어 `CXProviderDelegate`에서 다음을 수행 합니다.

```csharp
public override void PerformEndCallAction (CXProvider provider, CXEndCallAction action)
{
    // Find requested call
    var call = CallManager.FindCall (action.CallUuid);

    // Found?
    if (call == null) {
        // No, inform system and exit
        action.Fail ();
        return;
    }

    // Attempt to answer call
    call.EndCall ((successful) => {
        // Was the call successfully answered?
        if (successful) {
            // Remove call from manager's queue
            CallManager.Calls.Remove (call);

            // Yes, inform system
            action.Fulfill ();
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

이 코드는 먼저 활성 호출 목록에서 지정 된 호출을 검색 합니다. 호출을 찾을 수 없는 경우 시스템에 알림이 표시 되 고 메서드가 종료 됩니다. 이 항목이 발견 되 면 호출을 종료 하기 위해 `ActiveCall` 클래스의 `EndCall` 메서드가 호출 되 고 성공 하거나 실패 하는 경우 시스템이 정보를 제공 합니다. 성공 하면 활성 호출의 컬렉션에서 호출이 제거 됩니다.

## <a name="managing-multiple-calls"></a>여러 호출 관리

대부분의 VOIP 앱은 한 번에 여러 호출을 처리할 수 있습니다. 예를 들어 현재 활성 VOIP 호출이 있고 앱에서 들어오는 호출이 새로 발생 한다는 알림을 받은 경우 두 번째 호출에 대 한 첫 번째 호출에서 일시 중지 하거나 중지할 수 있습니다.

위의 상황에서 시스템은 여러 작업 (예: `CXEndCallAction` 및 `CXAnswerCallAction`)의 목록을 포함 하는 앱에 `CXTransaction`를 보냅니다. 이러한 모든 작업은 개별적으로 수행 해야 하므로 시스템에서 UI를 적절 하 게 업데이트할 수 있습니다.

## <a name="handling-outgoing-calls"></a>나가는 호출 처리

사용자가 최근 목록 (예: Phone 앱)에서 항목을 탭 하는 경우, 예를 들어 앱에 속하는 호출에서 시작 하는 경우 시스템에서 _시작 호출_ 을 받게 됩니다.

[![](callkit-images/callkit08.png "Receiving a Start Call Intent")](callkit-images/callkit08.png#lightbox)

1. 앱은 시스템에서 받은 시작 호출 의도에 따라 _호출 시작 동작_ 을 만듭니다. 
2. 앱은 `CXCallController`를 사용 하 여 시스템에서 호출 시작 작업을 요청 합니다.
3. 시스템이 작업을 수락 하면 `XCProvider` 대리자를 통해 응용 프로그램으로 반환 됩니다.
4. 앱은 통신 네트워크를 사용 하 여 나가는 호출을 시작 합니다.

의도에 대 한 자세한 내용은 의도 [및 의도 UI 확장](~/ios/platform/sirikit/understanding-sirikit.md) 설명서를 참조 하세요. 

### <a name="the-outgoing-call-lifecycle"></a>나가는 통화 수명 주기

CallKit 및 나가는 호출로 작업할 때 앱은 다음 수명 주기 이벤트를 시스템에 알려야 합니다.

1. **시작** -나가는 호출이 시작 될 것으로 시스템에 알립니다.
2. **시작 됨** -시스템에 나가는 호출이 시작 되었음을 알립니다.
3. **연결** -나가는 호출이 연결 하 고 있음을 시스템에 알립니다.
4. **연결 됨** -나가는 호출이 연결 되었으며 양쪽 당사자가 지금 통신할 수 있음을 알립니다.

예를 들어 다음 코드는 나가는 호출을 시작 합니다.

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void StartCall (string contact)
{
    // Build call action
    var handle = new CXHandle (CXHandleType.Generic, contact);
    var startCallAction = new CXStartCallAction (new NSUuid (), handle);

    // Create transaction
    var transaction = new CXTransaction (startCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

`CXHandle` 만들고이를 사용 하 여 `CXCallController` 클래스의 `RequestTransaction` 메서드를 사용 하 여 시스템으로 전송 되는 `CXTransaction`에 번들로 제공 되는 `CXStartCallAction`를 구성 합니다. `RequestTransaction` 메서드를 호출 하면 새 호출이 시작 되기 전에 시스템에서 소스 (Phone 앱, FaceTime, VOIP 등)와 관계 없이 모든 기존 호출을 보류 중으로 설정할 수 있습니다.

나가는 VOIP 통화를 시작 하는 요청은 Siri와 같은 여러 다른 원본 (연락처 앱에서 연락처 앱에 있는 항목) 또는 최근 목록 (Phone 앱)에서 가져올 수 있습니다. 이러한 상황에서 앱은 `NSUserActivity` 내에서 시작 호출 의도를 보내고 AppDelegate는이를 처리 해야 합니다.

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var handle = StartCallRequest.CallHandleFromActivity (userActivity);

    // Found?
    if (handle == null) {
        // No, report to system
        Console.WriteLine ("Unable to get call handle from User Activity: {0}", userActivity);
        return false;
    } else {
        // Yes, start call and inform system
        CallManager.StartCall (handle);
        return true;
    }
}
```

여기에서 도우미 클래스 `StartCallRequest`의 `CallHandleFromActivity` 메서드는 호출 되는 사용자에 대 한 핸들을 가져오는 데 사용 됩니다 (위의 [StartCallRequest 클래스](#the-startcallrequest-class) 참조).

[Providerdelegate 클래스](#the-providerdelegate-class) 의 `PerformStartCallAction` 메서드는 마지막으로 나가는 호출을 시작 하 고 시스템의 수명 주기를 알리는 데 사용 됩니다.

```csharp
public override void PerformStartCallAction (CXProvider provider, CXStartCallAction action)
{
    // Create new call record
    var activeCall = new ActiveCall (action.CallUuid, action.CallHandle.Value, true);

    // Monitor state changes
    activeCall.StartingConnectionChanged += (call) => {
        if (call.IsConnecting) {
            // Inform system that the call is starting
            Provider.ReportConnectingOutgoingCall (call.UUID, call.StartedConnectingOn.ToNSDate());
        }
    };

    activeCall.ConnectedChanged += (call) => {
        if (call.IsConnected) {
            // Inform system that the call has connected
            Provider.ReportConnectedOutgoingCall (call.UUID, call.ConnectedOn.ToNSDate ());
        }
    };

    // Start call
    activeCall.StartCall ((successful) => {
        // Was the call able to be started?
        if (successful) {
            // Yes, inform the system
            action.Fulfill ();

            // Add call to manager
            CallManager.Calls.Add (activeCall);
        } else {
            // No, inform system
            action.Fail ();
        }
    });
}
```

진행 중인 호출에 대 한 정보를 포함 하는 `ActiveCall` 클래스의 인스턴스를 만들고 호출 되는 사용자로 채웁니다. `StartingConnectionChanged` 및 `ConnectedChanged` 이벤트는 나가는 통화 수명 주기를 모니터링 하 고 보고 하는 데 사용 됩니다. 호출이 시작 되 고 시스템에서 작업이 완료 되었음을 알립니다.

### <a name="ending-an-outgoing-call"></a>나가는 호출 종료

사용자가 나가는 호출을 마치고 종료 하려는 경우 다음 코드를 사용할 수 있습니다.

```csharp
private CXCallController CallController = new CXCallController ();
...

private void SendTransactionRequest (CXTransaction transaction)
{
    // Send request to call controller
    CallController.RequestTransaction (transaction, (error) => {
        // Was there an error?
        if (error == null) {
            // No, report success
            Console.WriteLine ("Transaction request sent successfully.");
        } else {
            // Yes, report error
            Console.WriteLine ("Error requesting transaction: {0}", error);
        }
    });
}

public void EndCall (ActiveCall call)
{
    // Build action
    var endCallAction = new CXEndCallAction (call.UUID);

    // Create transaction
    var transaction = new CXTransaction (endCallAction);

    // Inform system of call request
    SendTransactionRequest (transaction);
}
```

가 end에 대 한 호출의 UUID를 사용 하 여 `CXEndCallAction`를 만든 경우 `CXCallController` 클래스의 `RequestTransaction` 메서드를 사용 하 여 시스템으로 전송 되는 `CXTransaction`으로 묶습니다. 

## <a name="additional-callkit-details"></a>추가 CallKit 세부 정보

이 섹션에서는 다음과 같은 CallKit로 작업할 때 개발자가 고려해 야 하는 몇 가지 추가 세부 정보를 다룹니다.

- 공급자 구성
- 작업 오류
- 시스템 제한 사항
- VOIP 오디오

### <a name="provider-configuration"></a>공급자 구성

공급자 구성에서는 CallKit로 작업할 때 iOS 10 VOIP 앱에서 사용자 환경을 사용자 지정할 수 있습니다 (기본 호출 UI 내부).

앱에서 다음과 같은 유형의 사용자 지정을 수행할 수 있습니다.

- 지역화 된 이름을 표시 합니다.
- 비디오 통화 지원 사용.
- 자체 템플릿 이미지 아이콘을 표시 하 여 호출 UI에서 단추를 사용자 지정 합니다. 사용자 지정 단추와의 사용자 상호 작용은 처리할 앱에 직접 전송 됩니다. 

### <a name="action-errors"></a>작업 오류

CallKit를 사용 하는 iOS 10 VOIP 앱은 정상적으로 실패 한 작업을 처리 하 고 사용자에 게 작업 상태를 항상 알려 주는 상태를 유지 해야 합니다. 

다음 예제를 고려 합니다.

1. 앱이 시작 호출 작업을 수신 하 고 해당 통신 네트워크를 사용 하 여 새 VOIP 호출을 초기화 하는 프로세스를 시작 했습니다.
2. 네트워크 통신 기능이 제한 되어 있거나 없기 때문에이 연결에 실패 합니다.
3. 응용 프로그램은 오류를 시스템에 알리기 위해 호출 시작 작업 (`Action.Fail()`)으로 **실패** 메시지를 다시 보내야 *합니다* .
4. 이를 통해 시스템은 사용자에 게 호출 상태를 알릴 수 있습니다. 예를 들어 호출 오류 UI를 표시 합니다.

또한 iOS 10 VOIP 앱은 지정 된 시간 내에 예상 되는 작업을 처리할 수 없을 때 발생할 수 있는 _시간 제한 오류_ 에 응답 해야 합니다. CallKit에서 제공 되는 각 작업 유형에는 연결 된 최대 시간 제한 값이 있습니다. 이러한 시간 제한 값은 사용자가 요청한 CallKit 작업이 반응 형 방식으로 처리 되도록 하 여 OS 유체와 응답성을 유지 합니다.

이러한 시간 제한 상황을 정상적으로 처리 하기 위해 재정의 해야 하는 공급자 대리자 (`CXProviderDelegate`)에는 여러 메서드가 있습니다.

### <a name="system-restrictions"></a>시스템 제한 사항

IOS 10 VOIP 앱을 실행 중인 iOS 장치의 현재 상태에 따라 특정 시스템 제한이 적용 될 수 있습니다.

예를 들어 다음 경우 시스템에서 들어오는 VOIP 호출을 제한할 수 있습니다.

1. 을 호출 하는 사람은 사용자의 차단 된 호출자 목록에 있습니다.
2. 사용자의 iOS 장치는 비-방해 모드입니다.

VOIP 호출이 이러한 상황에 의해 제한 되는 경우 다음 코드를 사용 하 여 처리 합니다.

```csharp
public class ProviderDelegate : CXProviderDelegate
{
...

    public void ReportIncomingCall (NSUuid uuid, string handle)
    {
        // Create update to describe the incoming call and caller
        var update = new CXCallUpdate ();
        update.RemoteHandle = new CXHandle (CXHandleType.Generic, handle);
    
        // Report incoming call to system
        Provider.ReportNewIncomingCall (uuid, update, (error) => {
            // Was the call accepted
            if (error == null) {
                // Yes, report to call manager
                CallManager.Calls.Add (new ActiveCall (uuid, handle, false));
            } else {
                // Report error to user here
                if (error.Code == (int)CXErrorCodeIncomingCallError.CallUuidAlreadyExists) {
                    // Handle duplicate call ID
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByBlockList) {
                    // Handle call from blocked user
                } else if (error.Code == (int)CXErrorCodeIncomingCallError.FilteredByDoNotDisturb) {
                    // Handle call while in do-not-disturb mode
                } else {
                    // Handle unknown error
                }
            }
        });
    }

}
```

### <a name="voip-audio"></a>VOIP 오디오

CallKit는 라이브 VOIP 통화 중 iOS 10 VOIP 앱에 필요한 오디오 리소스를 처리할 수 있는 몇 가지 이점을 제공 합니다. 가장 큰 이점 중 하나는 앱의 오디오 세션이 iOS 10에서 실행 될 때 높은 우선 순위를 갖는 것입니다. 이는 기본 제공 전화 및 FaceTime 앱과 동일한 우선 순위 수준 이며,이 향상 된 우선 순위 수준은 실행 중인 다른 앱이 VOIP 앱의 오디오 세션을 중단 하지 못하도록 합니다.

또한 CallKit는 성능을 향상 시키고 사용자 기본 설정 및 장치 상태에 따라 라이브 통화 중에 특정 출력 장치로 VOIP 오디오를 지능적으로 라우팅할 수 있는 다른 오디오 라우팅 힌트에 액세스할 수 있습니다. 예를 들어 bluetooth 헤드폰, live CarPlay 연결 또는 내게 필요한 옵션 설정 등의 연결 된 장치를 기준으로 합니다.

CallKit를 사용 하는 일반적인 VOIP 호출의 수명 주기 동안 앱은 CallKit에서 제공 하는 오디오 스트림을 구성 해야 합니다. 다음 예제를 살펴보겠습니다.

[![](callkit-images/callkit09.png "The Start Call Action Sequence")](callkit-images/callkit09.png#lightbox)

1. 들어오는 호출에 응답 하기 위해 앱에서 호출 시작 작업을 수신 합니다.
2. 응용 프로그램에서이 작업을 수행 하기 전에 `AVAudioSession`에 필요한 구성을 제공 합니다.
3. 앱은 작업이 수행 되었음을 시스템에 알립니다.
4. 호출이 연결 되기 전에 CallKit는 앱이 요청한 구성과 일치 하는 우선 순위가 높은 `AVAudioSession`을 제공 합니다. 앱은 `CXProviderDelegate`의 `DidActivateAudioSession` 메서드를 통해 알림이 제공 됩니다.

## <a name="working-with-call-directory-extensions"></a>호출 디렉터리 확장 작업

CallKit로 작업 하는 경우 _호출 디렉터리 확장_ 을 사용 하 여 차단 된 호출 번호를 추가 하 고 지정 된 VOIP 앱과 관련 된 숫자를 IOS 장치의 연락처 앱에 있는 연락처에 식별할 수 있습니다.

### <a name="implementing-a-call-directory-extension"></a>호출 디렉터리 확장 구현

Xamarin.ios 앱에서 호출 디렉터리 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 앱 솔루션을 엽니다.
2. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**를 선택 합니다.
3. **IOS** > **확장** > **호출 디렉터리 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다. 

    [![](callkit-images/calldir01.png "Creating a new Call Directory Extension")](callkit-images/calldir01.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **다음** 단추를 클릭 합니다. 

    [![](callkit-images/calldir02.png "Entering a name for the extension")](callkit-images/calldir02.png#lightbox)
5. 필요한 경우 **프로젝트 이름** 및/또는 **솔루션 이름을** 조정 하 고 **만들기** 단추를 클릭 합니다. 

    [![](callkit-images/calldir03.png "Creating the project")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 앱 솔루션을 엽니다.
2. **솔루션 탐색기** 에서 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 프로젝트 추가**를 선택 합니다.
3. **IOS** > **확장** > **호출 디렉터리 확장** 을 선택 하 고 **다음** 단추를 클릭 합니다. 

    [![](callkit-images/calldir01w.png "Creating a new Call Directory Extension")](callkit-images/calldir01.png#lightbox)
4. 확장의 **이름을** 입력 하 고 **확인** 단추를 클릭 합니다.

-----

그러면 다음과 같은 `CallDirectoryHandler.cs` 클래스가 프로젝트에 추가 됩니다.

```csharp
using System;

using Foundation;
using CallKit;

namespace MonkeyCallDirExtension
{
    [Register ("CallDirectoryHandler")]
    public class CallDirectoryHandler : CXCallDirectoryProvider, ICXCallDirectoryExtensionContextDelegate
    {
        #region Constructors
        protected CallDirectoryHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void BeginRequest (CXCallDirectoryExtensionContext context)
        {
            context.Delegate = this;

            if (!AddBlockingPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add blocking phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 1, null);
                context.CancelRequest (error);
                return;
            }

            if (!AddIdentificationPhoneNumbers (context)) {
                Console.WriteLine ("Unable to add identification phone numbers");
                var error = new NSError (new NSString ("CallDirectoryHandler"), 2, null);
                context.CancelRequest (error);
                return;
            }

            context.CompleteRequest (null);
        }
        #endregion

        #region Private Methods
        private bool AddBlockingPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to block from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 14085555555, 18005555555 };

            foreach (var phoneNumber in phoneNumbers)
                context.AddBlockingEntry (phoneNumber);

            return true;
        }

        private bool AddIdentificationPhoneNumbers (CXCallDirectoryExtensionContext context)
        {
            // Retrieve phone numbers to identify and their identification labels from data store. For optimal performance and memory usage when there are many phone numbers,
            // consider only loading a subset of numbers at a given time and using autorelease pool(s) to release objects allocated during each batch of numbers which are loaded.
            //
            // Numbers must be provided in numerically ascending order.

            long [] phoneNumbers = { 18775555555, 18885555555 };
            string [] labels = { "Telemarketer", "Local business" };

            for (var i = 0; i < phoneNumbers.Length; i++) {
                long phoneNumber = phoneNumbers [i];
                string label = labels [i];
                context.AddIdentificationEntry (phoneNumber, label);
            }

            return true;
        }
        #endregion

        #region Public Methods
        public void RequestFailed (CXCallDirectoryExtensionContext extensionContext, NSError error)
        {
            // An error occurred while adding blocking or identification entries, check the NSError for details.
            // For Call Directory error codes, see the CXErrorCodeCallDirectoryManagerError enum.
            //
            // This may be used to store the error details in a location accessible by the extension's containing app, so that the
            // app may be notified about errors which occurred while loading data even if the request to load data was initiated by
            // the user in Settings instead of via the app itself.
        }
        #endregion
    }
}
```

필요한 기능을 제공 하려면 호출 디렉터리 처리기의 `BeginRequest` 메서드를 수정 해야 합니다. 위의 샘플에서는 VOIP 앱의 연락처 데이터베이스에서 차단 및 사용 가능한 숫자 목록을 설정 하려고 합니다. 어떤 이유로 든 요청이 실패 하는 경우 오류를 설명 하는 `NSError` 만들고 `CXCallDirectoryExtensionContext` 클래스의 `CancelRequest` 메서드를 전달 합니다.

차단 된 번호를 설정 하려면 `CXCallDirectoryExtensionContext` 클래스의 `AddBlockingEntry` 메서드를 사용 합니다. 메서드에 제공 되는 숫자는 숫자순으로 _오름차순 이어야 합니다_ . 전화 번호가 많은 경우 최적의 성능과 메모리 사용을 위해 지정 된 시간에 숫자 하위 집합을 로드 하 고 autorelease pool을 사용 하 여 로드 된 각 일괄 처리에서 할당 된 개체를 해제 하는 것만 고려 하십시오.

VOIP 앱에 알려진 연락처 번호의 연락처에 문의 하려면 `CXCallDirectoryExtensionContext` 클래스의 `AddIdentificationEntry` 메서드를 사용 하 고 숫자와 식별 레이블을 모두 제공 합니다. 또한 메서드에 제공 되는 숫자는 숫자로 오름차순으로 정렬 _되어야_ 합니다. 전화 번호가 많은 경우 최적의 성능과 메모리 사용을 위해 지정 된 시간에 숫자 하위 집합을 로드 하 고 autorelease pool을 사용 하 여 로드 된 각 일괄 처리에서 할당 된 개체를 해제 하는 것만 고려 하십시오.

## <a name="summary"></a>요약

이 문서에서는 iOS 10에서 Apple에서 릴리스된 새 CallKit API와 Xamarin.ios VOIP 앱에서이 API를 구현 하는 방법에 대해 설명 했습니다. 이는 CallKit에서 앱을 iOS 시스템에 통합 하는 방법, 기본 제공 앱 (예: 휴대폰)에 기능 패리티를 제공 하는 방법, Siri 상호 작용을 통해 잠금 및 홈 화면과 같은 위치에서 iOS 전체에 앱의 가시성을 강화 하는 방법을 보여 줍니다. 연락처 앱입니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
