---
title: Xamarin.iOS에서 CallKit
description: 이 문서에서는 새로운 CallKit API는 Apple iOS 10 및 Xamarin.iOS VOIP 앱에서이 구현 하는 방법에 릴리스를 다룹니다.
ms.prod: xamarin
ms.assetid: 738A142D-FFD2-4738-B3ED-57C273179848
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 6db9ff0085c17f07d07a7591f5d735793bfbc5f9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61390597"
---
# <a name="callkit-in-xamarinios"></a>Xamarin.iOS에서 CallKit

IOS 10에서에서 새 CallKit API와 통합 하는 iPhone UI 사용 하 여 친숙 한 인터페이스를 제공 합니다. 최종 사용자 환경을 앱이 VOIP 제공 합니다. 이 API를 사용 하 여 사용자 수 보기 iOS 장치의 잠금 화면에서 VOIP 호출을 사용 하 여 상호 작용 및 Phone 앱을 사용 하 여 연락처를 관리할 **즐겨찾기** 하 고 **최근** 뷰.

## <a name="about-callkit"></a>CallKit에 대 한

Apple에 따라 CallKit는는 첫 번째 파티 iOS 10에서 환경에 타사 음성을 통해 IP (VOIP) 앱으로 상승 하는 새 프레임 워크를 사용 합니다. CallKit API VOIP 앱을을 iPhone UI 사용 하 여 통합 친숙 한 인터페이스를 제공 하 고 최종 사용자에 게 발생할 수 있습니다. 와 마찬가지로 전화 앱은 기본 제공 사용자 수 보기 및 iOS 장치의 잠금 화면에서 VOIP 호출을 사용 하 여 상호 작용 하며 전화 앱을 사용 하 여 연락처를 관리할 **즐겨찾기** 하 고 **최근** 뷰.

또한 CallKit API 이름 (호출자 ID)를 사용 하 여 전화 번호를 연결 하거나 여야 하는 경우 시스템 차단 (차단 호출)를 알릴 수 있는 앱 확장을 만들 수가 있습니다.

### <a name="the-existing-voip-app-experience"></a>기존 VOIP 앱 환경

새로운 CallKit API 및 해당 기능에 설명 하기 전에 타사의 현재 사용자 환경에 iOS에서 VOIP 앱에 9 (및 낮은) MonkeyCall 라는 가상의 VOIP 앱을 사용 하 여 당사자입니다. MonkeyCall는 기존 iOS Api를 사용 하 여 VOIP 호출을 주고받을 수 있도록 하는 간단한 앱입니다.

현재 사용자 MonkeyCall에서 들어오는 호출을 수신 하 고 자신의 iPhone 잠겨, 잠금 화면에서 받은 알림을 구분 되지 않습니다 다른 유형의 알림 (같은 메시지에서 해당 앱을 예를 들어 메일 또는).

사용자는 호출에 응답을 원하는 경우 방해가 MonkeyCall 알림을 앱을 열고 수 호출을 수락 하 고 대화를 시작 하려면 먼저 휴대폰의 잠금을 해제 하려면 해당 암호 (또는 사용자 Touch ID)를 입력 해야 합니다.

전화를 잠금 해제 하면 쉽기는 환경이 지원 됩니다. 마찬가지로 들어오는 MonkeyCall 호출 화면 맨 위에서 슬라이드 하는 표준 알림 배너가 표시 됩니다. 알림을 임시 이므로 누락 될 수 있습니다 쉽게를 알림 센터를 열고 응답 한 다음 호출 또는 찾기 및 MonkeyCall 응용 프로그램을 수동으로 실행 하려면 특정 알림의 찾을 해야만 사용자가 있습니다.

### <a name="the-callkit-voip-app-experience"></a>CallKit VOIP 앱 환경

MonkeyCall 앱에서 새 CallKit Api를 구현 하 여 들어오는 VOIP 호출을 사용 하 여 사용자의 환경은 iOS 10에서에서 크게 향상 될 수 있습니다. 휴대폰에서 위의 잠기면 VOIP 호출을 수신 하는 사용자를 예로 들어를 보겠습니다. CallKit를 구현 하 여 호출 전체 화면, 네이티브 UI 및 표준 안쪽으로 살짝 밀어-답변 기능을 사용 하 여 기본 제공 Phone 앱에서 수신 된 것 처럼 호출은 iPhone의 잠금 화면에 표시 됩니다.

