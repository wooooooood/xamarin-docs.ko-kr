---
title: Xamarin.iOS에서 CloudKit
description: 이 문서에서는 Xamarin.iOS에서 CloudKit을 사용 하는 방법을 설명 합니다. CloudKit의 개요를 제공 하 고 해당, CloudKit 편의 API, 확장성, 사용자 계정 및 개발 환경과 프로덕션 환경을 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/11/2016
ms.openlocfilehash: 8ef12c8b0822f3d0486f584878f572a266b0d44e
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831870"
---
# <a name="cloudkit-in-xamarinios"></a>Xamarin.iOS에서 CloudKit

CloudKit 프레임 워크는 해당 액세스 iCloud 응용 프로그램 개발을 간소화합니다. 여기에 응용 프로그램 정보를 안전 하 게 저장할 수 뿐만 아니라 응용 프로그램 데이터 및 자산 권한을 검색 합니다. 이 키트는 개인 정보를 공유 하지 않고 Id가 iCloud 사용 하 여 응용 프로그램에 대 한 액세스를 허용 하 여 사용자가 계층 익명성을 제공 합니다.

개발자는 클라이언트 쪽 응용 프로그램에 집중 하 고 서버 쪽 응용 프로그램 논리를 작성할 필요가 없습니다 iCloud 수 수 있습니다. CloudKit 인증, 사설 및 공용 데이터베이스 및 구조화 된 데이터 및 자산 저장소 서비스를 제공합니다.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 요구 됩니다.

-  **Xcode 및 iOS SDK** – Apple의 Xcode 및 iOS 8 Api 해야 설치 하 고 개발자의 컴퓨터에서 구성 합니다.
-  **Mac 용 visual Studio** – 최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
-  **iOS 8 장치** – 최신 버전의 테스트를 위해 iOS 8 실행 하는 iOS 장치.

## <a name="what-is-cloudkit"></a>CloudKit 란?

