---
title: Xamarin.ios의 CloudKit
description: 이 문서에서는 Xamarin.ios에서 CloudKit를 사용 하는 방법을 설명 합니다. CloudKit의 개요를 제공 하 고이를 사용 하도록 설정 하는 방법, CloudKit 편리한 API, 확장성, 사용자 계정, 개발 및 프로덕션 환경에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/11/2016
ms.openlocfilehash: 29ccb919f68a45212bff3b66b4bc3fbdebd24faf
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663462"
---
# <a name="cloudkit-in-xamarinios"></a>Xamarin.ios의 CloudKit

CloudKit 프레임 워크는 iCloud에 액세스 하는 응용 프로그램 개발을 간소화 합니다. 여기에는 응용 프로그램 데이터를 검색 하 고 응용 프로그램 정보를 안전 하 게 저장할 수 있는 권한이 포함 됩니다. 이 키트는 개인 정보를 공유 하지 않고 iCloud Id로 응용 프로그램에 대 한 액세스를 허용 하 여 사용자에 게 익명의 계층을 제공 합니다.

개발자는 클라이언트 쪽 응용 프로그램에 집중 하 고 iCloud를 사용 하 여 서버 쪽 응용 프로그램 논리를 작성할 필요가 없습니다. CloudKit는 인증, 개인 및 공용 데이터베이스, 구조화 된 데이터 및 asset storage 서비스를 제공 합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 필요 합니다.

- **Xcode 및 IOS SDK** – Apple의 Xcode 및 Ios 8 api는 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **Mac용 Visual Studio** – 최신 버전의 Mac용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
- **ios 8 장치** – 테스트를 위해 최신 버전의 ios 8을 실행 하는 ios 장치입니다.

## <a name="what-is-cloudkit"></a>CloudKit 이란?

CloudKit는 개발자에 게 iCloud 서버에 대 한 액세스 권한을 제공 하는 방법입니다. ICloud 드라이브 및 iCloud 사진 라이브러리의 기반을 제공 합니다. CloudKit는 macOS와 iOS 장치 모두에서 지원 됩니다.