마찬가지로 MonkeyCall VOIP 호출을 받으면 iPhone 잠금 해제 된 경우 동일한 전체 화면, 네이티브 UI 및 표준 안쪽으로 살짝 밀어-응답 탭-거부 되는 기능과 기본 제공 앱이 표시 되는 전화 및 MonkeyCall는 사용자 지정 벨 소리를 재생 합니다. .

CallKit MonkeyCall, 해당 VOIP를 허용 하는 추가 기능에 대 한 호출, 최근 항목에 기본 제공 나타나지 다른 형식과 상호 작용 하도록 호출 하 고 즐겨찾기 목록에서는 기본 제공 방해 금지 및 차단 기능을 사용 하려면 MonkeyCall 호출 Siri에서 시작 하 고 연락처 앱에서 사용자에 게 MonkeyCall 호출에 할당할 사용자를 위한 기능을 제공 합니다.

다음 섹션에서는 CallKit 아키텍처를 설명 하는, 들어오고 나가는 자세히 흐름과 CallKit API를 호출 합니다.

## <a name="the-callkit-architecture"></a>CallKit 아키텍처

Ios 10에서 Apple에서 채택 CallKit 시스템 서비스의 모든 예를 들어 CarPlay에 대 한 호출이 시스템 UI 통해 CallKit 알려져 되도록 합니다. MonkeyCall CallKit 채택 하므로 아래 제공 된 예제를이 이러한 기본 제공 시스템 서비스와 동일한 방식으로 시스템에 알려져 있으며 모두 동일한 기능을 가져옵니다.

