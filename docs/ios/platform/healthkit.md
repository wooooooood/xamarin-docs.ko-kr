---
title: Xamarin.ios의 HealthKit
description: 이 문서에서는 상태 관련 정보에 대 한 중앙 집중화 되 고 조정 된 보안 데이터 저장소를 제공 하는 iOS 8에 도입 된 HealthKit을 설명 합니다. HealthKit 앱을 프로 비전 하는 방법 및 HealthKit 프레임 워크를 사용 하는 코드를 작성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E3927A21-507C-43BA-A2AD-957716BA9B52
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 8090cb3c694083be4ef12294799d6aadf26b6038
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569129"
---
# <a name="healthkit-in-xamarinios"></a>Xamarin.ios의 HealthKit

상태 키트는 사용자의 상태 관련 정보에 대 한 보안 데이터 저장소를 제공 합니다. 상태 키트 앱은 사용자의 명시적 사용 권한을 사용 하 여이 데이터 저장소에 대 한 읽기 및 쓰기를 제공 하 고 관련 데이터가 추가 될 때 알림을 받을 수 있습니다. 앱에서 데이터를 제공 하거나, 사용자가 Apple의 제공 된 상태 앱을 사용 하 여 모든 데이터의 대시보드를 볼 수 있습니다.

상태 관련 데이터는 매우 중요 하 고 중요 하므로 상태 키트는 측정 단위와 기록 되는 정보 유형 (예를 들어 블러드 glucose level 또는 하트 요금)과의 명시적 연결을 사용 하 여 강력한 형식입니다. 또한 상태 키트 앱은 명시적 자격을 사용 해야 하 고, 특정 유형의 정보에 대 한 액세스를 요청 해야 하며, 사용자는 이러한 데이터 유형에 대 한 액세스 권한을 앱에 명시적으로 부여 해야 합니다.

이 문서에서는 다음을 소개 합니다.

- 상태 키트 데이터베이스에 액세스 하기 위한 응용 프로그램 프로 비전 및 사용자 권한 요청을 비롯 한 상태 키트의 보안 요구 사항
- 상태 키트의 유형 시스템-잘못 된 적용 또는 misinterpreting 데이터의 가능성을 최소화 합니다.
- 시스템 차원의 공유 상태 키트 데이터 저장소에 기록 합니다.

이 문서에서는 데이터베이스 쿼리, 측정 단위 간 변환 또는 새 데이터 알림 수신을 같은 고급 항목에 대해 다루지 않습니다.

이 문서에서는 사용자의 하트 요금을 기록 하는 샘플 응용 프로그램을 만듭니다.