[macOS와 iOS 장치 둘 다에서 CloudKit를 지 원하는 방법 ![](intro-to-cloudkit-images/image1.png)](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit는 iCloud 계정 인프라를 사용 합니다. 장치에서 iCloud 계정에 로그인 한 사용자가 있는 경우 CloudKit는 해당 ID를 사용 하 여 사용자를 식별 합니다. 사용할 수 있는 계정이 없으면 제한 된 읽기 전용 액세스 권한이 제공 됩니다.

CloudKit는 공용 및 개인 데이터베이스의 개념을 모두 지원 합니다. 공용 데이터베이스는 사용자가 액세스할 수 있는 모든 데이터의 "수프"을 제공 합니다. 전용 데이터베이스는 특정 사용자에 게 바인딩된 개인 데이터를 저장 하기 위한 것입니다.

CloudKit는 구조화 된 데이터와 대량 데이터를 모두 지원 합니다. 대량 파일 전송을 원활 하 게 처리할 수 있습니다. CloudKit은 백그라운드에서 많은 파일을 효율적으로 또는 iCloud 서버에서 효율적으로 전송 하므로 개발자가 다른 작업에 집중할 수 있습니다.

> [!NOTE]
> CloudKit는 _전송 기술_입니다. 지 속성을 제공 하지 않습니다. 응용 프로그램은 서버에서 효율적으로 정보를 보내고 받을 수 있습니다.

이 문서를 작성할 당시에는 Apple에서 처음에는 대역폭 및 저장소 용량에 대 한 높은 제한으로 CloudKit를 무료로 제공 합니다. 대규모 프로젝트 또는 대규모 사용자 기반 응용 프로그램의 경우 Apple은 저렴 한 가격 책정 눈금이 제공 되는 것을 힌트 했습니다.

## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Xamarin 응용 프로그램에서 CloudKit 사용

Xamarin 응용 프로그램에서 CloudKit 프레임 워크를 사용 하려면 먼저 [기능 사용](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 및 [자격 가이드 작업](~/ios/deploy-test/provisioning/entitlements.md) 에서 자세히 설명 된 대로 응용 프로그램을 올바르게 프로 비전 해야 합니다.

CloudKit에 액세스 하려면 **info.plist** 파일에 **iCloud**, **키-값 저장소**및 **cloudkit** 사용 권한을 포함 해야 합니다.

### <a name="sample-app"></a>샘플 앱

[CloudKitAtlas 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-cloudkitatlas) 에서는 Xamarin과 함께 cloudkit를 사용 하는 방법을 보여 줍니다. 아래 단계에서는 샘플을 구성 하는 방법을 보여 줍니다. CloudKit에만 필요한 것 외에 추가 설정이 필요 합니다.

1. Mac용 Visual Studio 또는 Visual Studio에서 프로젝트를 엽니다.
2. **솔루션 탐색기**에서 **info.plist** 파일을 열고 **번들 식별자** 가 프로 비전 설정의 일부로 만든 **앱 ID** 에 정의 된 것과 일치 하는지 확인 합니다.
3. **Info.plist** 파일의 아래쪽으로 스크롤하고 **사용 하도록 설정 된 백그라운드 모드**, **위치 업데이트**및 **원격 알림**을 선택 합니다.
4. 솔루션에서 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **옵션**을 선택 합니다.
5. **IOS 번들 서명**을 선택 하 고 위에서 만든 **개발자 Id** 및 **프로 비전 프로필** 을 선택 합니다.
6. **Info.plist** 에 iCloud, **키-값 저장소**및 **cloudkit** **사용**이 포함 되어 있는지 확인 합니다.
7. 응용 프로그램에 대 한 **어디 컨테이너가** 있는지 확인 합니다. 예: `iCloud.com.your-company.CloudKitAtlas`
8. 파일의 변경 내용을 저장합니다.

이러한 설정이 적용 된 후 샘플 앱은 이제 CloudKit 프레임 워크 Api 뿐만 아니라 백그라운드, 위치 및 notification services에 액세스할 준비가 되었습니다.

## <a name="cloudkit-api-overview"></a>CloudKit API 개요

Xamarin iOS 응용 프로그램에서 CloudKit를 구현 하기 전에이 문서에서는 다음 항목을 포함 하는 CloudKit 프레임 워크의 기본 사항을 다룹니다.

1. **컨테이너** – iCloud 통신의 격리 된 사일로.
2. **데이터베이스** – Public 및 private은 응용 프로그램에서 사용할 수 있습니다.
3. **레코드** – cloudkit에서 구조화 된 데이터를 이동 하는 메커니즘입니다.
4. **레코드 영역** – 레코드의 그룹입니다.
5. **레코드 식별자** – 정규화 되며 레코드의 특정 위치를 나타냅니다.
6. **참조** – 지정 된 데이터베이스 내의 관련 레코드 간에 부모-자식 관계를 제공 합니다.
7. **자산** – 규모가 크고 구조화 되지 않은 데이터의 파일을 iCloud에 업로드 하 고 지정 된 레코드와 연결할 수 있습니다.

### <a name="containers"></a>컨테이너

IOS 장치에서 실행 되는 지정 된 응용 프로그램은 항상 해당 장치에서 다른 응용 프로그램 및 서비스와 함께 실행 됩니다. 클라이언트 장치에서 응용 프로그램은 어떤 방식으로든 사일로 되거나 샌드 박싱 됩니다. 경우에 따라이는 리터럴 샌드박스에서, 다른 응용 프로그램에서는 단순히 자체 메모리 공간에서 실행 되 고 있습니다.

클라이언트 응용 프로그램을 가져와 다른 클라이언트에서 분리 하는 개념은 매우 강력 하며 다음과 같은 이점을 제공 합니다.

1. **보안** – 한 응용 프로그램은 다른 클라이언트 앱 또는 OS 자체를 방해할 수 없습니다.
1. **안정성** – 클라이언트 응용 프로그램이 충돌 하는 경우 OS의 다른 앱을 사용할 수 없습니다.
1. **개인** 정보 – 각 클라이언트 응용 프로그램은 장치에 저장 된 개인 정보에 대 한 액세스를 제한 합니다.

CloudKit는 위에 나열 된 것과 동일한 이점을 제공 하 고 클라우드 기반 정보를 사용 하는 데 적용 하도록 설계 되었습니다.

 [![](intro-to-cloudkit-images/image31.png "CloudKit apps communicate using containers")](intro-to-cloudkit-images/image31.png#lightbox)

응용 프로그램이 장치에서 실행 되는 중 하나인 것과 마찬가지로, 응용 프로그램은 iCloud를 일대다로 통신 합니다. 이러한 각각의 서로 다른 통신 사일로를 컨테이너 라고 합니다.

컨테이너는 `CKContainer` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 기본적으로 한 응용 프로그램은 하나의 컨테이너에 대해 통신 하 고이 컨테이너는 해당 응용 프로그램에 대 한 데이터를 분리 합니다. 즉, 여러 응용 프로그램이 동일한 iCloud 계정에 정보를 저장할 수 있지만이 정보는 혼합 되지 않습니다.

또한 iCloud 데이터의 컨테이너 화를 통해 CloudKit에서 사용자 정보를 캡슐화 할 수 있습니다. 이러한 방식으로 응용 프로그램은 사용자의 개인 정보 보호 및 보안을 계속 유지 하면서도 iCloud 계정 및에 저장 된 사용자 정보에 대 한 몇 가지 제한 된 액세스 권한을 가집니다.

컨테이너는 응용 프로그램 개발자가 WWDR 포털을 통해 완전히 관리 됩니다. 컨테이너의 네임 스페이스는 모든 Apple 개발자에 게 전역적 이므로 컨테이너는 지정 된 개발자의 응용 프로그램에 대해서만 고유 하 고 모든 Apple 개발자 및 응용 프로그램에 대해 고유할 필요는 없습니다.

Apple은 응용 프로그램 컨테이너의 네임 스페이스를 만들 때 역방향 DNS 표기법을 사용 하는 것을 제안 합니다. 예: `iCloud.com.company-name.application-name`

컨테이너는 기본적으로 지정 된 응용 프로그램에 대해 일대일로 바인딩되고 응용 프로그램 간에 공유할 수 있습니다. 따라서 여러 응용 프로그램이 단일 컨테이너에서 조정 될 수 있습니다. 단일 응용 프로그램은 여러 컨테이너와 통신할 수도 있습니다.

### <a name="databases"></a>데이터베이스

CloudKit의 주요 기능 중 하나는 응용 프로그램의 데이터 모델 및 복제를 사용 하 여 iCloud 서버를 모델링 하는 것입니다. 일부 정보는이를 만든 사용자를 위한 것 이며, 다른 정보는 식당 리뷰와 같이 사용자가 공개적으로 사용 하 여 만들 수 있는 공용 데이터 또는 개발자가 응용 프로그램에 대해 게시 한 정보 일 수 있습니다. 두 경우 모두 대상 그룹은 단일 사용자가 아니라 사람의 커뮤니티입니다.

 [![](intro-to-cloudkit-images/image32.png "CloudKit Container Diagram")](intro-to-cloudkit-images/image32.png#lightbox)

컨테이너 내부에서 가장 먼저 공용 데이터베이스입니다. 여기에는 모든 공용 정보가 상주 하 고 공동 mingles 됩니다. 또한 응용 프로그램의 각 사용자에 대 한 여러 개별 개인 데이터베이스가 있습니다.

IOS 장치에서 실행 되는 경우 응용 프로그램은 현재 로그온 한 iCloud 사용자에 대 한 정보에만 액세스할 수 있습니다. 따라서 컨테이너의 응용 프로그램 보기는 다음과 같습니다.

 [![](intro-to-cloudkit-images/image33.png "The applications view of the container")](intro-to-cloudkit-images/image33.png#lightbox)

현재 로그온 한 iCloud 사용자와 연결 된 공용 데이터베이스와 개인 데이터베이스만 볼 수 있습니다.

데이터베이스는 `CKDatabase` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 모든 응용 프로그램은 두 개의 데이터베이스, 즉 공용 데이터베이스와 전용 데이터베이스에 액세스할 수 있습니다.

컨테이너는 CloudKit의 초기 진입점입니다. 다음 코드를 사용 하 여 응용 프로그램의 기본 컨테이너에서 public 및 private 데이터베이스에 액세스할 수 있습니다.

```csharp
using CloudKit;
//...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
//...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

데이터베이스 유형 간의 차이점은 다음과 같습니다.

||공용 데이터베이스|전용 데이터베이스|
|---|--- |--- |
|**데이터 형식**|공유 데이터|현재 사용자의 데이터|
|**할당량**|개발자 할당량에서의 고려|사용자의 할당량에 대 한 고려|
|**기본 권한**|전 세계 읽기|사용자가 읽을 수 있음|
|**편집 권한**|레코드 클래스 수준을 통한 iCloud 대시보드 역할|해당 사항 없음|

### <a name="records"></a>레코드

컨테이너는 데이터베이스를 보유 하며 데이터베이스 내에는 레코드가 있습니다. 레코드는 CloudKit에서 구조적 데이터를 이동 하는 메커니즘입니다.

 [![](intro-to-cloudkit-images/image34.png "Containers hold databases, and inside databases are records")](intro-to-cloudkit-images/image34.png#lightbox)

레코드는 키-값 쌍을 래핑하는 `CKRecord` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 응용 프로그램의 개체 인스턴스는 CloudKit의 `CKRecord`와 동일 합니다. 또한 각 `CKRecord`는 개체의 클래스와 동일한 레코드 형식을 갖습니다.

레코드는 just-in-time 스키마를 가지 므로 처리를 위해 전달 되기 전에 데이터를 CloudKit에 설명 합니다. 이 시점부터 CloudKit는 정보를 해석 하 고 레코드를 저장 하 고 검색 하는 물류를 처리 합니다.

`CKRecord` 클래스는 다양 한 범위의 메타 데이터도 지원 합니다. 예를 들어 레코드는 만들어진 시기와 해당 레코드를 만든 사용자에 대 한 정보를 포함 합니다. 레코드에는 마지막으로 수정한 시간 및 해당 레코드를 수정한 사용자에 대 한 정보도 포함 됩니다.

레코드는 변경 태그의 개념을 포함 합니다. 지정 된 레코드에 대 한 이전 버전의 수정 버전입니다. 변경 태그는 클라이언트와 서버에 지정 된 레코드의 동일한 버전이 있는지 여부를 확인 하는 간단한 방법으로 사용 됩니다.

위에서 설명한 것 처럼 키-값 쌍의 줄 바꿈 `CKRecords` 다음 형식의 데이터를 레코드에 저장할 수 있습니다.

1. `NSString`
1. `NSNumber`
1. `NSData`
1. `NSDate`
1. `CLLocation`
1. `CKReferences`
1. `CKAssets`

단일 값 형식 외에도 레코드는 위에 나열 된 형식의 동일한 배열을 포함할 수 있습니다.

다음 코드를 사용 하 여 새 레코드를 만들고 데이터베이스에 저장할 수 있습니다.

```csharp
using CloudKit;
//...

private const string ReferenceItemRecordName = "ReferenceItems";
//...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>레코드 영역

레코드는 지정 된 데이터베이스 내에 자체적으로 존재 하지 않습니다. 레코드 그룹은 레코드 영역 내에 함께 존재 합니다. 레코드 영역은 기존 관계형 데이터베이스에서 테이블로 간주할 수 있습니다.

 [![](intro-to-cloudkit-images/image35.png "Groups of records exist together inside a Record Zone")](intro-to-cloudkit-images/image35.png#lightbox)

지정 된 레코드 영역 내에 여러 레코드가 있을 수 있으며 지정 된 데이터베이스 내에 여러 레코드 영역이 있을 수 있습니다. 모든 데이터베이스에는 기본 레코드 영역이 있습니다.

 [![](intro-to-cloudkit-images/image36.png "Every database contains a Default Record Zone and Custom Zone")](intro-to-cloudkit-images/image36.png#lightbox)

기본적으로 레코드가 저장 됩니다. 또한 사용자 지정 레코드 영역을 만들 수 있습니다. 레코드 영역은 원자성 커밋 및 변경 내용 추적 완료 되는 기본 세분성을 나타냅니다.

## <a name="record-identifiers"></a>레코드 식별자

레코드 식별자는 클라이언트에서 제공 하는 레코드 이름과 레코드가 존재 하는 영역을 모두 포함 하는 튜플로 표시 됩니다. 레코드 식별자에는 다음과 같은 특징이 있습니다.

- 클라이언트 응용 프로그램에 의해 생성 됩니다.
- 완전히 정규화 되며 레코드의 특정 위치를 나타냅니다.
- 외부 데이터베이스에 있는 레코드의 고유 ID를 레코드 이름에 할당 하 여 CloudKit 내에 저장 되지 않은 로컬 데이터베이스를 연결 하는 데 사용할 수 있습니다.

개발자는 새 레코드를 만들 때 레코드 식별자를 전달 하도록 선택할 수 있습니다. 레코드 식별자를 지정 하지 않으면 UUID가 자동으로 만들어지고 레코드에 할당 됩니다.

개발자는 새 레코드 식별자를 만들 때 각 레코드가 속할 레코드 영역을 지정 하도록 선택할 수 있습니다. 지정 하지 않으면 기본 레코드 영역이 사용 됩니다.

레코드 식별자는 `CKRecordID` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 다음 코드를 사용 하 여 새 레코드 식별자를 만들 수 있습니다.

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>참조

참조는 지정 된 데이터베이스 내에서 관련 레코드 간의 관계를 제공 합니다.

 [![](intro-to-cloudkit-images/image37.png "References provide relationships between related Records within a given Database")](intro-to-cloudkit-images/image37.png#lightbox)

위의 예제에서 부모는 부모 레코드의 자식 레코드가 되도록 자식을 소유 합니다. 관계는 자식 레코드에서 부모 레코드로 이동 하 여 *역참조*로 참조 됩니다.

참조는 `CKReference` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 이를 통해 iCloud 서버에서 레코드 간의 관계를 이해할 수 있습니다.

참조는 연속 삭제의 메커니즘을 제공 합니다. 부모 레코드를 데이터베이스에서 삭제 하면 관계에 지정 된 모든 자식 레코드도 데이터베이스 에서도 자동으로 삭제 됩니다.

> [!NOTE]
> 현 수 포인터는 CloudKit를 사용 하는 경우에 발생할 수 있습니다. 예를 들어 응용 프로그램에서 레코드 포인터 목록을 가져오고 레코드를 선택한 다음 레코드를 요청 하면 레코드가 데이터베이스에 더 이상 존재 하지 않을 수 있습니다. 이러한 상황을 정상적으로 처리 하려면 응용 프로그램을 코딩 해야 합니다.

필요 하지는 않지만 CloudKit 프레임 워크를 사용 하는 경우에는 역참조를 사용 하는 것이 좋습니다. Apple은이를 가장 효율적으로 참조할 수 있도록 시스템을 미세 조정 했습니다.

참조를 만들 때 개발자는 이미 메모리에 있는 레코드를 제공 하거나 레코드 식별자에 대 한 참조를 만들 수 있습니다. 레코드 식별자를 사용 하 고 지정 된 참조가 데이터베이스에 없는 경우 현 수 포인터가 생성 됩니다.

다음은 알려진 레코드에 대 한 참조를 만드는 예입니다.

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>자산

자산은 구조화 되지 않은 크고 구조화 되지 않은 데이터의 파일을 iCloud에 업로드 하 고 지정 된 레코드와 연결할 수 있습니다.

 [![](intro-to-cloudkit-images/image38.png "Assets allow for a file of large, unstructured data to be uploaded to iCloud and associated with a given Record")](intro-to-cloudkit-images/image38.png#lightbox)

클라이언트에서 iCloud 서버에 업로드할 파일을 설명 하는 `CKRecord` 만들어집니다. 이 파일을 포함 하는 `CKAsset` 만들어지고 해당 파일을 설명 하는 레코드에 연결 됩니다.

파일이 서버에 업로드 되 면 레코드가 데이터베이스에 배치 되 고 파일이 특수 한 대량 저장소 데이터베이스에 복사 됩니다. 레코드 포인터와 업로드 된 파일 사이에 링크가 만들어집니다.

자산은 `CKAsset` 클래스를 통해 CloudKit 프레임 워크에 노출 되며, 구조화 되지 않은 대량의 데이터를 저장 하는 데 사용 됩니다. 개발자는 메모리에 구조화 되지 않은 대량의 데이터를 포함 하는 것을 원하지 않으므로 디스크의 파일을 사용 하 여 자산을 구현 합니다.

자산은 레코드를 포인터로 소유 하 여 iCloud에서 자산을 검색할 수 있도록 합니다. 이러한 방식으로 자산을 소유 하는 레코드가 삭제 되 면 서버에서 자산을 가비지 수집할 수 있습니다.

`CKAssets`은 다량의 데이터 파일을 처리 하기 때문에 Apple에서 효과적으로 업로드 하 고 자산을 다운로드 하는 데 사용 하 고 있습니다.

다음 코드를 사용 하 여 자산을 만들고 레코드와 연결할 수 있습니다.

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

이제 CloudKit 내에서 모든 기본 개체를 다루었습니다. 컨테이너는 응용 프로그램에 연결 되며 데이터베이스를 포함 합니다. 데이터베이스는 레코드 영역으로 그룹화 되 고 레코드 식별자가 가리키는 레코드를 포함 합니다. 부모-자식 관계는 참조를 사용 하 여 레코드 간에 정의 됩니다. 마지막으로, 자산을 사용 하 여 대량 파일을 업로드 하 고 레코드에 연결할 수 있습니다.

## <a name="cloudkit-convenience-api"></a>CloudKit 편리한 API

Apple은 CloudKit를 사용 하기 위한 두 가지 API 집합을 제공 합니다.

- **운영 API** – cloudkit의 모든 단일 기능을 제공 합니다. 더 복잡 한 응용 프로그램의 경우이 API는 CloudKit에 대 한 세밀 한 제어를 제공 합니다.
- **편리한 API** – 일반적으로 미리 구성 된 cloudkit 기능의 하위 집합을 제공 합니다. IOS 응용 프로그램에 CloudKit 기능을 포함 하기 위한 편리 하 고 쉬운 액세스 솔루션을 제공 합니다.

편리한 API는 일반적으로 대부분의 iOS 응용 프로그램에서 가장 적합 한 선택 항목으로 시작 하는 Apple 제안입니다. 이 섹션의 나머지 부분에서는 다음과 같은 편리한 API 항목을 다룹니다.

- 레코드 저장
- 레코드를 페치하는 중입니다.
- 레코드를 업데이트 하는 중입니다.

### <a name="common-setup-code"></a>일반 설정 코드

CloudKit 편리한 API를 시작 하기 전에 몇 가지 표준 설정 코드가 필요 합니다. 응용 프로그램의 `AppDelegate.cs` 파일을 수정 하 여 시작 하 고 다음과 같이 만듭니다.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
    }
}
```

위의 코드는 응용 프로그램의 나머지 부분에서 보다 쉽게 작업할 수 있도록 공용 및 개인 CloudKit 데이터베이스를 바로 가기로 노출 합니다.

다음으로 CloudKit를 사용 하는 뷰 또는 뷰 컨테이너에 다음 코드를 추가 합니다.

```csharp
using CloudKit;
//...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

그러면 `AppDelegate`에 대 한 바로 가기가 추가 되 고 위에서 만든 공용 및 개인 데이터베이스 바로 가기에 액세스할 수 있습니다.

이 코드를 사용 하 여 Xamarin iOS 8 응용 프로그램에서 CloudKit 편리한 API를 구현 하는 방법을 살펴보겠습니다.

### <a name="saving-a-record"></a>레코드 저장

위에서 설명한 패턴을 사용 하 여 레코드를 설명할 때 다음 코드는 새 레코드를 만들고 편리한 API를 사용 하 여 공용 데이터베이스에 저장 합니다.

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

위의 코드에 대해 세 가지 사항을 확인 해야 합니다.

1. 개발자는 `PublicDatabase`의 `SaveRecord` 메서드를 호출 하 여 데이터를 전송 하는 방법, 데이터를 작성 하는 영역 등을 지정할 필요가 없습니다. 편리한 API는 이러한 세부 정보를 모두 처리 합니다.
1. 호출은 비동기 호출이 고 성공 또는 실패를 포함 하 여 호출이 완료 되 면 콜백 루틴을 제공 합니다. 호출에 실패 하면 오류 메시지가 제공 됩니다.
1. CloudKit는 로컬 저장소/지 속성을 제공 하지 않습니다. 전송 미디어 전용입니다. 따라서 레코드를 저장 하도록 요청 하면 해당 레코드는 iCloud 서버에 즉시 전송 됩니다.

> [!NOTE]
> 연결이 지속적으로 삭제 되거나 중단 되는 모바일 네트워크 통신의 "손실" 특성으로 인해 CloudKit로 작업할 때 개발자가 수행 해야 하는 첫 번째 고려 사항 중 하나는 오류 처리입니다.

### <a name="fetching-a-record"></a>레코드 페치

레코드가 만들어지고 iCloud 서버에 성공적으로 저장 되 면 다음 코드를 사용 하 여 레코드를 검색 합니다.

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

레코드를 저장 하는 것과 마찬가지로 위의 코드는 간단 하 고 간단한 오류 처리가 필요 합니다.

### <a name="updating-a-record"></a>레코드 업데이트

ICloud 서버에서 레코드를 가져온 후에는 다음 코드를 사용 하 여 레코드를 수정 하 고 변경 내용을 데이터베이스에 다시 저장할 수 있습니다.

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

호출에 성공 하면 `PublicDatabase`의 `FetchRecord` 메서드에서 `CKRecord` 반환 됩니다. 그러면 응용 프로그램에서 레코드를 수정 하 고 `SaveRecord`를 다시 호출 하 여 변경 내용을 데이터베이스에 다시 기록 합니다.

이 섹션에서는 응용 프로그램이 CloudKit 편리한 API를 사용할 때 사용 하는 일반적인 주기를 보여 줍니다. 응용 프로그램은 iCloud에 레코드를 저장 하 고, iCloud에서 해당 레코드를 검색 하 고, 레코드를 수정 하 고, 이러한 변경 내용을 iCloud에 다시 저장 합니다.

## <a name="designing-for-scalability"></a>확장성을 위한 디자인

지금까지이 문서에서는 작업 될 때마다 iCloud 서버에서 응용 프로그램의 전체 개체 모델을 저장 하 고 검색 하는 방법을 살펴보았습니다. 이 접근 방식은 적은 양의 데이터와 매우 작은 사용자 기반에서 잘 작동 하지만 정보 및/또는 사용자 기반의 양이 늘어나면 확장 되지 않습니다.

### <a name="big-data-tiny-device"></a>빅 데이터, 작은 장치

응용 프로그램의 인기는 데이터베이스의 데이터를 더 많이 사용할 수 있으며,이는 장치에서 해당 전체 데이터의 캐시를 사용 하는 것입니다. 이 문제를 해결 하는 데 사용할 수 있는 기술은 다음과 같습니다.

- **클라우드의 대량 데이터 유지** – cloudkit는 대량 데이터를 효율적으로 처리할 수 있도록 설계 되었습니다.
- **클라이언트는 해당 데이터의 조각만 보아야** 합니다. 지정 된 시간에 작업을 처리 하는 데 필요한 최소한의 데이터를 가져옵니다.
- **클라이언트 뷰가 변경 될 수 있습니다** . 각 사용자에 게는 다른 기본 설정이 있으므로 표시 되는 데이터의 조각이 사용자에서 사용자로 변경 되 고 지정 된 조각의 사용자에 대 한 개별 뷰가 다를 수 있습니다.
- **클라이언트는 쿼리를 사용** 하 여 관점에 중점을 둡니다. 쿼리를 사용 하면 사용자가 클라우드에 있는 더 큰 데이터 집합의 작은 하위 집합을 볼 수 있습니다.

### <a name="queries"></a>쿼리

위에서 설명한 것 처럼, 쿼리를 통해 개발자는 클라우드에 존재 하는 큰 데이터 집합의 작은 하위 집합을 선택할 수 있습니다. 쿼리는 `CKQuery` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다.

쿼리는 레코드 형식 (`RecordType`), 조건자 (`NSPredicate`) 및 선택적으로 정렬 설명자 (`NSSortDescriptors`)와 같은 세 가지를 결합 합니다. CloudKit는 대부분의 `NSPredicate`을 지원 합니다.

#### <a name="supported-predicates"></a>지원 되는 조건자

CloudKit는 쿼리를 사용할 때 다음과 같은 유형의 `NSPredicates` 지원 합니다.

1. 이름이 변수에 저장 된 값과 동일한 레코드 일치:

    ```csharp
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```

2. 는 동적 키 값에 따라 일치 하는 항목을 허용 하므로 컴파일 시간에 키를 몰라도 됩니다.

    ```csharp
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```

3. 레코드 값이 지정 된 값 보다 큰 레코드 일치:

    ```csharp
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. 레코드 위치가 지정 된 위치의 100 미터 내에 있는 일치 레코드:

    ```csharp
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit는 토큰화 된 검색을 지원 합니다. 이 호출은 `after` 및 `session`에 대해 하나씩 두 개의 토큰을 만듭니다. 이러한 두 토큰이 포함 된 레코드를 반환 합니다.

    ```csharp
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```

6. CloudKit는 `AND` 연산자를 사용 하 여 조인 된 복합 조건자를 지원 합니다.

    ```csharp
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```

#### <a name="creating-queries"></a>쿼리 만들기

다음 코드를 사용 하 여 Xamarin iOS 8 응용 프로그램에서 `CKQuery`를 만들 수 있습니다.

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

첫째, 지정 된 이름과 일치 하는 레코드만 선택 하는 조건자를 만듭니다. 그런 다음 조건자와 일치 하는 지정 된 레코드 형식의 레코드를 선택 하는 쿼리를 만듭니다.

#### <a name="performing-a-query"></a>쿼리 수행

쿼리를 만든 후에는 다음 코드를 사용 하 여 쿼리를 수행 하 고 반환 된 레코드를 처리 합니다.

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

위의 코드는 위에서 만든 쿼리를 사용 하 여 공용 데이터베이스에 대해 실행 합니다. 레코드 영역을 지정 하지 않았기 때문에 모든 영역이 검색 됩니다. 오류가 발생 하지 않은 경우 쿼리의 매개 변수와 일치 하는 `CKRecords`의 배열이 반환 됩니다.

쿼리에 대해 생각 하는 방법은 폴링 이며 큰 데이터 집합을 통해 조각화 하는 것입니다. 그러나 쿼리는 다음과 같은 이유로 크고 대부분 정적 데이터 집합에 적합 하지 않습니다.

- 장치 배터리 수명에 대해 올바르지 않습니다.
- 네트워크 트래픽에는 올바르지 않습니다.
- 표시 되는 정보는 응용 프로그램이 데이터베이스를 폴링하는 빈도에 의해 제한 되기 때문에 사용자 환경에는 적합 하지 않습니다. 현재 사용자는 변경 사항이 있을 때 푸시 알림을 필요로 합니다.

### <a name="subscriptions"></a>알림 신청

대부분의 정적 데이터 집합을 처리할 때는 클라이언트가 클라이언트를 대신 하 여 서버에서 실행 되어야 합니다. 쿼리는 백그라운드에서 실행 되어야 하며, 현재 장치나 다른 장치가 동일한 데이터베이스를 터치 하는지 여부에 관계 없이 모든 단일 레코드 저장 후 실행 되어야 합니다.

마지막으로 서버 쪽 쿼리를 실행할 때 데이터베이스에 연결 된 모든 장치에 푸시 알림을 보내야 합니다.

구독은 `CKSubscription` 클래스를 통해 CloudKit 프레임 워크에서 노출 됩니다. 레코드 형식 (`RecordType`), 조건자 (`NSPredicate`) 및 Apple 푸시 알림 (`Push`)을 결합 합니다.

> [!NOTE]
> CloudKit 푸시는 푸시 발생의 원인이 되는 것과 같은 CloudKit 관련 정보를 포함 하는 페이로드를 포함 하 고 있으므로 약간 확대 됩니다.

#### <a name="how-subscriptions-work"></a>구독 작동 방법

코드에서 C# 구독을 구현 하기 전에 구독 작동 방법에 대 한 간략 한 개요를 살펴보겠습니다.

 [![](intro-to-cloudkit-images/image39.png "An overview of how subscriptions work")](intro-to-cloudkit-images/image39.png#lightbox)

위의 그래프는 일반적인 구독 프로세스를 다음과 같이 보여 줍니다.

1. 클라이언트 장치는 구독을 트리거하는 조건 집합을 포함 하는 새 구독 및 트리거가 발생할 때 전송 될 푸시 알림을 만듭니다.
2. 구독은 기존 구독 컬렉션에 추가 되는 데이터베이스로 전송 됩니다.
3. 두 번째 장치는 새 레코드를 만들고 해당 레코드를 데이터베이스에 저장 합니다.
4. 데이터베이스는 구독 목록을 검색 하 여 새 레코드가 조건 중 하 나와 일치 하는지 확인 합니다.
5. 일치 항목이 발견 되 면 트리거를 트리거한 레코드에 대 한 정보를 구독에 등록 한 장치에 푸시 알림이 전송 됩니다.

이러한 지식이 있으면 Xamarin iOS 8 응용 프로그램에서 구독을 만드는 방법을 살펴보겠습니다.

#### <a name="creating-subscriptions"></a>구독 만들기

다음 코드를 사용 하 여 구독을 만들 수 있습니다.

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

먼저 구독을 트리거하는 조건을 제공 하는 조건자를 만듭니다. 그런 다음 특정 레코드 유형에 대 한 구독을 만들고 트리거를 테스트할 때의 옵션을 설정 합니다. 마지막으로 구독을 트리거할 때 발생 하는 알림의 유형을 정의 하 고 구독에 연결 합니다.

### <a name="saving-subscriptions"></a>구독 저장

구독을 만든 후 다음 코드는이를 데이터베이스에 저장 합니다.

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

편리한 API를 사용 하 여 호출은 비동기, 단순 및 쉬운 오류 처리를 제공 합니다.

#### <a name="handling-push-notifications"></a>푸시 알림 처리

개발자가 이전에 Apple Push Notification (AP)를 사용한 경우 CloudKit에 의해 생성 된 알림을 처리 하는 과정을 잘 알고 있어야 합니다.

`AppDelegate.cs`에서 `ReceivedRemoteNotification` 클래스를 다음과 같이 재정의 합니다.

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

위의 코드는 CloudKit에 정보를 CloudKit 알림에 구문 분석 하도록 요청 합니다. 다음으로 경고에 대 한 정보를 추출 합니다. 마지막으로 알림 유형을 테스트 하 고 그에 따라 알림을 처리 합니다.

이 섹션에서는 쿼리 및 구독을 사용 하 여 위에서 설명한 빅 데이터, 작은 장치 문제에 응답 하는 방법을 보여 줍니다. 응용 프로그램은 클라우드의 대량 데이터를 그대로 유지 하 고 이러한 기술을 사용 하 여이 데이터 집합에 뷰를 제공 합니다.

## <a name="cloudkit-user-accounts"></a>CloudKit 사용자 계정

이 문서의 시작 부분에서 언급 했 듯이 CloudKit는 기존 iCloud 인프라를 기반으로 빌드됩니다. 다음 섹션에서는 CloudKit API를 사용 하 여 개발자에 게 계정을 노출 하는 방법에 대해 자세히 설명 합니다.

### <a name="authentication"></a>인증

사용자 계정을 처리할 때 첫 번째 고려 사항은 인증입니다. CloudKit는 장치에서 현재 로그인 한 iCloud 사용자를 통한 인증을 지원 합니다. 인증은 백그라운드에서 수행 되며 iOS에서 처리 됩니다. 이러한 방식으로 개발자는 인증 구현에 대 한 세부 정보를 걱정 하지 않아도 됩니다. 사용자가 로그온 되어 있는지만 테스트 합니다.

### <a name="user-account-information"></a>사용자 계정 정보

CloudKit는 개발자에 게 다음과 같은 사용자 정보를 제공 합니다.

- **Id** – 사용자를 고유 하 게 식별 하는 방법입니다.
- **메타 데이터** – 사용자에 대 한 정보를 저장 하 고 검색 하는 기능입니다.
- **개인** 정보-모든 정보는 개인 정보 취급 manor에서 처리 됩니다. 사용자가 동의 하지 않은 경우 아무 것도 노출 되지 않습니다.
- **검색** – 사용자에 게 동일한 응용 프로그램을 사용 하는 친구를 검색할 수 있는 기능을 제공 합니다.

다음에는 이러한 항목에 대해 자세히 살펴보겠습니다.

#### <a name="identity"></a>항등

위에서 설명한 것 처럼 CloudKit는 응용 프로그램이 지정 된 사용자를 고유 하 게 식별할 수 있는 방법을 제공 합니다.

 [![](intro-to-cloudkit-images/image40.png "Uniquely identifing a given user")](intro-to-cloudkit-images/image40.png#lightbox)

사용자의 장치에서 실행 되는 클라이언트 응용 프로그램과 CloudKit 컨테이너 내의 모든 특정 사용자 개인 데이터베이스가 있습니다. 클라이언트 응용 프로그램은 이러한 특정 사용자 중 하나에 연결 됩니다. 장치에서 로컬로 iCloud에 로그인 한 사용자를 기반으로 합니다.

이는 iCloud에서 제공 되기 때문에 사용자 정보에 대 한 다양 한 백업 저장소가 있습니다. ICloud는 실제로 컨테이너를 호스트 하기 때문에 사용자의 상관 관계를 지정할 수 있습니다. 위의 그림에서 iCloud 계정이 `user@icloud.com` 있는 사용자는 현재 클라이언트에 연결 됩니다.

컨테이너를 기반으로 하는 컨테이너에서 무작위로 생성 된 고유한 사용자 ID가 만들어지고 사용자의 iCloud 계정 (메일 주소)과 연결 됩니다. 이 사용자 ID는 응용 프로그램에 반환 되며 개발자가 적합 한 방식으로 사용할 수 있습니다.

> [!NOTE]
> 동일한 iCloud 사용자에 대해 동일한 장치에서 실행 되는 다른 응용 프로그램은 서로 다른 CloudKit 컨테이너에 연결 되어 있기 때문에 서로 다른 사용자 Id를 갖게 됩니다.

다음 코드는 장치에서 현재 로그인 한 iCloud 사용자에 대 한 CloudKit 사용자 ID를 가져옵니다.

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

위의 코드에서는 현재 로그인 한 사용자의 ID를 제공 하도록 CloudKit 컨테이너에 요청 합니다. 이 정보는 iCloud 서버에서 제공 되므로 호출은 비동기적 이며 오류 처리가 필요 합니다.

#### <a name="metadata"></a>메타데이터

CloudKit의 각 사용자에 게는이를 설명 하는 특정 메타 데이터가 있습니다. 이 메타 데이터는 CloudKit 레코드로 표시 됩니다.

 [![](intro-to-cloudkit-images/image41.png "Each user in CloudKit has specific Metadata that describes them")](intro-to-cloudkit-images/image41.png#lightbox)

컨테이너의 특정 사용자에 대 한 전용 데이터베이스 내부를 살펴보면 해당 사용자를 정의 하는 레코드가 하나 있습니다. 공용 데이터베이스 내에는 컨테이너의 각 사용자에 대 한 여러 사용자 레코드가 있습니다. 이러한 항목 중 하나에는 현재 로그온 한 사용자의 레코드 ID와 일치 하는 레코드 ID가 있습니다.

공용 데이터베이스의 사용자 레코드는 전 세계에서 읽을 수 있습니다. 대부분의 경우 일반적인 레코드로 처리 되며 `CKRecordTypeUserRecord`형식이 있습니다. 이러한 레코드는 시스템에 예약 되어 있으며 쿼리에 사용할 수 없습니다.

사용자 레코드에 액세스 하려면 다음 코드를 사용 합니다.

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

위의 코드는 공용 데이터베이스를 요청 하 여 위에서 액세스 한 ID 인 사용자에 대 한 사용자 레코드를 반환 합니다. 이 정보는 iCloud 서버에서 제공 되므로 호출은 비동기적 이며 오류 처리가 필요 합니다.

#### <a name="privacy"></a>개인 정보 보호

CloudKit는 기본적으로 현재 로그온 한 사용자의 개인 정보 보호를 위해 설계 되었습니다. 사용자에 대 한 개인 식별 정보는 기본적으로 노출 되지 않습니다. 응용 프로그램에서 사용자에 대 한 제한 된 정보를 요구 하는 경우도 있습니다.

이러한 경우 응용 프로그램은 사용자가이 정보를 공개 하도록 요청할 수 있습니다. 사용자에 게 계정 정보를 표시 하도록 옵트인 하도록 요청 하는 대화 상자가 표시 됩니다.

#### <a name="discovery"></a>검색

사용자가 응용 프로그램에서 사용자 계정 정보에 대 한 제한 된 액세스를 허용 하도록 옵트인 (opt in) 하는 경우 응용 프로그램의 다른 사용자가 검색할 수 있습니다.

 [![](intro-to-cloudkit-images/image42.png "A user can be discoverable to other users of the application")](intro-to-cloudkit-images/image42.png#lightbox)

클라이언트 응용 프로그램은 컨테이너와 통신 하 고, 컨테이너는 iCloud를 말하고 사용자 정보에 액세스 합니다. 사용자는 전자 메일 주소를 제공할 수 있으며 검색을 사용 하 여 사용자에 대 한 정보를 다시 가져올 수 있습니다. 필요에 따라 사용자 ID를 사용 하 여 사용자에 대 한 정보를 검색할 수도 있습니다.

또한 CloudKit는 전체 주소록을 쿼리하여 현재 로그온 한 iCloud 사용자의 친구가 될 수 있는 모든 사용자에 대 한 정보를 검색 하는 방법을 제공 합니다. CloudKit 프로세스는 사용자의 연락처 책을 끌어오고 메일 주소를 사용 하 여 해당 주소와 일치 하는 다른 사용자의 응용 프로그램을 찾을 수 있는지 확인 합니다.

이렇게 하면 응용 프로그램에서 사용자의 연락처에 대 한 액세스를 제공 하거나 사용자에 게 연락처 액세스를 승인 하도록 요청 하지 않고 사용자의 연락처를 활용할 수 있습니다. 응용 프로그램에 대 한 연락처 정보를 사용할 수 없는 경우에는 CloudKit 프로세스만 액세스할 수 있습니다.

요약 하자면, 사용자 검색에 사용할 수 있는 세 가지 종류의 입력은 다음과 같습니다.

- **사용자 레코드 id** – 현재 로그인 한 cloudkit 사용자의 사용자 ID에 대해 검색을 수행할 수 있습니다.
- **사용자 전자 메일 주소** – 사용자가 전자 메일 주소를 제공할 수 있으며 검색에 사용할 수 있습니다.
- **주소록** – 사용자의 주소록은 연락처에 나열 된 것과 동일한 전자 메일 주소를 가진 응용 프로그램 사용자를 검색 하는 데 사용할 수 있습니다.

사용자 검색은 다음 정보를 반환 합니다.

- **사용자 레코드 id** -공용 데이터베이스의 사용자에 대 한 고유 id입니다.
- **First 및 Last Name** -Public 데이터베이스에 저장 됩니다.

이 정보는 옵트인 (opt in) 된 사용자에 대해서만 반환 됩니다.

다음 코드는 현재 장치에서 iCloud에 로그인 한 사용자에 대 한 정보를 검색 합니다.

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
//...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

연락처 책의 모든 사용자를 쿼리하려면 다음 코드를 사용 합니다.

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

이 섹션에서는 CloudKit에서 응용 프로그램에 제공할 수 있는 사용자 계정에 액세스 하는 네 가지 주요 영역에 대해 설명 했습니다. 사용자의 Id 및 메타 데이터를 가져오는 것부터 CloudKit에 기본 제공 되는 개인 정보 취급 방침에 대해 마지막으로 응용 프로그램의 다른 사용자를 검색 하는 기능입니다.

## <a name="the-development-and-production-environments"></a>개발 및 프로덕션 환경

CloudKit는 응용 프로그램의 레코드 유형 및 데이터에 대 한 별도의 개발 및 프로덕션 환경을 제공 합니다. 개발 환경은 개발 팀의 구성원만 사용할 수 있는 보다 유연한 환경입니다. 응용 프로그램에서 레코드에 새 필드를 추가 하 고 해당 레코드를 개발 환경에 저장 하면 서버에서 스키마 정보를 자동으로 업데이트 합니다.

개발자는이 기능을 사용 하 여 개발 중에 스키마를 변경 하 여 시간을 절약할 수 있습니다. 한 가지 주의할 점은, 레코드에 필드를 추가한 후에는 해당 필드와 연결 된 데이터 형식을 프로그래밍 방식으로 변경할 수 없다는 것입니다. 필드의 형식을 변경 하려면 개발자가 [Cloudkit 대시보드의](https://icloud.developer.apple.com/dashboard/) 필드를 삭제 하 고 새 형식으로 다시 추가 해야 합니다.

개발자는 응용 프로그램을 배포 하기 전에 **Cloudkit 대시보드**를 사용 하 여 스키마와 데이터를 프로덕션 환경으로 마이그레이션할 수 있습니다. 프로덕션 환경에 대해 실행 하는 경우 서버는 응용 프로그램에서 프로그래밍 방식으로 스키마를 변경 하지 못하도록 합니다. 개발자는 **Cloudkit 대시보드** 를 사용 하 여 변경 작업을 수행할 수 있지만 프로덕션 환경에서 레코드에 필드를 추가 하려고 하면 오류가 발생 합니다.

> [!NOTE]
> IOS 시뮬레이터는 **개발 환경**에서만 작동 합니다. 개발자가 **프로덕션 환경**에서 응용 프로그램을 테스트할 준비가 되 면 물리적 iOS 장치가 필요 합니다.

## <a name="shipping-a-cloudkit-enabled-app"></a>CloudKit 사용 앱 전달

CloudKit를 사용 하는 응용 프로그램을 전달 하기 전에 **프로덕션 Cloudkit 환경을** 대상으로 하도록 구성 해야 합니다. 그렇지 않으면 응용 프로그램이 Apple에서 거부 됩니다.

다음을 수행합니다.

1. Ma 용 Visual Studio에서 **릴리스** > **iOS 장치**에 대 한 응용 프로그램을 컴파일합니다.

    [![](intro-to-cloudkit-images/shipping01.png "Compile the application for Release")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. **빌드** 메뉴에서 **보관**을 선택 합니다.

    [![](intro-to-cloudkit-images/shipping02.png "Select Archive")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **보관 파일이** 생성 되 고 Mac용 Visual Studio 표시 됩니다.

    [![](intro-to-cloudkit-images/shipping03.png "The Archive will be created and displayed")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. **Xcode**를 시작합니다.
5. **창** 메뉴에서 **구성 도우미**를 선택 합니다.

    [![](intro-to-cloudkit-images/shipping04.png "Select Organizer")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. 응용 프로그램의 보관 위치를 선택 하 고 **내보내기 ...** 단추를 클릭 합니다.

    [![](intro-to-cloudkit-images/shipping05.png "The application's archive")](intro-to-cloudkit-images/shipping05.png#lightbox)

7. 내보낼 메서드를 선택 하 고 **다음** 단추를 클릭 합니다.

    [![](intro-to-cloudkit-images/shipping06.png "Select a method for export")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. 드롭다운 목록에서 **개발 팀** 을 선택 하 고 **선택** 단추를 클릭 합니다.

    [![](intro-to-cloudkit-images/shipping07.png "Select the Development Team from the dropdown list")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. 드롭다운 목록에서 **프로덕션** 을 선택 하 고 **다음** 단추를 클릭 합니다.

    [![](intro-to-cloudkit-images/shipping08.png "Select Production from the dropdown list")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. 설정을 검토 하 고 **내보내기** 단추를 클릭 합니다.

    [![](intro-to-cloudkit-images/shipping09.png "Review the setting")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. 결과 응용 프로그램 `.ipa` 파일을 생성할 위치를 선택 합니다.

이 프로세스는 응용 프로그램을 iTunes Connect에 직접 전송 하는 것과 유사 합니다. 내보내기 **... 단추를 클릭 하면 됩니다** . 구성 도우미 창에서 보관 파일을 선택한 후

## <a name="when-to-use-cloudkit"></a>CloudKit를 사용 하는 경우

이 문서에서 살펴본 것 처럼 CloudKit는 응용 프로그램이 iCloud 서버에서 정보를 저장 하 고 검색 하는 쉬운 방법을 제공 합니다. 즉, CloudKit는 기존 도구나 프레임 워크를 사용 하거나 사용 중단 하지 않습니다.

### <a name="use-cases"></a>사용 사례

다음 사용 사례는 개발자가 특정 iCloud 프레임 워크 또는 기술을 사용할 시기를 결정 하는 데 도움이 됩니다.

- **ICloud 키-값 저장소** – 적은 양의 데이터를 최신 상태로 유지 하 고 응용 프로그램 기본 설정으로 작업 하는 데 유용 합니다. 그러나 매우 적은 양의 정보에 대 한 제약을 받습니다.
- **Icloud 드라이브** – 기존 icloud 문서를 기반으로 구축 되었으며 파일 시스템에서 구조화 되지 않은 데이터를 동기화 하기 위한 간단한 api를 제공 합니다. Mac OS X에 대 한 전체 오프 라인 캐시를 제공 하며 문서 중심 응용 프로그램에 적합 합니다.
- **ICloud 핵심 데이터** – 모든 사용자의 장치 간에 데이터를 복제할 수 있습니다. 데이터는 단일 사용자 이며 개인의 구조화 된 데이터를 동기화 된 상태로 유지 하는 데 적합 합니다.
- **Cloudkit** – 공용 데이터와 구조를 모두 제공 하 고 대량 데이터 집합과 대용량 구조화 되지 않은 파일을 모두 처리할 수 있습니다. 사용자의 iCloud 계정에 연결 되 고 클라이언트에서 지시 된 데이터 전송을 제공 합니다.

이러한 사용 사례를 염두에 두면 개발자는 현재 필요한 응용 프로그램 기능을 모두 제공 하 고 향후 성장을 위해 뛰어난 확장성을 제공 하기 위해 올바른 iCloud 기술을 선택 해야 합니다.

## <a name="summary"></a>요약

이 문서는 CloudKit API에 대 한 간략 한 소개를 다룹니다. CloudKit를 사용 하도록 Xamarin iOS 응용 프로그램을 프로 비전 하 고 구성 하는 방법을 보여 줍니다. CloudKit의 편리한 API의 기능에 대해 살펴보았습니다. 쿼리 및 구독을 사용 하 여 확장성을 위해 CloudKit 사용 응용 프로그램을 디자인 하는 방법을 보여 줍니다. 그리고 마지막으로 CloudKit에 의해 응용 프로그램에 노출 되는 사용자 계정 정보를 표시 했습니다.

## <a name="related-links"></a>관련 링크

- [CloudKit (Apple)](https://developer.apple.com/icloud/cloudkit/)
- [CloudKitAtlas (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-cloudkitatlas)
- [프로 비전 프로필 만들기](~/ios/get-started/installation/device-provisioning/index.md)