[![](callkit-images/callkit01.png "CallKit 서비스 스택")](callkit-images/callkit01.png#lightbox)

위의 다이어그램에서 MonkeyCall 앱에 자세히 살펴보겠습니다. 앱 자체 네트워크와 통신 하는 코드를 모두 포함 되 고 자체 사용자 인터페이스를 포함 합니다. 시스템과 통신 하는 CallKit에 연결 합니다.

[![](callkit-images/callkit02.png "MonkeyCall 앱 아키텍처")](callkit-images/callkit02.png#lightbox)

두 가지 기본 인터페이스가 CallKit는 앱에서:

- `CXProvider` -그러면 MonkeyCall 응용 프로그램에서 발생할 수 있는 모든 대역의 알림 시스템을 알려줍니다.
- `CXCallController` -로컬 사용자 작업의 시스템에 알리기 위해 MonkeyCall 앱을 수 있습니다.

### <a name="the-cxprovider"></a>CXProvider

위에서 설명한 것 처럼 `CXProvider` 는 앱을 통해 발생할 수 있는 모든 대역의 알림 시스템을 알려줍니다. 이 로컬 사용자 작업으로 인해 발생 하지 않지만 들어오는 호출 같은 외부 이벤트로 인해 발생 하는 알림입니다.

앱을 사용할지는 `CXProvider` 다음에 대 한 합니다.

- 시스템에 대 한 들어오는 호출을 보고 합니다.
- 보고서는 시스템에 연결 된 나가는 호출 합니다.
- 시스템에 대 한 호출을 종료 하는 원격 사용자를 보고 합니다.

사용 하 여 앱에서 시스템와 통신 하도록 하려는 경우는 `CXCallUpdate` 클래스 및 시스템을 앱과 통신 해야 하는 경우 사용 된 `CXAction` 클래스:

[![](callkit-images/callkit03.png "CXProvider 통해 시스템과 통신")](callkit-images/callkit03.png#lightbox)

### <a name="the-cxcallcontroller"></a>CXCallController

`CXCallController` 앱이 VOIP 호출을 시작 하는 사용자와 같은 로컬 사용자 작업의 시스템을 알릴 수 있습니다. 구현 하 여를 `CXCallController` 시스템에서 다른 종류의 호출을 사용 하 여 연동에 앱을 가져옵니다. 예를 들어, 진행 중인 활성 전화 통신 호출을 이미 있으면 `CXCallController` VOIP 앱에서 해당 호출을 대기 중으로 배치 하 고 시작 또는 VOIP 호출에 응답을 허용할 수 있습니다.

앱을 사용할지는 `CXCallController` 다음에 대 한 합니다.

- 사용자 시스템에 대 한 나가는 호출 시작 되 면 보고서입니다.
- 사용자 시스템에 들어오는 호출에 응답 하는 경우 보고서입니다.
- 보고서 사용자 시스템에 대 한 호출을 종료 합니다.

사용 하 여 앱에서 로컬 사용자 작업을 시스템 통신 하려는 경우는 `CXTransaction` 클래스:

[![](callkit-images/callkit04.png "CXCallController를 사용 하 여 시스템에 보고")](callkit-images/callkit04.png#lightbox)

## <a name="implementing-callkit"></a>CallKit 구현

다음 섹션에서는 CallKit VOIP Xamarin.iOS 앱을 구현 하는 방법을 표시 됩니다. 예제를 위해이 문서의 코드 가상 MonkeyCall VOIP 앱에서 사용 됩니다. 여기에 제시 된 코드를 나타내는 여러 지원 클래스, 특정 파트는 CallKit 다음 섹션에서 자세히 다룹니다.

### <a name="the-activecall-class"></a>ActiveCall 클래스

`ActiveCall` 클래스는 모든 현재 활성 상태인 같이 VOIP 호출에 대 한 정보를 저장할 MonkeyCall 앱에서 사용 됩니다.

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

`ActiveCall` 호출 및 호출 상태가 변경 될 때 발생할 수 있는 두 이벤트의 상태를 정의 하는 몇 가지 속성을 포함 합니다. 예제 일 뿐 이므로, 시작, 대답 및 통화 종료를 시뮬레이션 하는 데 사용 되는 세 가지 방법 있습니다.

### <a name="the-startcallrequest-class"></a>StartCallRequest 클래스

`StartCallRequest` 정적 클래스는 나가는 작업을 호출할 때 사용할 몇 가지 도우미 메서드를 제공 합니다.

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

합니다 `CallHandleFromURL` 및 `CallHandleFromActivity` 클래스 AppDelegate에서 사용 하는 나가는 호출에서 호출할 사람의 연락처 핸들을 가져올 수 있습니다. 자세한 내용은 참조 하십시오 합니다 [나가는 호출 처리](#handling-outgoing-calls) 섹션 아래.

### <a name="the-activecallmanager-class"></a>ActiveCallManager 클래스

`ActiveCallManager` MonkeyCall 앱에서 열려 있는 모든 호출을 처리 하는 클래스입니다.

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

다시 시뮬레이션만 이므로, 합니다 `ActiveCallManager` 만의 컬렉션을 유지 `ActiveCall` 개체에서 지정된 된 호출을 찾기 위한 루틴을 있고 해당 `UUID` 속성입니다. 또한 시작, 종료 및 나가는 호출의-대기 상태를 변경 하는 메서드를 포함 합니다. 자세한 내용은 참조 하십시오 합니다 [나가는 호출 처리](#handling-outgoing-calls) 섹션 아래.

### <a name="the-providerdelegate-class"></a>ProviderDelegate 클래스

위에서 설명 했 듯이 `CXProvider` 대역의 알림에 대 한 앱 및 시스템 간의 양방향 통신을 제공 합니다. 개발자가 사용자 지정을 제공 해야 `CXProviderDelegate` 에 연결 하 고는 `CXProvider` 대역의 CallKit 이벤트를 처리 하는 앱에 대 한 합니다. 다음을 사용 하 여 MonkeyCall `CXProviderDelegate`:

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

이 대리자의 인스턴스를 만들 때 전달 되는 `ActiveCallManager` 는 사용 하 여 호출 작업을 처리 합니다. 핸들 형식 정의 어 (`CXHandleType`)는는 `CXProvider` 응답할:

```csharp
// Define handle types
var handleTypes = new [] { (NSNumber)(int)CXHandleType.PhoneNumber };
```

및 호출을 진행에서 중일 때 앱의 아이콘에 적용 될 템플릿 이미지를 가져옵니다.

```csharp
// Get Image Template
var templateImage = UIImage.FromFile ("telephone_receiver.png");
```

이러한 값에 번들로 가져올는 `CXProviderConfiguration` 구성에 사용할는 `CXProvider`:

```csharp
// Setup the initial configurations
Configuration = new CXProviderConfiguration ("MonkeyCall") {
    MaximumCallsPerCallGroup = 1,
    SupportedHandleTypes = new NSSet<NSNumber> (handleTypes),
    IconTemplateImageData = templateImage.AsPNG(),
    RingtoneSound = "musicloop01.wav"
};
```

대리자는 다음 새로 만들고 `CXProvider` 이러한 구성을 사용 하 여 자체에 연결 합니다.

```csharp
// Create a new provider
Provider = new CXProvider (Configuration);

// Attach this delegate
Provider.SetDelegate (this, null);
```

CallKit를 사용 하는 경우 앱은 더 이상 만들고 고유한 오디오 세션 처리, 대신 구성 하 고 시스템을 만들고에 처리 하는 오디오 세션을 사용 해야 합니다. 

실제 앱,이 경우는 `DidActivateAudioSession` 메서드를 사용 하 여 미리 구성 된 호출을 시작할 수는 `AVAudioSession` 시스템 제공:

```csharp
public override void DidActivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // Start the call's audio session here...
}
```

사용할 것을 `DidDeactivateAudioSession` 마무리 하 고 시스템에 연결을 해제 하려면 오디오 세션을 제공 합니다.

```csharp
public override void DidDeactivateAudioSession (CXProvider provider, AVFoundation.AVAudioSession audioSession)
{
    // End the calls audio session and restart any non-call
    // releated audio
}
```

코드의 나머지 부분에 나오는 섹션에서 자세히 설명 합니다.

### <a name="the-appdelegate-class"></a>AppDelegate 클래스

MonkeyCall AppDelegate를 사용 하 여 인스턴스를 포함 하는 `ActiveCallManager` 고 `CXProviderDelegate` 앱 전체에서 사용 되는:

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

합니다 `OpenUrl` 및 `ContinueUserActivity` 재정의 메서드는 앱은 나가는 호출을 처리 하는 경우 사용 됩니다. 자세한 내용은 참조 하십시오 합니다 [나가는 호출 처리](#handling-outgoing-calls) 섹션 아래.

## <a name="handling-incoming-calls"></a>들어오는 호출 처리

일부의 상태와 들어오는 VOIP 호출을 거칠 수 있는 일반적인 들어오는 호출 워크플로 중에 같은 프로세스:

- 들어오는 호출의 존재는 사용자 (및 시스템) 알리는 합니다.
- 사용자가 다른 사용자를 사용 하 여 호출을 초기화 하는 호출에 응답할 때 알림을 수신 합니다.
- 사용자가 현재 통화를 종료 하려고 할 때 시스템 및 통신 네트워크를 알립니다.

다음 섹션에서는 앱 CallKit을 사용 하 여 다시 MonkeyCall VOIP 앱을 사용 하 여 예를 들어 들어오는 호출 워크플로 처리 하는 방법에 대해 자세하게 걸립니다.

### <a name="informing-user-of-incoming-call"></a>들어오는 호출의 사용자에 게 알리는

원격 사용자는 로컬 사용자와 VOIP 대화를 시작 하는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit05.png "원격 사용자 VOIP 대화 시작")](callkit-images/callkit05.png#lightbox)

1. 앱 알림이 통신 네트워크에서 들어오는 VOIP 호출을 인지 합니다.
2. 앱에서는 합니다 `CXProvider` 보낼를 `CXCallUpdate` 시스템 호출의 알립니다.
3. 시스템의 시스템 UI, 시스템 서비스 및 CallKit를 사용 하 여 다른 VOIP 앱에 대 한 호출을 게시 합니다.

예를 들어,는 `CXProviderDelegate`:

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

이 코드에서는 새 `CXCallUpdate` 인스턴스 하 고 호출자를 식별 하는 것에 대 한 핸들을 연결 합니다. 사용 하 여 다음으로 `ReportNewIncomingCall` 메서드는 `CXProvider` 호출의 시스템에 알림을 보내야 하는 클래스입니다. 호출을 추가 성공한 경우 활성 호출 응용 프로그램의 컬렉션에 유효 하지 않은 경우 오류 해야 사용자에 게 보고 합니다.

### <a name="user-answering-incoming-call"></a>사용자 응답 들어오는 호출

사용자를 들어오는 VOIP 호출에 응답 하려는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit06.png "사용자는 들어오는 VOIP 호출에 응답")](callkit-images/callkit06.png#lightbox)

1. 시스템 UI를 사용자가 VOIP 호출에 응답 하는 시스템에 알립니다.
2. 시스템은 보냅니다를 `CXAnswerCallAction` 앱의 `CXProvider` 응답 의도 알립니다.
3. 앱에 게 알리고 해당 통신 네트워크 사용자가 전화를 응답 VOIP 호출을 정상적으로 진행 됩니다.

예를 들어,는 `CXProviderDelegate`:

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

이 코드는 먼저 해당 활성 호출 목록에 지정 된 호출에 대 한 검색합니다. 호출을 찾을 수 없는 시스템 알림이 전송 되 고 메서드를 종료 합니다. 가 있을 경우 합니다 `AnswerCall` 메서드를 `ActiveCall` 클래스를 호출 하려면 호출 되 고 성공 하거나 실패 하는 경우 시스템은 정보입니다.

### <a name="user-ending-incoming-call"></a>들어오는 호출을 종료 하는 사용자

앱의 UI 내에서 호출을 종료 하려는 경우 결과 다음과 같습니다.

[![](callkit-images/callkit07.png "사용자가 앱의 UI 내에서 호출을 종료")](callkit-images/callkit07.png#lightbox)

1. 앱을 만듭니다 `CXEndCallAction` 에 번들로 제공 가져옵니다는 `CXTransaction` 호출이 종료 되 고 알리기 위해 시스템에 전송 되는 합니다.
2. 시스템 종료 호출 의도 확인 하 고 전송 합니다 `CXEndCallAction` 를 통해 앱으로 다시는 `CXProvider`합니다.
3. 앱 지워졌다는 통신 네트워크 호출이 종료 됩니다.

예를 들어,는 `CXProviderDelegate`:

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

이 코드는 먼저 해당 활성 호출 목록에 지정 된 호출에 대 한 검색합니다. 호출을 찾을 수 없는 시스템 알림이 전송 되 고 메서드를 종료 합니다. 가 있을 경우 합니다 `EndCall` 메서드는 `ActiveCall` 클래스는 통화를 종료 하려면 호출 되 고 성공 또는 실패 하는 경우 시스템은 정보. 성공 하면 호출 활성 호출의 컬렉션에서 제거 됩니다.

## <a name="managing-multiple-calls"></a>여러 호출 관리

대부분의 VOIP 앱은 여러 호출을 한 번에 처리할 수 있습니다. 현재 활성 VOIP 호출 하 고는 새 들어오는 호출을 사용자는 앱을 가져옵니다 알림 일시 중지할 수 있습니다 하는 경우와 같이 또는 두 번째 응답할 첫 번째 호출에서 끊기에 대 한

위의 상황 지정 시스템에 게 전송 합니다는 `CXTransaction` 여러 동작의 목록을 포함 하는 앱에 (같은 합니다 `CXEndCallAction` 및 `CXAnswerCallAction`). 이러한 모든 작업은 충족 해야 개별적으로 시스템 UI를 적절 하 게 업데이트할 수 있도록 합니다.

## <a name="handling-outgoing-calls"></a>호출 나가는 처리

사용자 (Phone 앱)에 최근 목록에서 항목을 탭 하는 경우 예를 들어, 앱에 속하는 호출에서 이것이 보내집니다는 _호출 의도 시작_ 시스템에서:

[![](callkit-images/callkit08.png "시작 호출 의도 수신합니다.")](callkit-images/callkit08.png#lightbox)

1. 앱을 만듭니다는 _작업 호출 시작_ 시스템에서 받은 시작 호출 의도에 따라 합니다. 
2. 앱은 사용의 `CXCallController` 시스템에서 시작 호출 작업을 요청 합니다.
3. 반환을 통해 앱에는 시스템에서 작업을 허용 하는 경우는 `XCProvider` 위임 합니다.
4. 앱은 통신 네트워크를 사용 하 여 발신 전화를 시작합니다.

의도에 대 한 자세한 내용은 참조 하십시오 우리의 [인 텐트와 인 텐트 UI 확장](~/ios/platform/sirikit/understanding-sirikit.md) 설명서. 

### <a name="the-outgoing-call-lifecycle"></a>나가는 호출 수명 주기

CallKit 및 나가는 호출을 사용 하는 경우 앱의 수명 주기 이벤트는 다음 시스템을 알려줍니다 해야 합니다.

1. **시작** -발신 전화를 시작 하는 시스템을 알려줍니다.
2. **시작** -발신 전화를 시작 하는 시스템을 알려줍니다.
3. **연결** -나가는 호출이 연결 된 시스템을 알려줍니다.
4. **연결 된** -알리고 나가는 호출에 연결 하는 양 당사자 이제 통신할 수 있습니다.

예를 들어, 다음 코드는 나가는 호출을 시작 됩니다.

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

만듭니다는 `CXHandle` 구성를 사용 하 여는 `CXStartCallAction` 에 제공 되는 `CXTransaction` 즉 시스템 사용 하 여 전송를 `RequestTransaction` 메서드의 `CXCallController` 클래스. 호출 하 여는 `RequestTransaction` 메서드를 시스템 소스에 관계 없이 모든 기존 호출에서-대기를 배치할 수 있습니다 (FaceTime, VOIP 전화 앱 등) 새 호출을 시작 하기 전에, 합니다.

나가는 VOIP 호출을 시작 하기 위한 요청 (연락처 앱)에서 연락처 카드에 대 한 항목 Siri 등의 여러 다른 원본에서 또는 Phone 앱) (의 최근 목록에서 가져올 수 있습니다. 이러한 상황에서는 앱 전송 시작 호출 여부는 내부를 `NSUserActivity` AppDelegate 처리 해야 합니다.

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

여기를 `CallHandleFromActivity` 도우미 클래스의 메서드 `StartCallRequest` 호출 되는 사용자에 핸들을 가져오는 데 사용 되 고 됩니다 (참조 [StartCallRequest 클래스](#the-startcallrequest-class) 위에).

합니다 `PerformStartCallAction` 메서드를 [ProviderDelegate 클래스](#the-providerdelegate-class) 마지막 실제 보내는 호출을 시작 하 고 수명 주기의 체제에 알려주는 데:

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

인스턴스를 만들고 여 `ActiveCall` 클래스 (진행 중인 호출에 대 한 정보를 포함) 및 호출 되는 사용자를 채웁니다. 합니다 `StartingConnectionChanged` 고 `ConnectedChanged` 이벤트 모니터링 하 고 나가는 호출 수명 주기를 보고 하는 데 사용 됩니다. 호출을 시작 하 고 시스템 작업이 수행 되었음을 알림을 받지 키를 누릅니다.

### <a name="ending-an-outgoing-call"></a>나가는 호출을 종료합니다.

사용자 종료 하고자 및 발신 전화를 사용 하 여 완료 되 면 다음 코드를 사용할 수 있습니다.

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

경우 만듭니다를 `CXEndCallAction` 종료에 대 한 호출의 UUID를 사용 하 여 번들에 `CXTransaction` 즉 시스템 사용 하 여 전송 합니다 `RequestTransaction` 메서드의 `CXCallController` 클래스. 

## <a name="additional-callkit-details"></a>추가 CallKit 세부 정보

이 섹션에서는 개발자와 같은 CallKit 작업할 때 고려해 야 해야 하는 몇 가지 추가 세부 정보를 설명 합니다.

- 공급자 구성
- 작업 오류
- 시스템 제한
- VOIP 오디오

### <a name="provider-configuration"></a>공급자 구성

공급자 구성을 통해 경우 네이티브 호출에서 UI) (내부 사용자 환경을 사용자 지정 하는 iOS 10 VOIP 앱 CallKit를 사용 합니다.

앱은 사용자 지정은 설정할 수 있습니다.

- 지역화 된 이름을 표시 합니다.
- 비디오 호출 지원을 사용 하도록 설정 합니다.
- 고유한 템플릿 이미지 아이콘을 제공 하 여 호출에 UI의 단추를 사용자 지정 합니다. 사용자 지정 단추를 사용 하 여 사용자 상호 작용은 처리할 앱에 직접 전송 됩니다. 

### <a name="action-errors"></a>작업 오류

iOS 10 VOIP 앱 CallKit를 사용 하 여 정상적으로 실패 한 작업을 처리 하 고 사용자에 모든 시간에 게 작업 상태를 유지 해야 합니다. 

다음 예제를 고려해 야 합니다.

1. 앱에 시작 하는 호출 작업 받고에서 통신 네트워크를 사용 하 여 새 VOIP 호출을 초기화 하는 프로세스를 시작 합니다.
2. 제한 되거나 없는 네트워크 통신 기능으로 인해이 연결이 실패합니다.
3. 앱 *해야 합니다* 보낼 합니다 **실패** 시작 호출 작업으로 메시지 (`Action.Fail()`) 오류의 시스템에 알림을 보내야 합니다.
4. 이 통해 호출의 상태를 알리기 위해 시스템입니다. 예를 들어 호출 실패 UI를 표시 합니다.

또한 iOS 10 VOIP 앱 해야 응답할 _시간 초과 오류_ 예상된 작업 지정된 된 기간 내에서 처리할 수 없는 경우 발생할 수 있는 합니다. CallKit 제공한 각 작업 유형에 연결 된 최대 시간 제한 값이 있습니다. 이러한 시간 제한 값에는 사용자가 요청한 CallKit 작업에서에서 처리 되는 응답성이 뛰어난 방식으로 유지할 OS 유동성 및 응답성이 확인 합니다.

공급자 대리자에는 여러 가지가 있습니다 (`CXProviderDelegate`)를 정상적으로이 시간 초과 상황을 처리 하는 재정의 해야 합니다.

### <a name="system-restrictions"></a>시스템 제한

IOS 10 VOIP 앱을 실행 하는 iOS 장치의 현재 상태에 따라 시스템 제한이 적용 될 수 있습니다.

예를 들어, 들어오는 VOIP 호출 하는 경우 시스템에서 제한할 수 있습니다.

1. 호출 하는 사용자가 사용자의 차단 호출자 목록입니다.
2. 사용자의 iOS 장치 Do Not 방해 모드로 설정 됩니다.

VOIP 호출 이러한 상황으로 제한 됩니다, 다음 코드를 사용 하 여 처리.

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

CallKit 라이브 VOIP 호출을 사용 하는 동안 iOS 10 VOIP 앱 해야 하는 오디오 리소스를 처리 하기 위한 몇 가지 이점이 있습니다. 앱의 가장 큰 이점 중 하나는 오디오 세션은 더 높은 우선 순위 iOS 10에서에서 실행 하는 경우. 동일한 우선 순위 수준 전화에서 작성된 되며 FaceTime 앱 및이 향상 된 우선 순위 수준을 VOIP 앱의 오디오 세션을 중단에서 다른 앱을 실행 하지 것입니다.

또한 CallKit 성능을 향상 시킬 하 고 특정 출력 장치에 사용자 기본 설정 및 장치 상태에 따라 라이브 호출 하는 동안 VOIP 오디오를 지능적으로 라우팅할 수 있는 다른 오디오 라우팅 힌트에 액세스할 수 있습니다. 예를 들어 라이브 CarPlay 연결 또는 내게 필요한 옵션 설정 bluetooth 헤드폰 등 연결 된 장치 기반으로 합니다.

일반적인 VOIP의 수명 주기 동안 CallKit를 사용 하 여 호출, 앱 CallKit가 제공 하는 오디오 Stream을 구성 해야 합니다. 다음 예제를 살펴보십시오.

[![](callkit-images/callkit09.png "시작 호출 작업 시퀀스")](callkit-images/callkit09.png#lightbox)

1. 시작 하는 호출 작업을 들어오는 호출에 응답 하도록 앱에서 수신 됩니다.
2. 이 작업은 앱에 의해 충족 됩니다, 전에 구성에 대 한 필요 제공 해당 `AVAudioSession`합니다.
3. 앱 작업에 의해 시스템에 알립니다.
4. CallKit 제공을 우선 순위가 높은 호출에 연결 하기 전에 `AVAudioSession` 앱이 요청한 구성을 일치 합니다. 앱을 통해 알려드립니다 합니다 `DidActivateAudioSession` 메서드의 해당 `CXProviderDelegate`합니다.

## <a name="working-with-call-directory-extensions"></a>통화 디렉터리 확장을 사용 하 여 작업

CallKit를 작업할 때 _디렉터리 확장 호출_ 차단 된 호출 번호를 추가 하 고 iOS 장치에서 연락처 앱에 있는 연락처를 지정된 된 VOIP 앱 관련 된 번호를 식별 하는 방법을 제공 합니다.

### <a name="implementing-a-call-directory-extension"></a>통화 디렉터리 확장을 구현합니다.

Xamarin.iOS 앱에서 통화 디렉터리 확장을 구현 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 용 Visual Studio에서 앱의 솔루션을 열으십시오
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **디렉터리 확장 호출** 을 클릭 합니다 **다음** 단추: 

    [![](callkit-images/calldir01.png "새 통화 디렉터리 확장을 만들기")](callkit-images/calldir01.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **다음** 단추: 

    [![](callkit-images/calldir02.png "확장의 이름을 입력합니다.")](callkit-images/calldir02.png#lightbox)
5. 조정 합니다 **프로젝트 이름** 및/또는 **솔루션 이름** 필요 하 고 클릭 합니다 **만들기** 단추: 

    [![](callkit-images/calldir03.png "프로젝트 만들기")](callkit-images/calldir03.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 앱의 솔루션을 엽니다.
2. 솔루션 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 프로젝트 추가**합니다.
3. 선택 **iOS** > **확장** > **디렉터리 확장 호출** 을 클릭 합니다 **다음** 단추: 

    [![](callkit-images/calldir01w.png "새 통화 디렉터리 확장을 만들기")](callkit-images/calldir01.png#lightbox)
4. 입력을 **이름을** 확장을 클릭 합니다 **확인** 단추

-----

이 추가 `CallDirectoryHandler.cs` 다음과 같이 프로젝트에 클래스:

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

`BeginRequest` 디렉터리 호출 처리기에서 메서드를 필요한 기능을 제공 하도록 수정 해야 합니다. 위의 샘플의 경우 VOIP 앱의 연락처 데이터베이스에서 차단 되 고 사용 가능한 번호 목록이 설정 하려고 합니다. 어떤 이유로 든 실패를 요청 하거나 만듭니다는 `NSError` 오류를 설명 하 고 전달 하는 `CancelRequest` 메서드를 `CXCallDirectoryExtensionContext` 클래스입니다.

차단 된 번호 사용을 설정 하는 `AddBlockingEntry` 메서드는 `CXCallDirectoryExtensionContext` 클래스입니다. 메서드에 제공 된 번호 _해야_ 숫자로 오름차순 이어야 합니다. 최적의 성능 및 메모리 사용량에 대 한 경우 여러 전화 번호, 고려만 지정된 된 시간에 숫자의 하위 집합을 로드 하 고 로드 되는 숫자의 각 일괄 처리 하는 동안 할당 된 개체를 해제 하려면 autorelease 풀을 사용 하 여 합니다.

VOIP 앱에 알려진 연락처 번호의 연락처 앱에 알리기 위해을 사용 합니다 `AddIdentificationEntry` 메서드의 `CXCallDirectoryExtensionContext` 클래스 및 수와를 식별 하는 레이블을 제공 합니다. 메서드에 제공 된 번호를 다시 _해야_ 숫자로 오름차순 이어야 합니다. 최적의 성능 및 메모리 사용량에 대 한 경우 여러 전화 번호, 고려만 지정된 된 시간에 숫자의 하위 집합을 로드 하 고 로드 되는 숫자의 각 일괄 처리 하는 동안 할당 된 개체를 해제 하려면 autorelease 풀을 사용 하 여 합니다.

## <a name="summary"></a>요약

이 문서에 새 CallKit API는 Apple iOS 10 및 Xamarin.iOS VOIP 앱에서이 구현 하는 방법의 출시를 설명 합니다. 표시에 CallKit iOS 시스템에 통합할 앱을 허용 하는 방법, 기본 제공 앱 (예: 휴대폰)를 사용 하 여 기능 패리티를 제공 하는 방법을 고 Siri 상호 작용 및 통해 잠금 및 홈 화면 같은 위치에는 iOS에서 앱의 가시성 증가 하는 방법 연락처 앱입니다.

## <a name="related-links"></a>관련 링크

- [iOS 10 샘플](https://developer.xamarin.com/samples/ios/iOS10/)