[![](healthkit-images/image01.png "A sample application to record the users heart rate")](healthkit-images/image01.png#lightbox)

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 필요 합니다.

- **Xcode 7 및 ios 8 이상** – Apple의 최신 Xcode 및 ios api를 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **Mac용 Visual Studio 또는 Visual Studio** – 최신 버전의 Mac용 Visual Studio를 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **ios 8 이상 장치** – 테스트를 위해 최신 버전의 ios 8 이상을 실행 하는 ios 장치입니다.

> [!IMPORTANT]
> 상태 키트는 iOS 8에서 도입 되었습니다. 현재는 상태 키트를 iOS 시뮬레이터에서 사용할 수 없으며, 디버깅 하려면 실제 iOS 장치에 연결 해야 합니다.

## <a name="creating-and-provisioning-a-health-kit-app"></a>상태 키트 앱 만들기 및 프로 비전
Xamarin iOS 8 응용 프로그램은 HealthKit API를 사용할 수 있기 전에 적절 하 게 구성 하 고 프로 비전 해야 합니다. 이 섹션에서는 Xamarin 응용 프로그램을 올바르게 설정 하는 데 필요한 단계를 다룹니다.

상태 키트 앱에는 다음이 필요 합니다.

- 명시적인 **앱 ID**입니다.
- 해당 명시적인 **앱 ID** 및 **상태 키트** 사용 권한과 연결 된 **프로 비전 프로필** 입니다.
- `Entitlements.plist` `com.apple.developer.healthkit` 형식의 속성이로 설정 된 `Boolean` `Yes` 입니다.
- `Info.plist`키에 `UIRequiredDeviceCapabilities` 값이 포함 된 항목이 들어 `String` 있는 `healthkit` 입니다.
- 또한에는 해당 하는 `Info.plist` 개인 정보 취급 방침 항목이 있어야 합니다. `String` 앱에서 데이터를 쓰려고 하는 경우 키에 대 한 설명과 `NSHealthUpdateUsageDescription` `String` `NSHealthShareUsageDescription` 앱에서 상태 키트 데이터를 읽을 경우 키에 대 한 설명이 있어야 합니다.

IOS 앱을 프로 비전 하는 방법에 대 한 자세한 내용을 보려면 Xamarin **시작** 시리즈의 [장치 프로 비전](~/ios/get-started/installation/device-provisioning/index.md) 문서에서 개발자 인증서, 앱 Id, 프로 비전 프로필 및 앱 자격 간의 관계를 설명 합니다.

<a name="explicit-appid"></a>

### <a name="explicit-app-id-and-provisioning-profile"></a>명시적 앱 ID 및 프로 비전 프로필

명시적인 **앱 ID** 및 적절 한 **프로 비전 프로필** 생성은 Apple의 [iOS 개발자 센터](https://developer.apple.com/devcenter/ios/index.action)내에서 수행 됩니다. 

현재 **앱 id** 는 개발자 센터의 [인증서, 식별자 & 프로필](https://developer.apple.com/account/ios/identifiers/bundle/bundleList.action) 섹션에 나열 됩니다. 일반적으로이 목록에는 **ID** `*` **앱 id**  -  **이름을** 임의의 수의 접미사로 사용할 수 있음을 나타내는의 id 값이 표시 됩니다. 이러한 *와일드 카드 앱 id* 는 상태 키트와 함께 사용할 수 없습니다.

명시적인 **앱 id**를 만들려면 **+** 오른쪽 위에 있는 단추를 클릭 하 여 **iOS 앱 id 등록** 페이지로 이동 합니다.

[![](healthkit-images/image02.png "Registering an app on the Apple Developer Portal")](healthkit-images/image02.png#lightbox)

위의 그림에 나와 있는 것 처럼 앱 설명을 만든 후 **Explicit 앱 id** 섹션을 사용 하 여 응용 프로그램에 대 한 id를 만듭니다. **App Services** 섹션의 **서비스 사용** 섹션에서 **Health Kit** 를 선택 합니다.

완료 되 면 **계속** 단추를 눌러 계정에 **앱 ID** 를 등록 합니다. **인증서, 식별자 및 프로필** 페이지로 돌아갑니다. 프로 **비전 프로필** 을 클릭 하 여 현재 프로 비전 프로필의 목록으로 이동 하 고 **+** 오른쪽 위 모퉁이에 있는 단추를 클릭 하 여 **IOS 프로 비전 프로필 추가** 페이지로 이동 합니다. **IOS 앱 개발** 옵션을 선택 하 고 **계속** 을 클릭 하 여 **앱 ID 선택** 페이지로 이동 합니다. 여기에서 이전에 지정한 명시적 **앱 ID** 를 선택 합니다.

[![](healthkit-images/image03.png "Select the explicit App ID")](healthkit-images/image03.png#lightbox)

**계속** 을 클릭 하 고 나머지 화면에서 작업 합니다. 여기에서 **개발자 인증서**, **장치**및이 **프로 비전 프로필**에 대 한 **이름을** 지정 합니다.

[![](healthkit-images/image04.png "Generating the Provisioning Profile")](healthkit-images/image04.png#lightbox)

**생성** 을 클릭 하 고 프로필 만들기를 기다립니다. 파일을 다운로드 하 고 두 번 클릭 하 여 Xcode에 설치 합니다. **Xcode > 기본 설정 > 계정 > 세부 정보 보기** ...에서 설치를 확인할 수 있습니다. 방금 설치 된 프로 비전 프로필이 표시 되 고, 해당 **자격** 에 대 한 상태 키트 및 기타 특수 서비스 아이콘이 있어야 합니다.

[![](healthkit-images/image05.png "Viewing the profile in Xcode")](healthkit-images/image05.png#lightbox)

<a name="associating-appid"></a>

### <a name="associating-the-app-id-and-provisioning-profile-with-your-xamarinios-app"></a>앱 ID 및 프로 비전 프로필을 Xamarin.ios 앱에 연결

설명 된 대로 적절 한 **프로 비전 프로필** 을 만들고 설치한 후에는 일반적으로 Mac용 Visual Studio 또는 Visual Studio에서 솔루션을 만들 수 있습니다. 상태 키트 액세스는 모든 iOS c # 또는 F # 프로젝트에서 사용할 수 있습니다.

Xamarin iOS 8 프로젝트를 직접 만드는 프로세스를 안내 하는 대신이 문서에 연결 된 샘플 앱 (미리 작성 된 스토리 보드 및 코드 포함)을 엽니다. 샘플 앱을 상태 키트 사용 **프로 비전 프로필**에 연결 하려면 **Solution Pad**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 해당 **옵션** 대화 상자를 엽니다. **IOS 응용 프로그램** 패널로 전환 하 고 이전에 앱의 **번들 식별자**로 만든 명시적 **앱 ID** 를 입력 합니다.

[![](healthkit-images/image06.png "Enter the explicit App ID")](healthkit-images/image06.png#lightbox)

이제 **IOS 번들 서명** 패널로 전환 합니다. 명시적 **앱 ID**와 연결 된 최근 설치 된 **프로 비전 프로필**은 이제 **프로 비전 프로필로**사용할 수 있습니다.

[![](healthkit-images/image07.png "Select the Provisioning Profile")](healthkit-images/image07.png#lightbox)

**프로 비전 프로필** 을 사용할 수 없는 경우 Ios **응용 프로그램** 패널의 **번들 식별자** 와 **Ios 개발자 센터** 에 지정 된 번들 식별자를 두 번 확인 하 고 **프로 비전 프로필이** 설치 되었는지 확인 합니다 (**Xcode > 기본 설정 > 계정 > 보기**...).

상태 키트 사용 **프로 비전 프로필** 을 선택 하면 **확인** 을 클릭 하 여 프로젝트 옵션 대화 상자를 닫습니다.

### <a name="entitlementsplist-and-infoplist-values"></a>Info.plist 및 info.plist 값

샘플 앱에는 `Entitlements.plist` 모든 프로젝트 템플릿에 포함 되지 않은 파일 (상태 키트 사용 앱에 필요)이 포함 되어 있습니다. 프로젝트에 자격을 포함 하지 않는 경우 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **파일 > 새 파일 > iOS > info.plist** 를 선택 하 여 수동으로 추가 합니다.

궁극적으로는 `Entitlements.plist` 다음의 키 및 값 쌍이 있어야 합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.HealthKit</key>
    <true/>
</dict>
</plist>

```

마찬가지로, 앱의에는 `Info.plist` `healthkit` 키와 연결 된 값이 있어야 합니다 `UIRequiredDeviceCapabilities` .

```xml
<key>UIRequiredDeviceCapabilities</key>
<array>
<string>armv7</string>
    <string>healthkit</string>
</array>

```

이 문서와 함께 제공 되는 샘플 응용 프로그램에는 `Entitlements.plist` 필요한 모든 키를 포함 하는 미리 구성 된이 포함 되어 있습니다.

<a name="programming"></a>

## <a name="programming-health-kit"></a>프로그래밍 상태 키트

상태 키트 데이터 저장소는 응용 프로그램 간에 공유 되는 개인 사용자 관련 데이터 저장소입니다. 상태 정보는 매우 중요 하기 때문에 사용자는 데이터 액세스를 허용 하는 긍정 단계를 수행 해야 합니다. 이 액세스는 부분적 (쓰기는 가능 하지만 읽기는 불가능 하 고 일부 데이터 형식에 대 한 액세스는 아님)이 될 수 있으며 언제 든 지 취소할 수 있습니다. 상태 키트 응용 프로그램은 defensively로 작성 해야 하며,이는 많은 사용자가 상태 관련 정보를 저장 하는 것에 대 한 망설를 이해 합니다.

상태 키트 데이터는 Apple 지정 형식으로 제한 됩니다. 이러한 형식은 엄격 하 게 정의 되어 있습니다. 예를 들어 블러드 형식과 같은 일부는 Apple에서 제공 하는 열거형의 특정 값으로 제한 되 고 다른 값은 크기를 측정 단위 (예: 그램, calories, 리터)와 결합 합니다. 호환 되는 측정 단위를 공유 하는 데이터도에 의해 구분 됩니다 `HKObjectType` . 예를 들어 형식 시스템은 `HKQuantityTypeIdentifier.NumberOfTimesFallen` `HKQuantityTypeIdentifier.FlightsClimbed` 두 가지 모두 `HKUnit.Count` 측정 단위를 사용 하는 경우에도가 필요한 필드에 값을 저장 하려는 잘못 된 시도를 catch 합니다.

상태 키트 데이터 저장소의 저장할 형식은의 모든 서브 클래스입니다 `HKObjectType` . `HKCharacteristicType`개체는 생물학적 성, 혈압 유형 및 생년월일을 저장 합니다. 보다 일반적으로는 `HKSampleType` 특정 시간 또는 일정 기간 동안 샘플링 되는 데이터를 나타내는 개체입니다. 

[![](healthkit-images/image08.png "HKSampleType objects chart")](healthkit-images/image08.png#lightbox)

`HKSampleType`는 추상적 이며 4 개의 구체적 하위 클래스를 포함 합니다. 현재 `HKCategoryType` 절전 모드 분석 인 데이터 유형은 하나 뿐입니다. 상태 키트의 대부분의 데이터는 형식이 `HKQuantityType` 며 `HKQuantitySample` , 친숙 한 팩터리 디자인 패턴을 사용 하 여 만든 개체에 데이터를 저장 합니다.

[![](healthkit-images/image09.png "The large majority of data in Health Kit are of type HKQuantityType and store their data in HKQuantitySample objects")](healthkit-images/image09.png#lightbox)

`HKQuantityType`형식은에서 사이 `HKQuantityTypeIdentifier.ActiveEnergyBurned` `HKQuantityTypeIdentifier.StepCount` 입니다. 

<a name="requesting-permission"></a>

### <a name="requesting-permission-from-the-user"></a>사용자의 권한 요청

최종 사용자는 앱에서 상태 키트 데이터를 읽거나 쓸 수 있도록 긍정적인 단계를 수행 해야 합니다. 이 작업은 iOS 8 장치에 미리 설치 된 상태 앱을 통해 수행 됩니다. 상태 키트 앱을 처음 실행할 때 사용자에 게 시스템 제어 **상태 액세스** 대화 상자가 표시 됩니다.

[![](healthkit-images/image10.png "The user is presented with a system-controlled Health Access dialog")](healthkit-images/image10.png#lightbox)

나중에 사용자는 상태 앱의 **소스** 대화 상자를 사용 하 여 사용 권한을 변경할 수 있습니다.

[![](healthkit-images/image11.png "The user can change permissions using Health apps Sources dialog")](healthkit-images/image11.png#lightbox)

상태 정보는 매우 중요 하므로 앱 개발자는 앱을 실행 하는 동안 사용 권한이 거부 및 변경 될 것 이란 가정 하에 defensively 프로그램을 작성 해야 합니다. 가장 일반적인 방법은 메서드에서 권한을 요청 하 고 사용자 인터페이스를 적절 하 게 수정 하는 것입니다 `UIApplicationDelegate.OnActivated` .

### <a name="permissions-walkthrough"></a>권한 연습

상태 키트 프로 비전 된 프로젝트에서 파일을 엽니다 `AppDelegate.cs` . 파일의 맨 위에 있는를 사용 하 여 문을 확인 합니다 `HealthKit` .

다음 코드는 상태 키트 권한과 관련이 있습니다.

```csharp
private HKHealthStore healthKitStore = new HKHealthStore ();

public override void OnActivated (UIApplication application)
{
        ValidateAuthorization ();
}

private void ValidateAuthorization ()
{
        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
        var heartRateType = HKObjectType.GetQuantityType (heartRateId);
        var typesToWrite = new NSSet (new [] { heartRateType });
        var typesToRead = new NSSet ();
        healthKitStore.RequestAuthorizationToShare (
                typesToWrite, 
                typesToRead, 
                ReactToHealthCarePermissions);
}

void ReactToHealthCarePermissions (bool success, NSError error)
{
        var access = healthKitStore.GetAuthorizationStatus (HKObjectType.GetQuantityType (HKQuantityTypeIdentifierKey.HeartRate));
        if (access.HasFlag (HKAuthorizationStatus.SharingAuthorized)) {
                HeartRateModel.Instance.Enabled = true;
        } else {
                HeartRateModel.Instance.Enabled = false;
        }
}

```

이러한 메서드의 모든 코드는에서 인라인으로 수행 될 수 `OnActivated` 있지만 샘플 앱은 별도의 메서드를 사용 하 여 의도를 명확 하 게 합니다 .는 `ValidateAuthorization()` 작성 되는 특정 형식에 대 한 액세스를 요청 하는 데 필요한 단계 (및 앱을 원하는 경우 읽기)를 사용 하 고 `ReactToHealthCarePermissions()` 사용자가 상태 대화 상자에서 사용 권한 대화 상자와 상호 작용 한 후에 활성화 되는 콜백입니다.

작업은 `ValidateAuthorization()` `HKObjectTypes` 앱이 쓰고 해당 데이터를 업데이트 하기 위한 권한 부여를 요청 하는 집합을 빌드하는 것입니다. 샘플 앱에서는 `HKObjectType` 키에 대 한입니다 `KHQuantityTypeIdentifierKey.HeartRate` . 이 형식은 집합에 추가 되는 `typesToWrite` 반면 집합은 `typesToRead` 빈 상태로 유지 됩니다. 이러한 집합 및 콜백에 대 한 참조는 `ReactToHealthCarePermissions()` 에 전달 됩니다 `HKHealthStore.RequestAuthorizationToShare()` .

사용자가 사용 `ReactToHealthCarePermissions()` 권한 대화 상자와 상호 작용 하 고 `bool` 사용자가 `true` 사용 권한 대화 상자와 상호 작용 하는 경우 값이이 고 `NSError` , null이 아닌 경우에는 사용 권한 대화 상자를 표시 하는 것과 관련 된 일종의 오류를 나타내는 경우 콜백이 호출 됩니다.

> [!IMPORTANT]
> 이 함수에 대 한 인수를 명확 하 게 표시 합니다. _성공_ 및 _오류_ 매개 변수는 사용자에 게 Health Kit 데이터에 액세스할 수 있는 권한을 부여 했는지 여부를 나타내지 않습니다. 사용자에 게 데이터에 대 한 액세스를 허용 하는 기회가 제공 되었다는 것만 표시 됩니다.

앱이 데이터에 액세스할 수 있는지 여부를 확인 하려면를 사용 하 여를 `HKHealthStore.GetAuthorizationStatus()` 전달 `HKQuantityTypeIdentifierKey.HeartRate` 합니다. 반환 된 상태에 따라 앱은 데이터를 입력할 수 있는 기능을 사용 하거나 사용 하지 않도록 설정 합니다. 액세스 거부를 처리 하는 데 사용할 수 있는 표준 사용자 환경은 없으며 여러 가지 옵션을 사용할 수 있습니다. 예제 앱에서 상태는 단일 개체에 대해 설정 되 고, 그 후에는 `HeartRateModel` 관련 이벤트가 발생 합니다.

## <a name="model-view-and-controller"></a>모델, 뷰 및 컨트롤러

Singleton 개체를 검토 하려면 `HeartRateModel` `HeartRateModel.cs` 다음 파일을 엽니다.

```csharp
using System;
using HealthKit;
using Foundation;

namespace HKWork
{
        public class GenericEventArgs<T> : EventArgs
        {
                public T Value { get; protected set; }
                public DateTime Time { get; protected set; }

                public GenericEventArgs (T value)
                {
                        this.Value = value;
                        Time = DateTime.Now;
                }
        }

        public delegate void GenericEventHandler<T> (object sender,GenericEventArgs<T> args);

        public sealed class HeartRateModel : NSObject
        {
                private static volatile HeartRateModel singleton;
                private static object syncRoot = new Object ();

                private HeartRateModel ()
                {
                }

                public static HeartRateModel Instance {
                        get {
                                //Double-check lazy initialization
                                if (singleton == null) {
                                        lock (syncRoot) {
                                                if (singleton == null) {
                                                        singleton = new HeartRateModel ();
                                                }
                                        }
                                }

                                return singleton;
                        }
                }

                private bool enabled = false;

                public event GenericEventHandler<bool> EnabledChanged;
                public event GenericEventHandler<String> ErrorMessageChanged;
                public event GenericEventHandler<Double> HeartRateStored;

                public bool Enabled { 
                        get { return enabled; }
                        set {
                                if (enabled != value) {
                                        enabled = value;
                                        InvokeOnMainThread(() => EnabledChanged (this, new GenericEventArgs<bool>(value)));
                                }
                        }
                }

                public void PermissionsError(string msg)
                {
                        Enabled = false;
                        InvokeOnMainThread(() => ErrorMessageChanged (this, new GenericEventArgs<string>(msg)));
                }

                //Converts its argument into a strongly-typed quantity representing the value in beats-per-minute
                public HKQuantity HeartRateInBeatsPerMinute(ushort beatsPerMinute)
                {
                        var heartRateUnitType = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        var quantity = HKQuantity.FromQuantity (heartRateUnitType, beatsPerMinute);

                        return quantity;
                }
                        
                public void StoreHeartRate(HKQuantity quantity)
                {
                        var bpm = HKUnit.Count.UnitDividedBy (HKUnit.Minute);
                        //Confirm that the value passed in is of a valid type (can be converted to beats-per-minute)
                        if (! quantity.IsCompatible(bpm))
                        {
                                InvokeOnMainThread(() => ErrorMessageChanged(this, new GenericEventArgs<string> ("Units must be compatible with BPM")));
                        }

                        var heartRateId = HKQuantityTypeIdentifierKey.HeartRate;
                        var heartRateQuantityType = HKQuantityType.GetQuantityType (heartRateId);
                        var heartRateSample = HKQuantitySample.FromType (heartRateQuantityType, quantity, new NSDate (), new NSDate (), new HKMetadata());

                        using (var healthKitStore = new HKHealthStore ()) {
                                healthKitStore.SaveObject (heartRateSample, (success, error) => {
                                        InvokeOnMainThread (() => {
                                                if (success) {
                                                        HeartRateStored(this, new GenericEventArgs<Double>(quantity.GetDoubleValue(bpm)));
                                                } else {
                                                        ErrorMessageChanged(this, new GenericEventArgs<string>("Save failed"));
                                                }
                                                if (error != null) {
                                                        //If there's some kind of error, disable 
                                                        Enabled = false;
                                                        ErrorMessageChanged (this, new GenericEventArgs<string>(error.ToString()));
                                                }
                                        });
                                });
                        }
                }
        }
}

```

첫 번째 섹션은 일반 이벤트와 처리기를 만들기 위한 상용구 코드입니다. 클래스의 초기 부분은 `HeartRateModel` 스레드로부터 안전한 singleton 개체를 만들기 위한 상용구 이기도 합니다.

그런 다음은 `HeartRateModel` 3 개의 이벤트를 노출 합니다. 

- `EnabledChanged`-하트 비트 저장소를 사용 하거나 사용 하지 않도록 설정 했음을 나타냅니다 (저장소는 처음에 사용 하지 않도록 설정 됨). 
- `ErrorMessageChanged`-이 샘플 앱의 경우 매우 간단한 오류 처리 모델 (마지막 오류가 있는 문자열)이 있습니다. 
- `HeartRateStored`-하트 비율이 상태 키트 데이터베이스에 저장 될 때 발생 합니다.

이러한 이벤트가 발생 될 때마다 `NSObject.InvokeOnMainThread()` 구독자가 UI를 업데이트할 수 있도록 하는를 통해 수행 됩니다. 또는 이벤트를 백그라운드 스레드에서 발생 하는 것으로 문서화 하 고, 호환성을 유지 하는 책임은 해당 처리기로 유지할 수 있습니다. 스레드 고려 사항은 권한 요청 등의 많은 함수가 비동기 이며 주가 아닌 스레드에서 콜백을 실행 하기 때문에 상태 키트 응용 프로그램에서 중요 합니다.

에서 Heath Kit 관련 코드는 `HeartRateModel` 두 개의 함수 및에 `HeartRateInBeatsPerMinute()` `StoreHeartRate()` 있습니다. 

`HeartRateInBeatsPerMinute()`해당 인수를 강력한 형식의 상태 키트로 변환 `HKQuantity` 합니다. Quantity의 형식은에 의해 지정 되 `HKQuantityTypeIdentifierKey.HeartRate` 고 수량 단위는 `HKUnit.Count` 로 구분 됩니다 `HKUnit.Minute` . 즉, 단위는 *분당 비트*단위입니다. 

함수는를 `StoreHeartRate()` 사용 `HKQuantity` 합니다 (샘플 앱에서는에 의해 만들어짐 `HeartRateInBeatsPerMinute()` ). 해당 데이터의 유효성을 검사 하기 위해 `HKQuantity.IsCompatible()` 메서드를 사용 합니다 .이 메서드는 `true` 개체의 단위를 인수의 단위로 변환할 수 있는지 여부를 반환 합니다. 이를 사용 하 여 수량이 생성 된 경우 `HeartRateInBeatsPerMinute()` 반환 되는 것은 물론, `true` `true` 수량이 생성 된 경우 (예를 들어 *시간당 비트*수)도 반환 됩니다. 보다 일반적으로를 `HKQuantity.IsCompatible()` 사용 하 여 사용자 또는 장치가 하나의 측정 시스템 (예: 왕정 단위)에서 입력 하거나 표시할 수 있지만 다른 시스템 (예: 메트릭 단위)에 저장 될 수 있는 대용량, 거리 및 에너지를 확인할 수 있습니다. 

Quantity의 호환성을 확인 한 후에는 `HKQuantitySample.FromType()` 팩터리 메서드를 사용 하 여 강력한 형식의 개체를 만듭니다 `heartRateSample` . `HKSample`개체에는 시작 날짜와 종료 날짜가 있습니다. 즉각적인 판독값의 경우 이러한 값은 예제와 같이 동일 해야 합니다. 이 샘플은 또한 인수에 키-값 데이터를 설정 하지 `HKMetadata` 않지만 다음 코드와 같은 코드를 사용 하 여 센서 위치를 지정할 수 있습니다.

```csharp
var hkm = new HKMetadata();
hkm.HeartRateSensorLocation = HKHeartRateSensorLocation.Chest;

```

`heartRateSample`가 만들어지면 코드는 using 블록을 사용 하 여 데이터베이스에 대 한 새 연결을 만듭니다. 이 블록 내에서 `HKHealthStore.SaveObject()` 메서드는 데이터베이스에 대 한 비동기 쓰기를 시도 합니다. 람다 식에 대 한 결과 호출은 또는와 관련 이벤트를 `HeartRateStored` 트리거합니다 `ErrorMessageChanged` .

이제 모델을 프로그래밍 했으므로 컨트롤러에서 모델의 상태를 반영 하는 방법을 확인할 수 있습니다. 파일을 엽니다 `HKWorkViewController.cs` . 생성자는 `HeartRateModel` 단일 항목을 이벤트 처리 메서드에 적용 하기만 하면 됩니다 .이 메서드는 람다 식으로 인라인으로 수행 될 수 있지만 별도의 메서드를 사용 하면 좀 더 명확 하 게 만들 수 있습니다.

```csharp
public HKWorkViewController (IntPtr handle) : base (handle)
{
     HeartRateModel.Instance.EnabledChanged += OnEnabledChanged;
     HeartRateModel.Instance.ErrorMessageChanged += OnErrorMessageChanged;
     HeartRateModel.Instance.HeartRateStored += OnHeartBeatStored;
}

```

관련 처리기는 다음과 같습니다.

```csharp
void OnEnabledChanged (object sender, GenericEventArgs<bool> args)
{
        StoreData.Enabled = args.Value;
        PermissionsLabel.Text = args.Value ? "Ready to record" : "Not authorized to store data.";
        PermissionsLabel.SizeToFit ();
}

void OnErrorMessageChanged (object sender, GenericEventArgs<string> args)
{
        PermissionsLabel.Text = args.Value;
}

void OnHeartBeatStored (object sender, GenericEventArgs<double> args)
{
        PermissionsLabel.Text = String.Format ("Stored {0} BPM", args.Value);
}

```

물론, 단일 컨트롤러를 사용 하는 응용 프로그램에서는 별도의 모델 개체를 만들지 않고 제어 흐름에 대 한 이벤트를 사용 하는 것을 방지할 수 있지만, 모델 개체를 사용 하는 것이 실제 응용 프로그램에 더 적합 합니다.

## <a name="running-the-sample-app"></a>샘플 앱 실행

IOS 시뮬레이터는 상태 키트를 지원 하지 않습니다. 디버그는 iOS 8을 실행 하는 물리적 장치에서 수행 해야 합니다.

올바르게 프로 비전 된 iOS 8 개발 장치를 시스템에 연결 합니다. Mac용 Visual Studio에서 배포 대상으로 선택 하 고 메뉴에서 **실행 > 디버그**를 선택 합니다.

> [!IMPORTANT]
> 이 시점에서 프로 비전과 관련 된 실수가 발생 합니다. 오류를 해결 하려면 위의 상태 키트 앱 만들기 및 프로 비전 섹션을 검토 하세요. 구성 요소는 다음과 같습니다. 
>
> - **IOS 개발자 센터** -명시적 앱 ID & Health Kit에서 프로 비전 프로필을 사용 하도록 설정 했습니다. 
> - **프로젝트 옵션** -번들 식별자 (명시적 앱 ID) & 프로 비전 프로필입니다.
> - **소스 코드** -info.plist & info.plist

프로 비전이 올바르게 설정 되었다고 가정 하면 응용 프로그램이 시작 됩니다. 해당 메서드에 도달 하면 `OnActivated` 상태 키트 권한 부여를 요청 합니다. 운영 체제에서이 작업을 처음으로 발생 하면 사용자에 게 다음과 같은 대화 상자가 표시 됩니다.

[![](healthkit-images/image12.png "The user will be presented with this dialog")](healthkit-images/image12.png#lightbox)

앱에서 하트 요금 데이터를 업데이트할 수 있도록 설정 하면 앱이 다시 나타납니다. `ReactToHealthCarePermissions`콜백은 비동기적으로 활성화 됩니다. 이렇게 하면 이벤트가 발생 하 여 이벤트 처리기가 실행 되 고이로 `HeartRateModel’s` `Enabled` `EnabledChanged` 인해 `HKPermissionsViewController.OnEnabledChanged()` 단추가 활성화 됩니다 `StoreData` . 다음 다이어그램에서는이 시퀀스를 보여 줍니다.

[![](healthkit-images/image13.png "This diagram shows the sequence of events")](healthkit-images/image13.png#lightbox)

**기록** 단추를 누릅니다. 이렇게 하면 처리기가 `StoreData_TouchUpInside()` 실행 되어 텍스트 필드의 값을 구문 분석 하 `heartRate` 고, 앞에서 설명한 함수를 통해로 변환 하 `HKQuantity` 고, `HeartRateModel.HeartRateInBeatsPerMinute()` 해당 수량을에 전달 `HeartRateModel.StoreHeartRate()` 합니다. 앞에서 설명한 것 처럼이는 데이터를 저장 하려고 시도 하 고 또는 이벤트를 발생 시킵니다 `HeartRateStored` `ErrorMessageChanged` .

장치에서 **홈** 단추를 두 번 클릭 하 고 상태 앱을 엽니다. **원본** 탭을 클릭 하면 샘플 앱이 표시 됩니다. 이를 선택 하 고 하트 rate 데이터를 업데이트할 수 있는 권한을 허용 하지 않습니다. **홈** 단추를 두 번 클릭 하 고 앱으로 다시 전환 합니다. 다시 한 번 `ReactToHealthCarePermissions()` 호출 되지만 이번에는 액세스가 거부 되었기 때문에 사용자 **데이터** 단추를 사용할 수 없게 됩니다 .이는 비동기적으로 수행 되며 최종 사용자에 게 사용자 인터페이스의 변경 내용이 표시 될 수 있습니다.

## <a name="advanced-topics"></a>고급 항목

상태 키트 데이터베이스에서 데이터를 읽는 것은 데이터를 작성 하는 것과 매우 유사 합니다. 하나는 액세스 하려고 하는 데이터 형식을 지정 하 고, 권한 부여를 요청 하며, 권한이 부여 되 면 데이터를 사용할 수 있으며,이는 호환 되는 측정 단위로 자동으로 변환 됩니다.

관련 데이터가 업데이트 될 때 업데이트를 수행 하는 조건자 기반 쿼리 및 쿼리를 허용 하는 많은 정교한 쿼리 함수가 있습니다. 

상태 키트 응용 프로그램 개발자는 Apple [앱 검토 지침](https://developer.apple.com/app-store/review/guidelines/#healthkit)의 상태 키트 섹션을 검토 해야 합니다.

보안 및 유형 시스템 모델을 이해 하 고 나면 공유 상태 키트 데이터베이스에서 데이터를 저장 하 고 읽는 것은 매우 간단 합니다. 상태 키트 내의 많은 함수는 비동기적으로 작동 하며 응용 프로그램 개발자는 프로그램을 적절 하 게 작성 해야 합니다.

이 문서를 작성할 당시에는 현재 Android 또는 Windows Phone의 상태 키트와 동일 하지 않습니다.

## <a name="summary"></a>요약

이 문서에서는 상태 키트를 사용 하 여 응용 프로그램에서 상태 관련 정보를 저장, 검색 및 공유 하는 동시에 사용자가이 데이터에 액세스 하 고 제어할 수 있는 표준 상태 앱을 제공 하는 방법을 살펴보았습니다. 

또한 상태 키트를 사용 하는 앱은 상태 관련 정보를 재정의 하 고, 상태 키트를 사용 하는 앱에서 응용 프로그램 관리 측면 (프로 비전), 코딩 (상태 키트의 유형 시스템) 및 사용자 환경 (시스템 대화 상자와 상태 앱을 통한 권한 사용자 제어)의 복잡성 증가를 처리 해야 합니다. 

마지막으로, 상태 키트 저장소에 하트 비트 데이터를 기록 하 고 비동기 인식 디자인을 포함 하는 포함 된 샘플 앱을 사용 하 여 간단한 상태 키트 구현을 살펴보세요.

## <a name="related-links"></a>관련 링크

- [HKWork (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-introtohealthkit)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