CloudKit 방법이 서버 iCloud로 개발자 액세스를 제공 합니다. ICloud 드라이브와 iCloud 사진 라이브러리에 대 한 기초를 제공합니다. CloudKit은 Mac OS X 및 Apple iOS 장치에서 지원 됩니다.

 [![](intro-to-cloudkit-images/image1.png "CloudKit은 Mac OS X 및 Apple iOS 장치 모두에서 지원 되는 방법")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit은 iCloud 계정 인프라를 사용합니다. 계정에 장치를 iCloud에 로그온 한 사용자 인 경우 CloudKit 사용자를 식별 하는 ID를 사용 합니다. 사용 가능한 계정이 없는 경우 제한 된 읽기 전용 액세스 제공 됩니다.

CloudKit 공용 및 개인 데이터베이스의 개념을 지원합니다. Public 데이터베이스 사용자가 액세스할 수 있는 모든 데이터의 "soup"를 제공 합니다. 전용 데이터베이스는 특정 사용자를 바인딩할 개인 데이터를 저장할 것입니다.

CloudKit 구조적 및 대량 데이터를 지원합니다. 큰 파일 전송을 원활 하 게 처리할 수 있는 것입니다. CloudKit 담당와 iCloud 서버 백그라운드에서 큰 파일을 전송 하는 효율적으로 개발자가 다른 작업에 집중할 수를 확보 합니다.

> [!NOTE]
> CloudKit는 중요 한 것을 _전송 기술_합니다. 모든 지 속성; 제공 하지 않습니다. 만 응용 프로그램을을 보내고 서버에서 정보를 효율적으로 받을 수 있습니다.

이 문서의 작성 시점 현재 Apple 처음에 게 제공 CloudKit 무료로 대역폭 및 저장소 용량에 대 한 상한입니다. 더 큰 프로젝트 또는 큰 사용자 기반을 사용 하 여 응용 프로그램에 대 한 Apple에 저렴 한 가격 책정 확장을 제공할 것 힌트.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Xamarin 응용 프로그램에서 사용 하도록 설정 하면 CloudKit

Xamarin 응용 프로그램을 CloudKit 프레임 워크를 활용할 수 있습니다, 전에 응용 프로그램이 올바르게 프로 비전 되어야 합니다에 설명 된 대로 합니다 [기능을 사용 하 여 작업](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) 하 고 [자격](~/ios/deploy-test/provisioning/entitlements.md) 가이드

1.  Mac 또는 Visual Studio 용 Visual Studio에서 프로젝트를 엽니다.
2.  에 **솔루션 탐색기**를 엽니다는 **Info.plist** 파일을 확인 합니다 **번들 식별자** 에 정의 된 것과 일치 하 **앱 ID**설정으로 프로 비전 하는 부분을 생성 합니다.
 
    [![](intro-to-cloudkit-images/image26a.png "번들 식별자 입력")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  맨 아래로 스크롤 합니다 **Info.plist** 파일을 선택 **백그라운드 모드 사용**를 **위치 업데이트** 및 **Remote Notifications**:

    [![](intro-to-cloudkit-images/image27a.png "백그라운드 모드, 위치 업데이트 및 원격 알림을 선택에 사용 하도록 설정")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  선택한 솔루션에 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 **옵션**합니다.
5.  선택 **iOS 번들 서명**를 선택 합니다 **개발자 Id** 하 고 **프로 비전 프로필** 위에서 만든 합니다.
6.  확인 합니다 **Entitlements.plist** 포함 **iCloud를 사용 하도록 설정** 에 **키-값 저장소** 및 **CloudKit** 합니다.
7.  확인 합니다 **보편화 컨테이너** (위에서 만든) 하는 동안 응용 프로그램에 대 한 존재 합니다. 예: `iCloud.com.your-company.CloudKitAtlas`
8.  파일의 변경 내용을 저장합니다.


현재 위치에서 이러한 설정을 사용 하 여 응용 프로그램 CloudKit 프레임 워크 Api에 액세스할 준비가 되었습니다.

## <a name="cloudkit-api-overview"></a>CloudKit API 개요

CloudKit Xamarin iOS 응용 프로그램에서을 구현 하기 전에이 문서에서는 다음 항목을 포함 하는 CloudKit 프레임 워크의 기본 사항에 맞게 것:

1.  **컨테이너** – iCloud 통신 사일로 격리 합니다.
2.  **데이터베이스** -공용 및 개인 응용 프로그램에 사용할 수 있습니다.
3.  **레코드** – CloudKit에서 구조화 된 데이터는 이동 하는 메커니즘입니다.
4.  **레코드 영역** – 레코드의 그룹입니다.
5.  **레코드 식별자** – 완전히 정규화 됩니다 하 고 레코드의 특정 위치를 나타냅니다.
6.  **참조** -부모-자식 관계는 특정된 데이터베이스 내 관련된 레코드를 제공 합니다.
7.  **자산** – 대용량의 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 연결 된 파일에 대 한 허용 합니다.


### <a name="containers"></a>컨테이너

IOS 장치에서 실행 중인 지정된 된 응용 프로그램은 항상 실행 함께 다른 응용 프로그램 및 해당 장치의 서비스 쪽입니다. 클라이언트 장치에서 응용 프로그램 사일로 또는 어떤 방식으로든에서 샌드박스가 적용 된 것입니다. 경우에 따라이 리터럴 샌드박스에서 이며, 기타 응용 프로그램은를 실행 하면 자체 메모리 공간입니다.

클라이언트 응용 프로그램을 가져와 다른 클라이언트에서 구분 된 실행의 개념 기능은 매우 강력 하며 다음과 같은 이점을 제공 합니다.

1.  **보안** – 응용 프로그램 다른 클라이언트 앱 또는 OS 자체를 방해할 수 없습니다.
1.  **안정성** 클라이언트 응용 프로그램이 충돌할 때 – 운영 체제의 다른 앱에 사용할 수 없습니다.
1.  **개인 정보 보호** – 각 클라이언트 응용 프로그램에 장치에 저장 된 개인 정보에 대 한 액세스를 제한 합니다.


CloudKit 위에 나와 있는 대로 동일한 이점을 제공 하 고 클라우드 기반 정보를 사용 하 여 작업에 적용 하도록 설계 되었습니다.

 [![](intro-to-cloudkit-images/image31.png "CloudKit 앱 컨테이너를 사용 하 여 통신")](intro-to-cloudkit-images/image31.png#lightbox)

응용 프로그램은 마찬가지로 일 대 다 장치에서 실행 되는 따라서 iCloud의 일 대 다를 사용 하 여 응용 프로그램의 통신입니다. 이러한 다양 한 통신 사일로의 각 컨테이너 라고 합니다.

컨테이너를 통해 CloudKit 프레임 워크에서 제공 되는 `CKContainer` 클래스입니다. 기본적으로 하나의 컨테이너 및이 컨테이너에 응용 프로그램 명령에서는 해당 응용 프로그램에 대 한 데이터를 분리 합니다. 즉, 여러 응용 프로그램이 동일한 iCloud 계정에 정보를 저장 될 수는 아직이 정보는 혼합할 수 없습니다.

ICloud 데이터의 컨테이너 화 CloudKit 사용자 정보를 캡슐화 할 수 있습니다. 이러한 방식으로 응용 프로그램은 iCloud 계정에 대 한 일부 제한 된 액세스 있고 사용자의 보안과 개인 정보를 보호 하면서 모든 내에 사용자 정보를 저장 합니다.

컨테이너의 WWDR 포털을 통해 응용 프로그램 개발자가 완벽 하 게 관리 됩니다. 컨테이너의 네임 스페이스 이므로 모든 Apple 개발자가 글로벌 컨테이너만 되지 않아야 지정된 개발자의 응용 프로그램에 고유 하지만 모든 Apple 개발자 및 응용 프로그램입니다.

Apple 응용 프로그램 컨테이너에 대 한 네임 스페이스를 만들 때 역방향 DNS 표기법을 사용 하도록 제안 합니다. 예제:

```csharp
iCloud.com.company-name.application-name
```

컨테이너는, 기본적으로 바인딩된 일대일로 지정된 된 응용 프로그램, 응용 프로그램 간에 공유할 수 있습니다. 따라서 여러 응용 프로그램이 단일 컨테이너에 대해 조정할 수 있습니다. 단일 응용 프로그램을 여러 컨테이너도 통신할 수 있습니다.

### <a name="databases"></a>데이터베이스

ICloud 서버까지 모델 응용 프로그램의 데이터 모델 및 복제를 수행할 방법은 CloudKit의 주요 기능 중 하나입니다. 일부 정보는 만든 사용자를 위한, 다른 정보는 사용자 (예: 음식점 리뷰의 경우) 공개적으로 사용 하 여 생성 될 수 있는 공용 데이터 또는 정보를 개발자가 응용 프로그램에 대 한 게시 수 있습니다. 두 경우 모두 대상은 단일 사용자 없습니다 있지만 사람들의 커뮤니티 있습니다.

 [![](intro-to-cloudkit-images/image32.png "CloudKit 컨테이너 다이어그램")](intro-to-cloudkit-images/image32.png#lightbox)

무엇 보다도 컨테이너 내부 공용 데이터베이스가입니다. 공용 정보를 모두와 공동 mingles입니다. 또한 응용 프로그램의 각 사용자에 대 한 개별 개인 데이터베이스를 몇 가지 있습니다.

IOS 장치에서 실행 하는 경우 응용 프로그램에만 iCloud 현재 로그인 한 사용자에 대 한 정보에 액세스를 해야 합니다. 따라서 컨테이너의 응용 프로그램의 보기는 다음과 같을 수 됩니다.

 [![](intro-to-cloudkit-images/image33.png "컨테이너의 응용 프로그램 보기")](intro-to-cloudkit-images/image33.png#lightbox)

Public 데이터베이스와 iCloud 현재 로그인 한 사용자와 연결 된 개인 데이터베이스에만 볼 수 있습니다.

데이터베이스를 통해 CloudKit 프레임 워크에서 제공 되는 `CKDatabase` 클래스입니다. 모든 응용 프로그램에 두 개의 데이터베이스에 대 한 액세스: 공용 데이터베이스 및 개인 것입니다.

컨테이너는 CloudKit의 초기 진입점입니다. 다음 코드는 응용 프로그램의 기본 컨테이너에서에서 공용 및 개인 데이터베이스 액세스에 사용할 수 있습니다.

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

데이터베이스 형식 간의 차이점은 다음과 같습니다.

||Public 데이터베이스|전용 데이터베이스|
|---|--- |--- |
|**데이터 형식**|공유 데이터|현재 사용자의 데이터|
|**Quota**|개발자의 할당량에 대 한 고려|사용자의 할당량에 대 한 고려|
|**기본 권한**|읽을 수 있는 전 세계|읽을 수 있는 사용자|
|**사용 권한 편집**|레코드 클래스 수준을 통해 iCloud 대시보드 역할|해당 사항 없음|

### <a name="records"></a>레코드

데이터베이스 및 데이터베이스 내에서 레코드가 보유 하는 컨테이너입니다. 레코드를 CloudKit에서 구조화 된 데이터 이동 메커니즘은 같습니다.

 [![](intro-to-cloudkit-images/image34.png "데이터베이스 및 데이터베이스 내에서 레코드가 보유 하는 컨테이너")](intro-to-cloudkit-images/image34.png#lightbox)

레코드를 통해 CloudKit 프레임 워크에서 제공 되는 `CKRecord` 키-값 쌍을 래핑하는 클래스입니다. 응용 프로그램에서 개체의 인스턴스는 해당 하는 `CKRecord` CloudKit의 합니다. 또한 각 `CKRecord` 해당 하는 개체의 클래스는 레코드 유형을 소유 합니다.

레코드는 데이터 처리를 위해 전달 되 고 전에 CloudKit에 설명 되어 있으므로 적시에 스키마를 가집니다. 해당 지점에서 CloudKit 정보를 해석를 저장 하 고 검색할 레코드의 물류를 처리 합니다.

`CKRecord` 클래스도 다양 한 메타 데이터를 지원 합니다. 예를 들어, 레코드를 만들 때와 생성 된 사용자에 대 한 정보를 포함 합니다. 또한 레코드를 마지막으로 수정 된 경우 및이 수정 하는 사용자에 대 한 정보를 포함 합니다.

레코드 변경 태그의 개념을 포함합니다. 지정된 된 레코드의 수정 버전의 이전 버전입니다. 변경 태그는 클라이언트와 서버에 동일한 버전 지정된 된 레코드의 경우를 결정 하는 간단한 방법으로 사용 됩니다.

위에서 설명한 것 처럼 `CKRecords` 키-값 쌍을 래핑하고 따라서 레코드에는 다음과 같은 유형의 데이터를 저장할 수 있습니다.

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


레코드는 단일 값 형식 외에도 위의 나열 된 형식의 형식이 배열을 포함할 수 있습니다.

새 레코드를 만들고 데이터베이스에 저장 하려면 다음 코드를 사용할 수 있습니다.

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>레코드 영역

레코드는 지정된 된 데이터베이스 내에서 단독으로 존재 하지 않습니다 – 레코드의 그룹 레코드 영역 내에서 함께 존재 합니다. 전통적인 관계형 데이터베이스에서 테이블로 레코드 영역 생각할 수 있습니다.:

 [![](intro-to-cloudkit-images/image35.png "레코드의 그룹이 함께 레코드 영역 내에서")](intro-to-cloudkit-images/image35.png#lightbox)

지정 된 레코드 영역 내에서 여러 레코드와 지정된 된 데이터베이스 내에서 여러 레코드 영역의 수 있습니다. 모든 데이터베이스는 기본 레코드 영역을 포함합니다.

 [![](intro-to-cloudkit-images/image36.png "기본 레코드 영역 및 사용자 지정 영역을 포함 하는 모든 데이터베이스")](intro-to-cloudkit-images/image36.png#lightbox)

이 기본적으로 레코드 저장 됩니다. 또한 사용자 지정 레코드 영역을 만들 수 있습니다. 레코드 영역 나타냅니다는 원자성 커밋 및 변경 내용 추적에서 기본 세분성에서 수행 됩니다.

## <a name="record-identifiers"></a>레코드 식별자

레코드 식별자는 튜플으로 표현 됩니다 것을 모두 포함 하는 클라이언트 제공 레코드 이름 및 레코드 존재 하는 영역. 레코드 식별자에는 다음과 같은 특징이 있습니다.

-  클라이언트 응용 프로그램에서 생성 됩니다.
-  완전히 정규화 하며 레코드의 특정 위치를 나타냅니다.
-  외부 데이터베이스에서 레코드의 고유 ID를 레코드 이름에 할당 하면 CloudKit 내의 저장 되지 않은 로컬 데이터베이스를 연결 데 사용할 수 있습니다.


개발자가 새 레코드를 만드는 경우 레코드 식별자를 전달 하도록 선택할 수 있습니다. 레코드 식별자를 지정 하지 않은 경우 UUID는 자동으로 생성 되며 레코드에 할당 합니다.

개발자가 새 레코드 식별자를 만들 때 각 레코드에 속하는 레코드 영역을 지정 하도록 선택할 수 있습니다. 지정 되지 않은 경우 기본 레코드 영역 사용 됩니다.

레코드 식별자를 통해 CloudKit 프레임 워크에서 제공 되는 `CKRecordID` 클래스입니다. 새 레코드 식별자를 만들려면 다음 코드를 사용할 수 있습니다.

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>참조 항목

참조는 지정된 된 데이터베이스 내에서 관련된 레코드 간의 관계를 제공합니다.

 [![](intro-to-cloudkit-images/image37.png "지정된 된 데이터베이스 내에서 관련된 레코드 간의 관계를 제공 하는 참조")](intro-to-cloudkit-images/image37.png#lightbox)

위의 예제에서는 부모 자식을 소유 하는 자식 부모 레코드의 자식 레코드. 관계 부모 레코드에 자식 레코드의 이동 및 라고 한 *다시 참조*합니다.

참조를 통해 CloudKit 프레임 워크에서 제공 되는 `CKReference` 클래스입니다. 이들은 iCloud 서버 레코드 간의 관계를 이해 하도록 하는 방법입니다.

참조 삭제 연계 숨어있는 메커니즘을 제공합니다. 부모 레코드를 데이터베이스에서 삭제 되 면 (지정 된 대로 관계에서) 모든 자식 레코드는 물론 데이터베이스에서 자동으로 삭제 됩니다.

> [!NOTE]
> 현 수 포인터 CloudKit을 사용 하는 경우 발생할 수 됩니다. 예를 들어 때쯤이면 응용 프로그램에 레코드 포인터 목록을 인출, 레코드를 선택한 다음 레코드 라는 메시지가 표시 될 수 없습니다 더 이상 데이터베이스에 레코드가 없습니다. 이 상황을 정상적으로 처리 하는 응용 프로그램 코딩 해야 합니다.

필요 없이 다시 참조 CloudKit 프레임 워크를 사용 하 여 작업 하는 경우 기본 설정 됩니다. Apple이 가장 효율적인 형식의 참조를 미세 조정 했습니다.

참조를 만드는 개발자는 이미 메모리에 있는 레코드를 제공 하거나 레코드 식별자에 대 한 참조를 만듭니다. 지정한 참조 및 레코드 식별자를 사용 하 여 데이터베이스에 존재 하지 않은 경우 현 수 포인터 만들어집니다.

다음은 알려진된 레코드에 대 한 참조를 만드는 예제입니다.

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>자산

자산에는 대용량의 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 연결 된 허용:

 [![](intro-to-cloudkit-images/image38.png "자산에는 대용량의 구조화 되지 않은 데이터를 iCloud에 업로드 하 고 지정된 된 레코드와 연결 된 허용")](intro-to-cloudkit-images/image38.png#lightbox)

클라이언트에서는 `CKRecord` 만들어집니다는 iCloud 서버로 업로드 하려는 파일에 설명 합니다. `CKAsset` 파일을 포함 하도록 생성 되 고이 설명 하는 레코드에 연결 되어 있습니다.

파일을 서버로 업로드 하는 경우 데이터베이스의 레코드 놓이고 특별 한 대량 저장소 데이터베이스에 파일이 복사 됩니다. 링크는 레코드 포인터 사이의 업로드 한 파일이 생성 됩니다.

자산 CloudKit framework를 통해 노출 되는 `CKAsset` 클래스 및 대용량의 구조화 되지 않은 데이터를 저장 하는 데 사용 됩니다. 개발자 대용량의 구조화 되지 않은 데이터를 메모리에 하려고 하지 않습니다, 때문에 자산 파일에서 디스크를 사용 하 여 구현 됩니다.

자산 자산 레코드를 사용 하 여 포인터와 iCloud에서 검색할 수 있습니다. 레코드에 의해 소유 됩니다. 이러한 방식으로 서버에서 자산을 소유 하는 레코드를 삭제할 때 가비지 수집 자산 수 있습니다.

때문에 `CKAssets` 은 Apple 설계 CloudKit을 효율적으로 업로드 하 고 자산 다운로드, 대용량 데이터 파일을 처리 합니다.

다음 코드는 자산을 만들고 레코드를 사용 하 여 연결을 사용할 수 있습니다.

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

CloudKit 내 기본 개체의 모든 설명 이제 했습니다. 컨테이너 응용 프로그램에 연결 되 고 데이터베이스를 포함 합니다. 데이터베이스는 레코드 영역으로 그룹화 되 고 레코드 식별자가 가리키는 레코드를 포함 합니다. 참조를 사용 하 여 레코드 간의 부모-자식 관계 정의 됩니다. 마지막으로, 큰 파일 업로드 및 자산을 사용 하 여 레코드를 연결할 수 있습니다.

## <a name="cloudkit-convenience-api"></a>CloudKit Convenience API

Apple에서는 CloudKit을 사용 하 여 작업에 대 한 두 가지 API 집합을 제공 합니다.

-  **Operational API** – CloudKit의 모든 단일 기능을 제공 합니다. 더 복잡 한 응용 프로그램의 경우이 API CloudKit 세부적으로 제어를 제공합니다.
-  **Api** – CloudKit 기능의 일반적인, 미리 구성 된 하위 집합을 제공 합니다. CloudKit 기능을 비롯 하 여 iOS 응용 프로그램에서 편리 하 고 쉬운 액세스 솔루션을 제공 합니다.


편리한 API는 일반적으로 대부분의 iOS 응용 프로그램에 적합 하 고 Apple 제안 된 시작 합니다. 이 단원의 나머지 부분에서는 다음 편리한 API 항목을 설명 합니다.

-  레코드를 저장 합니다.
-  레코드를 인출 합니다.
-  레코드를 업데이트 합니다.


### <a name="common-setup-code"></a>일반적인 설치 코드

CloudKit 편리한 API를 시작 하기 전에 필요한 표준 설치 코드가 있습니다. 응용 프로그램을 수정 하 여 시작 `AppDelegate.cs` 파일을 다음과 같이 표시 되도록 합니다.

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
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
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
        #endregion
    }
}
```

위의 코드는 쉽게 응용 프로그램의 나머지 부분에서 사용 하는 바로 가기 공용 및 개인 CloudKit 데이터베이스를 제공 합니다.

다음, 뷰 또는 CloudKit 사용 하는 컨테이너 보기에 다음 코드를 추가 합니다.

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

에 대 한 바로 가기를 추가 `AppDelegate` 위에서 만든 공용 및 개인 데이터베이스 바로 가기에 액세스 하 고 있습니다.

이 코드를 사용 하 여 Xamarin iOS 8 응용 프로그램에서 CloudKit 편리한 API를 구현 대해를 살펴보겠습니다.

### <a name="saving-a-record"></a>레코드를 저장합니다.

레코드에 설명할 때 위에서 설명한 패턴을 사용 하는 다음 코드 새 레코드를 만들고 편리한 API를 사용 하 여 공개 데이터베이스에 저장:

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

위의 코드에 대해 세 가지 사항은 다음과 같습니다.

1.  호출 하 여 합니다 `SaveRecord` 메서드는 `PublicDatabase`, 개발자는 데이터 전송 방법, 등으로 작성 되는 어떤 영역을 지정할 필요가 없습니다. 편리한 API 휴가 중 해당 세부 정보의 모든 자체입니다.
1.  비동기 호출과 호출이 완료 되 면 성공 또는 실패를 사용 하 여 콜백 루틴을 제공 합니다. 호출이 실패 하는 경우 오류 메시지가 제공 됩니다.
1.  CloudKit 로컬 저장소/지 속성; 제공 하지 않습니다. 이 전송 미디어만. 따라서에 레코드를 저장 하는 요청이 수행 될 때 iCloud 서버로 즉시 전송 됩니다.


> [!NOTE]
> 모바일 네트워크 통신, 연결 손실 되는 지속적으로 또는 중단 CloudKit 사용 하 여 작업 오류가 처리 되 면 개발자 확인 해야 하는 첫 번째 고려 사항 중 하나는 "손실" 때문입니다.

### <a name="fetching-a-record"></a>레코드를 가져오는 중

레코드 생성와 iCloud 서버에 성공적으로 저장을 사용 하 여 레코드를 검색 하려면 다음 코드를 사용 합니다.

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

레코드를 저장, 마찬가지로 위의 코드는 비동기가 단순 하며 좋은 오류 처리 합니다.

### <a name="updating-a-record"></a>레코드 업데이트

ICloud 서버에서 레코드를 가져온 후 레코드를 수정 하 고 변경 내용을 다시 데이터베이스에 저장 하려면 다음 코드를 사용할 수 있습니다.

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

`FetchRecord` 메서드를 `PublicDatabase` 반환을 `CKRecord` 호출이 성공한 경우. 그런 다음 응용 프로그램 수정 기록 및 호출 `SaveRecord` 다시 쓰기 변경 내용을 저장 하는 데이터베이스입니다.

이 섹션에서는 CloudKit 편리한 API를 사용 하는 경우 응용 프로그램에서 사용할 일반적인 주기를 보여 주었습니다. 응용 프로그램은 iCloud에 레코드를 저장, iCloud에서 해당 레코드를 검색, 레코드를 수정 및 iCloud로 다시 변경 내용을 저장 합니다.

## <a name="designing-for-scalability"></a>확장성을 위한 디자인

지금까지이 문서에 살펴보고 저장 하 고 사용할 것 때마다 iCloud 서버에서 응용 프로그램의 전체 개체 모델을 검색 합니다. 이 방법은 적은 양의 데이터 및 기본 매우 작은 사용자 잘 하는 동안이 잘 확장 되지 않습니다 정보 및/또는 사용자의 크기 증가 기반 하는 경우.

### <a name="big-data-tiny-device"></a>빅 데이터, 작은 장치

응용 프로그램 더 많이 사용 되 면 데이터베이스에 더 많은 데이터 및 장치에서 전체 데이터의 캐시는 덜 적합 합니다. 이 문제를 해결 하기 위해 다음 기술은 사용할 수 있습니다.

-  **클라우드에서 대용량 데이터를 유지** – CloudKit 큰 데이터를 효율적으로 처리 하도록 디자인 되었습니다.
-  **클라이언트는 해당 데이터 조각의 보기만** – 최소한 지정된 된 시간에 모든 작업을 처리 하는 데 필요한 데이터의 종료 합니다.
-  **클라이언트 보기를 변경할 수** – 각 사용자가 다른 기본 설정, 표시 되는 데이터 조각의 사용자를 변경할 수 있으며 사용자의 개별 때문에 지정 된 모든 조각의 보기 다를 수 있습니다.
-  **클라이언트 쿼리를 사용 하 여 관점 집중할** – 쿼리인 경우에 클라우드 내에서 존재 하는 큰 데이터 집합의 작은 하위 집합을 볼 수 있습니다.


### <a name="queries"></a>쿼리

위에서 설명한 대로 쿼리를 사용 하면 개발자가 클라우드에 있는 큰 데이터 집합의 작은 하위 집합을 선택 합니다. 쿼리를 통해 CloudKit 프레임 워크에서 제공 되는 `CKQuery` 클래스입니다.

세 가지를 결합 하는 쿼리: 레코드 형식 ( `RecordType`), 조건자 ( `NSPredicate`) 및 필요에 따라 정렬 설명자 ( `NSSortDescriptors`). CloudKit 지원 대부분이 `NSPredicate`합니다.

#### <a name="supported-predicates"></a>지원 되는 조건자

CloudKit 지원 다음과 같은 유형의 `NSPredicates` 쿼리로 작업 하는 경우:


1. 일치 레코드를 변수에 저장 된 값 같음:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. 키에 컴파일 시간에 알 수 없는 있도록 동적 키 값을 사용 하려면 일치 하는 것을 허용 합니다.
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. 일치 레코드의 값이 지정된 된 값 보다 크면 레코드:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. 일치 레코드 100 미터의 지정된 된 위치 내의 레코드의 위치는:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit은 토큰화 된 검색을 지원합니다. 이 호출에 대해 하나씩 두 개의 토큰 만들어집니다 `after` 성과 기록표가 `session`합니다. 이러한 두 토큰을 포함 하는 레코드를 반환 합니다.
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
6. CloudKit 지원 복합 사용 하 여 조인 조건자는 `AND` 연산자입니다.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>쿼리 작성

다음 코드를 만드는 데 사용할 수는 `CKQuery` Xamarin iOS 8 응용 프로그램에서:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

먼저 지정 된 이름과 일치 하는 레코드만 선택 하기 위한 조건자를 만듭니다. 그런 다음 조건자와 일치 하는 지정 된 레코드 형식의 레코드를 선택 하는 쿼리를 만듭니다.

#### <a name="performing-a-query"></a>쿼리를 수행합니다.

쿼리를 만든 후 쿼리를 수행 하 고 반환된 된 레코드를 처리 하려면 다음 코드를 사용 합니다.

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

위의 코드는 위에서 만든 쿼리 및 공용 데이터베이스에 대해 실행 합니다. 레코드 영역 없음 지정 되어 있으므로 모든 영역이 검색 됩니다. 오류가 발생 하지 않으면, 배열을 `CKRecords` 쿼리 매개 변수를 일치 하는 반환 됩니다.

쿼리에 대 한 생각 하는 방법은 해당 설문 조사, 고 큰 데이터 집합을 통해 조각화 하는 데 유용 합니다. 그러나 쿼리,은 적합 하지 않습니다 대규모의 대부분 정적 데이터 집합에 대 한 다음과 같은 이유로:

-  장치 배터리 수명에 대 한 잘못 된 경우
-  네트워크 트래픽에 대 한 잘못 된 경우
-  응용 프로그램 데이터베이스를 폴링하는 빈도에서 보는 정보는 제한 되어 있으므로 사용자 환경에 대 한 잘못 된 됩니다. 현재 사용자를 변경 하면 푸시 알림을 기대 합니다.


### <a name="subscriptions"></a>구독

대규모의 대부분 정적 데이터 집합을 처리할 때 쿼리 클라이언트 장치에서 수행 해야, 클라이언트 대신 서버에서 실행 해야 합니다. 쿼리는 백그라운드에서 실행 해야 하 고 현재 장치 또는 동일한 데이터베이스를 변경 하는 다른 장치에서 모든 단일 레코드를 저장 한 후 실행 합니다.

마지막으로 푸시 알림은 서버 쪽 쿼리를 실행할 때 데이터베이스에 연결 된 모든 장치로 전송 되어야 합니다.

구독을 통해 CloudKit 프레임 워크에서 노출 되는 `CKSubscription` 클래스입니다. 즉, 레코드 유형을 결합 ( `RecordType`), 조건자 ( `NSPredicate`) 및 Apple 푸시 알림 ( `Push`).

> [!NOTE]
> CloudKit 푸시 CloudKit 원인을 발생에 대 한 요구와 같은 특정 정보를 포함 하는 페이로드가 포함 되어 있으므로 약간 확대 됩니다.

#### <a name="how-subscriptions-work"></a>구독이 작동 하는 방법

구독을 구현 하기 전에 C# 코드에서 구독이 작동 하는 방법에 대해 간략히 살펴보겠습니다.

 [![](intro-to-cloudkit-images/image39.png "구독 작업 하는 방법에 대 한 개요")](intro-to-cloudkit-images/image39.png#lightbox)

위의 그래프는 다음과 같은 일반적인 구독 프로세스를 보여줍니다.

1.  클라이언트 장치에는 트리거가 발생 한 경우 전송 될 푸시 알림 및 구독을 트리거하는 조건의 집합을 포함 하는 새 구독을 만듭니다.
2.  구독은 데이터베이스에 전송 됩니다 기존 구독의 컬렉션에 추가 됩니다.
3.  두 번째 장치를 새 레코드를 만들고 데이터베이스에 해당 레코드를 저장 합니다.
4.  데이터베이스를 통해 새 레코드를 해당 조건 중 하나라도 일치 하는 경우에 대 한 구독 목록을 검색 합니다.
5.  일치 하는 항목이 없으면 트리거될 경우 레코드에 대 한 정보를 사용 하 여 구독을 등록 하는 장치에 푸시 알림이 전송 됩니다.


현재 위치에서이 정보를 통해 살펴보겠습니다 구독 Xamarin ios에서 8 응용 프로그램 만들기.

#### <a name="creating-subscriptions"></a>구독 만들기

구독을 만들려면 다음 코드를 사용할 수 있습니다.

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

먼저 구독을 트리거하는 조건을 제공 하는 조건자를 만듭니다. 다음으로, 특정 레코드 형식에 대해 구독 만들고 트리거를 테스트 하는 경우의 옵션을 설정 합니다. 마지막으로 구독 트리거되고 구독을 연결 하는 경우 발생 하는 알림 유형을 정의 합니다.

### <a name="saving-subscriptions"></a>구독 저장

만들어진 구독을 사용 하 여 다음 코드는 해당 데이터베이스에 저장:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

편리한 API를 사용 호출이 비동기를 간단 하 고 쉽게 오류 처리를 제공 합니다.

#### <a name="handling-push-notifications"></a>푸시 알림 처리

개발자가 이전에 Apple 푸시 알림 (AP)에서 사용 하는 경우 CloudKit에서 생성 되는 알림 처리 하는 프로세스 친숙 한 이어야 합니다.

에 `AppDelegate.cs`를 재정의 합니다 `ReceivedRemoteNotification` 다음과 같이 클래스:

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

위의 코드는 userInfo CloudKit 알림을 구문 분석 하는 CloudKit을 요청 합니다. 그런 다음 정보는 경고에 대 한 추출 됩니다. 마지막으로 알림 유형을 테스트 하 고 알림을 적절 하 게 처리 됩니다.

이 섹션에서는 빅 데이터, 쿼리 및 구독을 사용 하 여 위의 작은 장치 문제에 대답 하는 방법을 보여 주었습니다. 응용 프로그램 클라우드에서 대용량 데이터를 그대로 두고 이러한 기술을 사용 하 여이 데이터 집합에 뷰를 제공 합니다.

## <a name="cloudkit-user-accounts"></a>CloudKit 사용자 계정

이 문서에서는의 시작 부분에 설명 했 듯이 CloudKit 기존 iCloud 인프라를 기반으로 빌드됩니다. 다음 섹션에서는 다룰 것을 세부적으로 CloudKit API를 사용 하 여 개발자에 게 계정이 노출 되는 방법입니다.

### <a name="authentication"></a>인증

사용자 계정을 처리할 때 첫 번째 고려는 인증입니다. CloudKit 장치에서 iCloud에 현재 로그인 한 사용자를 통해 인증을 지원합니다. 인증은 백그라운드에서 수행 하 고 iOS에 의해 처리 됩니다. 따라서에서 개발자 신경을 쓸 필요가 인증 구현의 세부 사항에 대 한 합니다. 사용자가 로그온 하는 경우 참조에 테스트 합니다.

### <a name="user-account-information"></a>사용자 계정 정보

CloudKit은 개발자에 게 다음과 같은 사용자 정보를 제공합니다.

-  **Identity** – 고유 하 게 사용자를 식별 하는 방법입니다.
-  **메타 데이터** – 저장 하 고 사용자에 대 한 정보를 검색할 수 있습니다.
-  **개인 정보 보호** – 모든 정보는 개인 정보 보호 작동할지 manor에서 처리 합니다. 아무 것에 사용자가 동의 하지 않는 한도 노출 합니다.
-  **검색** – 사용자가 동일한 응용 프로그램을 사용 하는 친구 들을 검색 하는 기능을 제공 합니다.


그런 다음 이러한 항목을 자세히 살펴보겠습니다.

#### <a name="identity"></a>클레임

위에서 설명한 대로 CloudKit 응용 프로그램을 지정된 된 사용자를 고유 하 게 식별 하는 방법을 제공 합니다.

 [![](intro-to-cloudkit-images/image40.png "고유 하 게 파악 지정된 된 사용자")](intro-to-cloudkit-images/image40.png#lightbox)

사용자의 장치 및 모든 CloudKit 컨테이너 내에서 특정 사용자 전용 데이터베이스에서 실행 중인 클라이언트 응용 프로그램이 있습니다. 클라이언트 응용 프로그램은 해당 특정 사용자 중 하나에 연결 될 것입니다. 장치에 로컬로 iCloud에 로그인 되어 있는 사용자를 기반 합니다.

ICloud에서 제공 합니다 이므로 다양 한 저장소의 사용자 정보를 백업 합니다. 및 사용자를 상호 연결할 수 있으므로 실제로 iCloud 컨테이너 호스트를 합니다. 위의 그림에서, 사용자의 iCloud 계정 `user@icloud.com` 현재 클라이언트에 연결 됩니다.

컨테이너 별로 고유, 임의로 생성 된 사용자 ID 생성 되어 사용자의 iCloud 계정 (메일 주소)를 사용 하 여 연결 합니다. 이 사용자 ID는 응용 프로그램에 반환 되 고 어떤 방식으로 개발자에 게 표시에 맞게 사용할 수 있습니다.

> [!NOTE]
> 다른 CloudKit 컨테이너에 연결 되어 있으므로 동일한 iCloud 사용자에 대해 동일한 장치에서 실행 중인 다른 응용 프로그램 별로 다른 사용자 Id 해야 합니다.

다음 코드 가져오고 CloudKit 사용자 ID는 현재 로그인에 대 한 장치에서 iCloud 사용자:

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

위의 코드를 현재 로그인된 한 사용자의 ID를 제공 하려면 CloudKit 컨테이너를 묻습니다. 이 정보를 icloud와 서버에서에서 들어오는 이후 호출이 비동기 인지 및 오류 해결이 필요 합니다.

#### <a name="metadata"></a>메타데이터

CloudKit의 각 사용자는 이러한 기능을 설명 하는 특정 메타 데이터를 가집니다. 이 메타 데이터는 CloudKit 레코드로 표시 됩니다.

 [![](intro-to-cloudkit-images/image41.png "CloudKit의 각 사용자가 이러한 기능을 설명 하는 특정 메타 데이터")](intro-to-cloudkit-images/image41.png#lightbox)

있는 컨테이너의 특정 사용자에 대 한 개인 데이터베이스 내에서 검색 하는 것은 해당 사용자를 정의 하는 하나의 레코드입니다. 데이터베이스 내부에 공용 컨테이너의 각 사용자에 대 한 사용자 레코드를 수 있습니다. 현재 로그온 한 사용자의 레코드 ID와 일치 하는 레코드 ID가는 중

공용 데이터베이스의 사용자 레코드를 읽을 수 있는 전 세계 에 대 한 대부분의 경우 일반 레코드로 처리 되 고 형식이 해당 `CKRecordTypeUserRecord`합니다. 이러한 레코드는 시스템에서 예약 된 되며 쿼리에 사용할 수 없습니다.

사용자 레코드에 액세스 하는 다음 코드를 사용 합니다.

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

위의 코드에서는 위의 액세스 하는 ID 인 사용자에 대 한 사용자 레코드를 반환할 공용 데이터베이스를 묻습니다. 이 정보를 icloud와 서버에서에서 들어오는 이후 호출이 비동기 인지 및 오류 해결이 필요 합니다.

#### <a name="privacy"></a>개인 정보 취급 방침

CloudKit 기본적으로 현재 로그온된 한 사용자의 개인 정보를 보호 하기 위해 디자인 되었습니다. 기본적으로 사용자에 대 한 개인 식별 정보가 없습니다 노출 됩니다. 응용 프로그램 사용자에 대 한 정보를 제한 해야 하는 경우도 있습니다.

이러한 경우 응용 프로그램 사용자는이 정보를 공개할 요청할 수 있습니다. 해당 계정 정보를 노출 하는 데 참여할 하 라는 사용자에 게 대화 상자가 표시 됩니다.

#### <a name="discovery"></a>검색

응용 프로그램을 허용 하도록 옵트인 사용자가 자신의 사용자 계정 정보에 대 한 액세스 제한, 가정 응용 프로그램의 다른 사용자에 게 검색할 수 있습니다.

 [![](intro-to-cloudkit-images/image42.png "사용자는 응용 프로그램의 다른 사용자에 게 검색할 수 있습니다.")](intro-to-cloudkit-images/image42.png#lightbox)

클라이언트 응용 프로그램을 컨테이너와 통신 하 고 사용자 정보에 액세스할 수 있는 iCloud 컨테이너와 통신 합니다. 사용자 전자 메일 주소를 제공할 수 있습니다 하 고 다시 사용자에 대 한 정보 검색을 사용할 수 있습니다. 필요에 따라 사용자 ID 사용자에 대 한 정보를 검색할 수 있습니다.

CloudKit 현재 로그인 된 친구 수 있는 모든 사용자에 대 한 정보를 검색 하는 방법을 제공 전체 주소록을 쿼리하여 iCloud 사용자입니다. CloudKit 프로세스는 사용자의 연락처 책에서 끌어오기를 다른 사용자가 해당 주소와 일치 하는 응용 프로그램의 경우 확인할 수 있습니다에 전자 메일 주소를 사용 합니다.

따라서이에 대 한 액세스를 제공 하거나 사용자에 게 연락처에 대 한 액세스를 승인 하 없이 사용자의 연락처 책을 활용 하 여 응용 프로그램. 순식간에는 연락처 정보를 사용할 수 있게 응용 프로그램을 CloudKit 프로세스에 대 한 액세스.

정리 하자면, 세 가지 다른 종류의 입력 사용자 검색에 사용할 수 있는:

-  **사용자 레코드 ID** – 현재 로그인 된 사용자 ID에 대해 검색을 수행할 수 있습니다 CloudKit 사용자에서입니다.
-  **사용자 전자 메일 주소** – 사용자 전자 메일 주소를 제공할 수 있습니다 하 고 검색에 사용할 수 있습니다.
-  **책 문의** – 사용자의 주소록 응용 프로그램의 해당 연락처에 나열 된 동일한 전자 메일 주소를가지고 있는 사용자를 검색할 수 있습니다.


사용자 검색에는 다음 정보를 반환 합니다.

-  **사용자 레코드 ID** -공용 데이터베이스에서 사용자의 고유 ID입니다.
-  **첫 번째 이름과 마지막** -공용 데이터베이스에 저장 합니다.


이 정보에 선택한 검색에 속하는 사용자만 반환 됩니다.

다음 코드는 장치에서 iCloud에 현재 로그인 한 사용자에 대 한 정보를 검색 합니다.

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

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

이 단원의 CloudKit 응용 프로그램에 제공할 수 있는 사용자의 계정에 대 한 액세스의 네 가지 주요 영역은 설명 했습니다. 사용자의 Id 및 메타 데이터 가져오기, 개인 정보 보호 정책에 기본 제공 되는 CloudKit을 마지막으로, 응용 프로그램의 다른 사용자를 검색 하는 기능입니다.

## <a name="the-development-and-production-environments"></a>개발 및 프로덕션 환경

CloudKit 응용 프로그램의 레코드 형식 및 데이터에 대 한 별도 개발 및 프로덕션 환경을 제공합니다. 개발 환경에는 개발 팀의 멤버에만 사용할 수 있는 유연한 환경입니다. 레코드에 새 필드를 추가 하 고 개발 환경에서 해당 레코드를 저장 하는 응용 프로그램, 서버 스키마 정보를 자동으로 업데이트 합니다.

개발자는 개발 시간을 저장 하는 동안 스키마를 변경 하려면이 기능을 사용 수 있습니다. 한 가지 주의할은 레코드에 필드를 추가한 후 필드와 연결 된 데이터 형식을 변경할 수 없습니다 프로그래밍 방식으로입니다. 개발자는 필드의 유형을 변경 하려면 필드를 삭제 해야 합니다 [CloudKit 대시보드](https://icloud.developer.apple.com/dashboard/) 새 형식을 사용 하 여 다시 추가 합니다.

응용 프로그램을 배포 하기 전에 개발자 마이그레이션하려면 해당 스키마 및 데이터 사용 하 여 프로덕션 환경 **CloudKit 대시보드**합니다. 프로덕션 환경에 대해 실행 하는 경우 서버 응용 프로그램을 프로그래밍 방식으로 스키마 변경에서 방지 합니다. 개발자의 변경 내용을 사용할 수 **CloudKit 대시보드** 하지만 오류는 프로덕션 환경에서 레코드에 필드를 추가 하려고 합니다.

> [!NOTE]
> IOS 시뮬레이터 에서만 작동 합니다 **개발 환경**합니다. 개발자의 응용 프로그램을 테스트할 준비가 경우는 **프로덕션 환경**, 실제 iOS 장치는 필요 합니다.


## <a name="shipping-a-cloudkit-enabled-app"></a>앱을 사용 하는 CloudKit 전달

CloudKit을 사용 하는 응용 프로그램 전달 하기 전에 구성 해야 합니다 대상에는 **프로덕션 CloudKit 환경** 또는 응용 프로그램은 Apple에서 거부 됩니다.

다음을 수행합니다.

1. Ma에 대해 Visual Studio에서 응용 프로그램을 컴파일할 **릴리스** > **iOS 장치**: 

    [![](intro-to-cloudkit-images/shipping01.png "릴리스할 응용 프로그램 컴파일")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. **빌드할** 메뉴에서 **보관**: 

    [![](intro-to-cloudkit-images/shipping02.png "보관 선택")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. 합니다 **보관** 만들어지고 Mac 용 Visual Studio에 표시 됩니다. 

    [![](intro-to-cloudkit-images/shipping03.png "보관 파일을 만들고 표시")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. **Xcode**를 시작합니다.
5. **창을** 메뉴에서 **도우미**: 

    [![](intro-to-cloudkit-images/shipping04.png "구성 도우미를 선택 합니다.")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. 응용 프로그램의 보관 파일을 선택 하 고 클릭는 **내보내기...**  단추: 

    [![](intro-to-cloudkit-images/shipping05.png "응용 프로그램의 보관 파일")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. 내보내기에 대 한 메서드를 선택 하 고 클릭 합니다 **다음** 단추: 

    [![](intro-to-cloudkit-images/shipping06.png "내보내기에 대 한 메서드를 선택 합니다.")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. 선택 된 **개발팀** 클릭 하 고 드롭다운 목록에서 합니다 **선택** 단추: 

    [![](intro-to-cloudkit-images/shipping07.png "드롭다운 목록에서 개발 팀을 선택 합니다.")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. 선택 **프로덕션** 클릭 하 고 드롭다운 목록에서 합니다 **다음** 단추: 

    [![](intro-to-cloudkit-images/shipping08.png "드롭다운 목록에서 프로덕션을 선택 합니다.")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. 설정 및 클릭을 검토 합니다 **내보내기** 단추: 

    [![](intro-to-cloudkit-images/shipping09.png "설정 검토")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. 응용 프로그램을 생성 하는 위치를 선택 `.ipa` 파일입니다.

ITunes Connect에 직접 응용 프로그램을 제출 하는 것에 대 한 프로세스도 비슷합니다, 클릭는 **제출 하는 중...**  대신 구성 도우미 창에서 보관 파일을 선택한 후... 내보내기 단추입니다.

## <a name="when-to-use-cloudkit"></a>CloudKit을 사용 하는 경우

이 문서에서 살펴본 대로 CloudKit 쉽게 저장 하 고 iCloud 서버에서 정보를 검색할 응용 프로그램을 제공 합니다. 즉, CloudKit는 사용 되지 않는 않거나 기존 도구 또는 프레임 워크의 사용을 중단 합니다.

### <a name="use-cases"></a>사용 사례

다음 사용 사례를 특정 iCloud 프레임 워크 또는 기술을 사용 하는 경우를 결정 하는 개발자는 데 도움이 됩니다.

-  **iCloud 키-값 저장소** – 비동기적으로 적은 양의 데이터를 최신 상태로 유지 하 고 응용 프로그램의 기본 작업에 유용 합니다. 그러나 매우 적은 양의 정보에 대 한 제한 됩니다.
-  **iCloud 드라이브** – 기존 iCloud 문서 Api 기반 및 파일 시스템에서 구조화 되지 않은 데이터를 동기화 하는 간단한 API를 제공 합니다. Mac OS X에서 전체 오프 라인 캐시를 제공 하 고 문서 중심 응용 프로그램에 적합 합니다.
-  **iCloud 핵심 데이터** – 데이터의 모든 사용자의 장치 간에 복제할 수 있습니다. 데이터가 단일 사용자와 동기화 하는 유지 개인의 구조화 된 데이터에 적합 합니다.
-  **CloudKit** – 공용 데이터 모두 구조를 제공 하 고 대량 및 큰 데이터 집합 및 큰 구조화 되지 않은 파일을 처리할 수 있습니다. 해당 연결 된 사용자에 게 제공와 iCloud 계정 클라이언트 데이터 전송을 제어 합니다.


이러한 유지에 사용 사례, 개발자는 현재 필요한 응용 프로그램 기능을 제공 하 고 향후 성장을 위한 우수한 확장성을 제공 하려면 올바른 iCloud 기술을 선택 해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 CloudKit API에 대 한 간단한 소개를 설명 했습니다. 이 프로 비전 및 CloudKit을 사용 하려면 Xamarin iOS 응용 프로그램을 구성 하는 방법을 보여 주었습니다. CloudKit 편의 API의 기능에서는 이러한 부분이 있습니다. 표시에는 CloudKit을 디자인 하는 방법을 쿼리 및 구독을 사용 하 여 확장성을 위해 응용 프로그램을 사용 하도록 설정 합니다. 및 CloudKit에서 응용 프로그램에 노출 되는 사용자 계정 정보에 마지막 표시 된 합니다.

## <a name="related-links"></a>관련 링크

- [CloudKitAtlas (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [프로 비전 프로필 만들기](~/ios/get-started/installation/device-provisioning/index.md)
