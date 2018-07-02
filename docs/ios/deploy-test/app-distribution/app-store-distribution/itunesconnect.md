---
title: iTunes Connect에서 앱 구성
description: 이 문서에서는 앱 스토어에서 배포를 위해 릴리스될 수 있도록 iTunes Connect에서 Xamarin.iOS 응용 프로그램을 설정하고 유지 관리하는 데 필요한 단계를 설명합니다.
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 05492f866bb083326ef1eccb8db3d624d8dc4806
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209312"
---
# <a name="configuring-an-app-in-itunes-connect"></a>iTunes Connect에서 앱 구성

> [!IMPORTANT]
> Apple은 2018년 7월부터 App Store에 제출된 모든 앱과 업데이트가 iOS 11 SDK로 빌드되었고 [iPhone X 디스플레이를 지원](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md)한다고 [발표](https://developer.apple.com/news/?id=05072018a)했습니다.

iTunes Connect는 특히 앱 스토어에서 iOS 응용 프로그램을 관리하는 웹 기반 도구 모음입니다. 먼저 iTunes Connect에서 Xamarin.iOS 응용 프로그램을 제대로 설정하고 구성한 후에, 이를 검토하여 궁극적으로 앱 스토어에서 판매하거나 무료 앱으로 릴리스할 수 있도록 Apple에 제출해야 합니다.

iTunes Connect는 다음과 같은 용도로 사용할 수 있습니다.

- 응용 프로그램 이름을 설정합니다(앱 스토어에 표시됨).
- 지원하는 iOS 장치에 작동 중인 응용 프로그램의 스크린샷 또는 비디오를 제공합니다.
- 최종 사용자에게 제공되는 기능 및 혜택을 포함하여 응용 프로그램에 대한 명확하고 간결한 설명을 제공합니다.
- 사용자가 앱 스토어에서 앱을 찾는 데 도움이 되는 범주와 하위 범주를 제공합니다.
- 사용자가 앱을 찾는 데 도움이 될 수 있는 키워드를 제공합니다.
- Apple에서 요구하는 웹 사이트에 대한 연락처 및 지원 URL을 제공합니다.
- 앱 스토어에서 자녀를 보호하도록 부모에게 알리는 데 사용되는 앱 등급을 설정합니다.
- 판매 가격을 선택하거나 응용 프로그램이 무료로 릴리스되도록 지정합니다.
- Game Center 및 인앱 구매와 같은 선택적인 앱 스토어 기술을 구성합니다.

또한 Apple이 앱 스토어에서 차별화되도록 앱의 특징을 결정하는 경우에 대비하여 앱에서 매력적인 고해상도 아트워크를 사용할 수 있어야 합니다. 자세한 내용은 Apple의 [iTunes Connect 개발자 가이드](https://developer.apple.com/support/itunes-connect/)를 참조하세요.

## <a name="managing-agreements-tax-and-banking"></a>계약, 세금 및 뱅킹 관리

iTunes Connect의 **계약, 세금 및 뱅킹** 섹션은 iTunes 개발자 지불 및 원천 징수와 관련된 필수 재무 정보를 제공하고 Apple과 체결한 계약 상태를 추적하는 데 사용됩니다. 앱 스토어에서 iOS 응용 프로그램을 무료 또는 판매용으로 릴리스하려면, 먼저 적절한 계약을 체결하고 기존 계약을 수정하는 데 동의해야 합니다.

[![](itunesconnect-images/agreement01.png "계약, 세금 및 뱅킹 관리")](itunesconnect-images/agreement01.png#lightbox)

여기서는 다음을 수행할 수 있습니다.

- **팀 에이전트**를 제공하고 **관리자** 또는 **재무**와 같은 iTunes Connect 계정에 대한 다른 사용자 역할을 정의합니다.
- 조직이 앱 스토어에서 응용 프로그램을 배포할 수 있도록 **계약**을 등록하고 유지 관리합니다.
- 조직과 관련된 계약, 뱅킹 정보 및 세금 정보를 일치시키는 데 사용되는 **법인** 이름(앱 스토어의 판매자 이름)을 지정합니다.
- 앱 스토어를 통해 응용 프로그램을 판매하려는 경우 **뱅킹** 및 **세금** 정보를 제공합니다.

다시 말하지만, iOS 응용 프로그램을 검토하고 릴리스하기 위해 iTunes Connect에 제출하기 전에 _이 정보를 올바르게 설정, 적용 및 최신 상태로 유지해야 합니다_. 자세한 내용은 Apple의 [계약, 세금 및 뱅킹 관리](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1) 설명서를 참조하세요.

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>iTunes Connect 레코드 만들기

앱 스토어를 통해 배포하기 위해 Xamarin.iOS 응용 프로그램을 iTunes Connect에 업로드하려면, 먼저 응용 프로그램에 대한 레코드를 iTunes Connect에 만들어야 합니다. 이 레코드에는 앱 스토어에서 표시되는 응용 프로그램에 대한 모든 정보(필요에 따라 다양한 언어 사용) 및 배포 프로세스를 통해 앱을 관리하는 데 필요한 모든 정보가 포함됩니다. 또한 iAd App Network 또는 Game Center와 같은 앱 스토어 기술을 구성하는 데도 사용됩니다.

iOS 응용 프로그램을 iTunes Connect에 추가하려면 **팀 에이전트**이거나 **관리자** 또는 **기술** 역할이 있는 사용자여야 합니다.

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.

    [![](itunesconnect-images/add01.png "My Apps 클릭")](itunesconnect-images/add01.png#lightbox)
2. 왼쪽 위 모서리에서 **+** 를 클릭하고 **새 iOS 앱**을 선택합니다.

    [![](itunesconnect-images/add02.png "새 iOS 앱 추가")](itunesconnect-images/add02.png#lightbox)
3. iTunes Connect에서 **새 iOS 앱** 대화 상자가 표시됩니다.

    [![](itunesconnect-images/add03.png "새 iOS 앱 대화 상자")](itunesconnect-images/add03.png#lightbox)
4. 앱 스토어에 표시되어야 하는 응용 프로그램에 대한 **이름** 및 **버전 번호**를 입력합니다.
5. **기본 언어**를 선택합니다.
6. **SKU** 번호를 입력합니다. 이 번호는 응용 프로그램을 추적하는 데 사용되는 고유한 상수 식별자입니다. 최종 사용자에게는 표시되지 않으며, 앱을 만든 후에는 _변경할 수 없습니다_.
7. 응용 프로그램을 프로비전할 때 개발자 센터에서 만든 응용 프로그램에 대한 **번들 ID**를 선택합니다. Mac용 Visual Studio 또는 Visual Studio에서 배포 번들에 서명할 때 이 동일한 번들 ID를 사용해야 합니다. 자세한 내용은 [배포 프로필 만들기](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) 및 [Xamarin.iOS 프로젝트에서 배포 프로필 선택](~/ios/get-started/installation/device-provisioning/index.md) 설명서를 참조하세요.
8. **만들기** 단추를 클릭하여 응용 프로그램에 대한 새 iTunes Connect 레코드를 작성합니다.

새 응용 프로그램이 iTunes Connect에 만들어지고, 설명, 가격, 범주, 등급 등과 같은 필수 정보를 입력할 준비가 됩니다.

[![](itunesconnect-images/add04.png "새 응용 프로그램이 iTunes Connect에 만들어집니다.")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>앱 비디오 및 스크린샷 관리

앱 스토어에서 iOS 응용 프로그램을 성공적으로 마케팅하기 위한 가장 중요한 요소 중 하나는 훌륭한 스크린샷 집합이며, 비디오 미리 보기는 선택 사항입니다. 사용자 상호 작용을 강조하고 고유한 기능을 보여 주는 응용 프로그램 실행의 실제 보기를 사용합니다. 응용 프로그램 미리 보기 비디오를 사용하여 사용자에게 응용 프로그램을 사용하는 것과 같은 느낌을 제공합니다.

스크린샷을 만들 때 Apple에서 권장하는 제안 사항은 다음과 같습니다.

- 응용 프로그램에서 지원하는 iOS 장치에서 최상의 프레젠테이션을 제공하도록 스크린샷을 최적화하고, 콘텐츠를 읽을 수 있는지 확인합니다.
- iOS 장치 이미지에서 스크린샷을 프레이밍하지 않습니다.
- 스크린샷 둘레에 그래픽이나 테두리 없이 전체 화면을 사용하여 응용 프로그램의 실제 보기를 표시합니다.
- 항상 스크린샷에서 상태 표시줄을 제거합니다. 그러면 iTunes Connect에서 해당 영역을 제외한 크기의 스크린샷이 제공됩니다.
- 가능한 경우 iOS 시뮬레이터 대신 실제 고해상도 Retina iOS 하드웨어( 제외)에서 스크린샷을 만듭니다.
- 앱 미리 보기 비디오를 사용할 수 없는 경우 첫 번째 스크린샷이 iPhone 및 iPad 앱 스토어에서 검색 결과로 표시되므로 최상의 스크린샷을 먼저 배치합니다.
- 5개의 스크린샷을 모두 사용하여 응용 프로그램의 스토리를 보여 주는 동시에 해당 응용 프로그램을 매력으로 만드는 순간을 강조합니다.

Apple에서는 응용 프로그램이 지원하는 모든 화면 크기와 해상도의 스크린샷과 비디오를 제공하도록 요구합니다. 또한 지원되는 방향에 따라 가로 및 세로 버전을 제공해야 합니다.

현재 요구되는 화면 크기와 해상도는 다음과 같습니다.

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>iTunes Connect에서 스크린샷 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **버전** 탭을 선택합니다.
4. **스크린샷** 섹션으로 스크롤합니다.
5. **이미지 크기**를 선택하고 필요한 이미지를 끌어옵니다(화면 크기당 최대 5개).

    [![](itunesconnect-images/screenshot01.png "이미지 크기를 선택하고 필요한 이미지 끌어오기")](itunesconnect-images/screenshot01.png#lightbox)
6. 필요한 모든 화면 크기에 대해 반복합니다.
7. 화면 위쪽의 **저장** 단추를 클릭하여 변경 내용을 저장합니다.

> [!NOTE]
> 참고: 스크린샷 또는 앱 미리 보기 비디오가 응용 프로그램의 현재 기능과 일치하지 않는 경우 Apple에서 제출을 거부합니다.

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>이름, 설명, 새로운 기능, 키워드 및 URL 관리

iTunes Connect 응용 프로그램 레코드의 이 섹션에서는 응용 프로그램에 대해 지역화된 정보, 기능, 새 버전에 대한 수정 사항, 검색 및 iAd 지원에 사용되는 키워드 및 지원하는 URL을 제공합니다.

### <a name="app-name"></a>앱 이름

응용 프로그램에서 수행하는 작업을 반영하는 설명이 포함된 응용 프로그램 이름을 선택합니다. 가능하면 짧고 간결하게 유지합니다. 응용 프로그램 이름은 사용자가 검색하는 방법에 중요한 역할을 하므로 이름을 간단하고 기억하기 쉽게 유지합니다. 이름이 iOS 장치(iPad, iPhone 및 iPod touch)에서 표시되는 방법에 특히 주의해야 합니다.

응용 프로그램 이름을 선택할 때 Apple에서 제안하는 지침은 다음과 같습니다.

- 짧고 간단하며 기억하기 쉽게 유지합니다.
- 타사의 저작권 또는 상표를 위반하지 않는지 확인합니다.
- 응용 프로그램의 기능과 일치하도록 작성합니다.
- 적절한 경우 해외 시장에 대해 지역화된 이름을 제공합니다.

### <a name="description"></a>설명

응용 프로그램과 해당 기능에 대해 명확하고 간결하며 유용한 설명을 작성합니다. 처음 몇 줄이 가장 중요하며 사용자에게 훌륭한 첫 인상을 주고 이들을 끌어들일 수 있는 기회를 제공합니다. 응용 프로그램을 차별화하고 다른 비슷한 앱과 구분되는 특징을 설명합니다.

Apple에서 응용 프로그램 설명을 작성하는 데 권장하는 제안 사항은 다음과 같습니다.

- 간략한 도입 단락 또는 두 단락 및 주요 기능에 대한 짧은 글머리 기호 목록을 포함합니다.
- 적절한 경우 해외 시장에 대해 지역화된 설명을 제공합니다.
- 사용자 의견, 수상 내역 또는 증명 자료는 가능하면 끝 부분에 포함합니다.
- 줄 바꿈 및 글머리 기호를 사용하여 가독성을 향상시킵니다.
- 앱 설명이 앱 스토어에서 장치 유형별로 표시되는 방식을 파악하여 설명에서 가장 중요한 문장을 쉽게 볼 수 있도록 합니다.

### <a name="whats-new"></a>새로운 기능

새 버전의 응용 프로그램을 업로드할 때 **이 버전의 새로운 기능** 필드는 철저하고 신중하게 완성되어야 사용할 수 있습니다.

새로운 기능 정보를 입력할 때 Apple에서 제안하는 지침은 다음과 같습니다.

- 사용자가 업데이트하도록 권장하는 메시징을 추가합니다.
- 항목을 중요도 순으로 나열하고 변경 내용과 버그 수정을 정확히 지적합니다.
- 변경 내용은 기술적인 전문 용어보다는 인증된 일반 언어로 나타냅니다.

### <a name="keywords"></a>키워드

응용 프로그램의 기능과 관련된 신중하고 전략적인 키워드는 사용자가 앱 스토어에서 검색할 때 응용 프로그램을 쉽게 찾을 수 있도록 합니다. 또한 응용 프로그램에서 iAd 광고를 제공하는 경우 iAd App Network는 앱에서 대상이 되는 광고를 선택할 때 키워드를 사용합니다.

키워드를 선택할 때 Apple에서 권장하는 제안 사항은 다음과 같습니다.

- 경쟁하는 앱 이름, 회사 또는 제품 이름 또는 상표 이름은 사용하지 않습니다.
- 일반적인 단어 또는 용어는 사용하지 않습니다.
- 부적절하거나 불쾌한 용어 또는 유명 인사의 이름과 같이 관련이 없는 단어는 사용하지 않습니다.
- 적절한 경우 해외 시장에 대한 키워드를 지역화합니다.

### <a name="urls"></a>URL

Apple에서는 개발자가 자신의 웹 사이트에 대한 링크를 제공하여 사용자가 응용 프로그램에 대해 가질 수 있는 문제 또는 질문을 지원하도록 요구합니다. 또한 응용 프로그램의 개인 정보 취급 방침에 대한 링크도 요구합니다(웹 사이트에 다시 호스팅되어야 함).

필요에 따라 웹 사이트에 호스팅되는 마케팅 정보에 대한 링크를 제공하여 앱 스토어에서 제공하는 것보다 더 많은 정보를 제공하는 데 사용할 수 있습니다.

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>iTunes Connect에서 이름, 설명, 새로운 기능, 키워드 및 URL 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **버전** 탭을 선택합니다.
4. **이름** 섹션으로 스크롤합니다.
5. 필요한 모든 정보를 입력합니다.

    [![](itunesconnect-images/name01.png "iTunes Connect에서 이름, 설명, 새로운 기능, 키워드 및 URL 편집")](itunesconnect-images/name01.png#lightbox)
6. 화면 위쪽의 **저장** 단추를 클릭하여 변경 내용을 저장합니다.

> [!IMPORTANT]
> 참고: 이름, 설명, 새로운 기능, 키워드 또는 URL이 응용 프로그램의 현재 기능과 일치하지 않는 경우 Apple에서 제출을 거부합니다.

<a name="general" />

## <a name="maintaining-general-app-information"></a>일반 앱 정보 관리

iTunes Connect 응용 프로그램 레코드의 이 섹션에서는 응용 프로그램의 고유 ID(Apple에서 할당한 대로), 응용 프로그램이 속한 범주, 등급, 저작권 및 응용 프로그램을 릴리스하는 회사에 대한 정보를 제공합니다.

### <a name="app-icon"></a>앱 아이콘

> [!IMPORTANT]
>  앱 아이콘은 더 이상 iTunes Connect를 통해 제출되지 않습니다. 프로젝트의 **Assets.xcassets** 파일에 있는 **AppIcon** 이미지 집합을 통해 제출해야 합니다. 자세한 내용은 [앱 스토어 아이콘](~/ios/app-fundamentals/images-icons/app-store-icon.md) 가이드를 참조하세요.

앱 아이콘은 사용자에게 보여 주는 응용 프로그램의 얼굴이므로 기억하기 쉽고 작은 크기로 잘 표시되어야 합니다. 기억하기 쉬운 아이콘은 명확하고 간단하며 즉시 인식할 수 있습니다.

응용 프로그램 아이콘을 디자인할 때 Apple에서 제안하는 지침은 다음과 같습니다.

- 응용 프로그램에 적합한 아이콘을 만듭니다.
- 응용 프로그램의 디자인과 일치하는 간단한 아이콘을 만듭니다.
- 단어는 아이콘에 사용하지 않습니다.
- 전 세계적으로 생각: 단일 앱 아이콘이 모든 스토어 지역에서 사용됩니다.

앱 스토어에 표시할 앱 아이콘에는 1024x1024 픽셀 이미지가 필요합니다.

자세한 내용은 Apple의 [iOS 휴먼 인터페이스 지침](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556) 및 [일반 앱 정보](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7) 설명서의 큰 앱 아이콘 섹션에 대한 설명을 참조하세요.

### <a name="app-id"></a>앱 ID

이 정보는 iTunes Connect 레코드를 만들 때 Apple에서 응용 프로그램에 할당한 고유 ID입니다. 이 번호는 웹 사이트의 앱 스토어 정보를 포함하여 Apple에서 제공하는 여러 웹 기반 인터페이스를 호출할 때 사용할 수 있습니다.

### <a name="version-number"></a>버전 번호

앱 스토어에서 사용자에게 표시되는 응용 프로그램의 현재 활성 버전입니다.

### <a name="category-and-sub-category"></a>범주 및 하위 범주

응용 프로그램에 대한 검색 기능 중 중요한 측면은 앱 스토어에 표시되는 범주입니다. 범주를 사용하면 사용자가 앱 컬렉션을 탐색하고 관심 있는 앱을 찾을 수 있습니다. iTunes Connect에서는 응용 프로그램을 정의할 때 최대 두 개의 다른 범주를 지정할 수 있습니다. 응용 프로그램의 주요 기능을 가장 잘 설명하는 범주를 신중하게 선택해야 합니다.

### <a name="ratings"></a>등급

앱 스토어에서 모든 응용 프로그램에는 등급이 있어야 합니다. 이 등급은 자녀를 보호하도록 부모에게 알리고, 응용 프로그램의 유형 및 내용에 따라 자식에 대한 액세스를 제한하는 데 사용됩니다. 응용 프로그램을 정의할 때 iTunes Connect에서 응용 프로그램에 내용이 표시되는 빈도를 식별하는 내용 설명 목록을 제공합니다. 이러한 선택 항목은 앱 스토어에 표시되는 등급으로 변환됩니다.

자녀를 위한 응용 프로그램을 만들 때 앱 스토어에는 11세 이하 자녀에 대한 특수 범주가 있습니다. 응용 프로그램이 특별히 어린이를 대상으로 하지 않는 경우에도 적절한 콘텐츠 등급을 제공하여 고객이 올바르게 선택할 수 있도록 합니다.

> [!IMPORTANT]
> 참고: 음란하거나 외설적이거나 공격적이거나 명예를 훼손한다고 판단되는 응용 프로그램인 경우 Apple에서 제출을 거부합니다.

### <a name="copyright-and-company-information"></a>저작권 및 회사 정보

Apple에서는 응용 프로그램에 대한 저작권 정보를 제공하도록 허용하며, 주소 및 연락처 정보(한국 앱 스토어에 릴리스되는 응용 프로그램에 필요함)와 같이 응용 프로그램을 릴리스하는 회사에 대한 정보를 요구합니다. 이 정보는 필요에 따라 앱 스토어에 표시됩니다.

### <a name="editing-general-app-information-in-itunes-connect"></a>iTunes Connect에서 일반 앱 정보 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **버전** 탭을 선택합니다.
4. **일반 앱 정보** 섹션으로 스크롤합니다.
5. 필요한 모든 정보를 입력합니다.

    [![](itunesconnect-images/general01.png "iTunes Connect에서 일반 앱 정보 편집")](itunesconnect-images/general01.png#lightbox)
6. **등급**에서 **편집** 단추를 클릭하여 등급 정보를 설정합니다.

    [![](itunesconnect-images/general02.png "등급 편집")](itunesconnect-images/general02.png#lightbox)
6. 화면 위쪽의 **저장** 단추를 클릭하여 변경 내용을 저장합니다.

> [!NOTE]
> 참고: 범주 또는 등급이 응용 프로그램의 현재 기능과 일치하지 않는 경우 Apple에서 제출을 거부합니다.

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Game Center 정보 유지 관리

Apple의 Game Center를 지원하는 iOS 게임 응용 프로그램의 경우 사용자가 사용할 수 있는 순위표 및 인게임 성적과 같은 정보를 제공할 수 있으며, 응용 프로그램이 네트워크 연결을 통해 멀티 플레이어와 호환될 수 있는 경우에도 마찬가지입니다.

### <a name="editing-game-center-information-in-itunes-connect"></a>iTunes Connect에서 Game Center 정보 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **버전** 탭을 선택합니다.
4. **Game Center** 섹션으로 스크롤합니다.
5. **Game Center** 섹션에서 스위치를 **켜기** 위치로 대칭 이동합니다.
5. 필요한 모든 정보를 입력합니다.

    [![](itunesconnect-images/gamecenter01.png "iTunes Connect에서 Game Center 정보 편집")](itunesconnect-images/gamecenter01.png#lightbox)
6. 화면 위쪽의 **저장** 단추를 클릭하여 변경 내용을 저장합니다.

**Game Center** 탭을 사용하여 Game Center를 활성화하고, 이 응용 프로그램에 제공되는 **순위표** 또는 **성적**을 유지 관리합니다.

[![](itunesconnect-images/gamecenter02.png "Game Center 활성화")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "이 응용 프로그램에 제공되는 순위표 또는 성적 유지 관리")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>앱 검토 정보 유지 관리

이 섹션을 사용하여 연락처 정보(기술 관련 질문이 있는 경우), 필요한 데모 계정 및 테스터에서 앱을 성공적으로 검토하는 데 도움이 될 수 있는 메모와 같이 응용 프로그램을 검토할 Apple 직원에게 필요한 정보를 제공합니다.

### <a name="editing-app-review-information-in-itunes-connect"></a>iTunes Connect에서 앱 검토 정보 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **버전** 탭을 선택합니다.
4. **앱 검토 정보** 섹션으로 스크롤합니다.
5. 필요한 모든 정보를 입력합니다.

    [![](itunesconnect-images/review01.png "iTunes Connect에서 앱 검토 정보 편집")](itunesconnect-images/review01.png#lightbox)
6. 응용 프로그램을 성공적으로 검토한 후 앱 스토어에 릴리스하려는 방법을 선택합니다.

    [![](itunesconnect-images/review02.png "iTunes Connect에서 릴리즈 정보 편집")](itunesconnect-images/review02.png#lightbox)
6. 화면 위쪽의 **저장** 단추를 클릭하여 변경 내용을 저장합니다.


## <a name="maintaining-pricing-information"></a>가격 정보 유지 관리

판매용 응용 프로그램을 릴리스하려는 경우, 사용 가능한 Apple 가격 책정 계층 중 하나를 선택하고 지정된 가격이 적용되는 날짜를 선택하여 판매 가격을 설정해야 합니다. 예를 들어 이 문서의 작성 시점에서 , **계층 1** 가격은 다음과 같습니다.

[![](itunesconnect-images/price01.png "가격 정보 유지 관리")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>교육 할인

한 번에 여러 복사본을 구매할 때 교육 기관에 할인된 가격으로 응용 프로그램을 제공하려면 이 확인란 선택합니다. 할인에 대한 세부 정보는 최신 **유료 응용 프로그램 계약**에 있으며, 교육 고객이 해당 응용 프로그램을 사용할 수 있게 되기 전에 이 계약에 서명해야 합니다.

### <a name="custom-business-to-business-application"></a>사용자 지정 기업 간 응용 프로그램

**사용자 지정 기업 간 응용 프로그램**으로 설정되는 응용 프로그램은 iTunes Connect에서 지정한 **Volume Purchase Program** 고객에게만 제공되며, 해당 지역에서만 사용할 수 있습니다. 예를 들어 미국 Volume Purchase Program 고객은 미국 비즈니스용 앱 스토어 Volume Purchase Program을 사용해야 합니다.

사용자 지정 기업 간 응용 프로그램은 교육 기관 또는 일반 앱 스토어 고객에게 제공되지 않습니다. *비즈니스용 앱 스토어 Volume Purchase Program*에 대해 자세히 알아보려면 Apple의 [FAQ(질문과 대답)](http://vpp.itunes.apple.com/faq) 페이지를 참조하세요. 고객이 **Volume Purchase Program**에 등록하는 방법에 대해 자세히 알아보려면 Apple의 [배포 프로그램](http://enroll.vpp.itunes.apple.com) 페이지를 참조하세요.

### <a name="editing-pricing-information-in-itunes-connect"></a>iTunes Connect에서 가격 정보 편집

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)에서 다음을 수행합니다.

1. **My Apps**를 클릭합니다.
2. 응용 프로그램의 **아이콘**을 클릭합니다.
3. **가격 책정** 탭을 선택합니다.

    [![](itunesconnect-images/price02.png "iTunes Connect에서 가격 정보 편집")](itunesconnect-images/price02.png#lightbox)
4. **가용성 날짜**를 선택합니다.
5. **가격 계층** 드롭다운 목록에서 원하는 가격을 선택합니다.
5. 필요에 따라 **교육 할인**을 사용하도록 설정합니다.
6. 필요에 따라 응용 프로그램을 **사용자 지정 기업 간 응용 프로그램**으로 정의합니다.
6. **저장** 단추를 클릭하여 변경 내용을 저장합니다.

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>인앱 구매 정보 유지 관리

응용 프로그램의 가상 인앱 제품(예: 새 게임 수준 또는 응용 프로그램 기능)을 판매하려는 경우 이 섹션을 사용하여 해당 구매 항목을 만들고 유지 관리합니다.

[![](itunesconnect-images/inapp01.png "인앱 구매 정보 유지 관리")](itunesconnect-images/inapp01.png#lightbox)

Xamarin.iOS 응용 프로그램에서 인앱 구매를 수행하는 작업에 대한 자세한 내용은 [인앱 구매](~/ios/platform/in-app-purchasing/index.md) 설명서를 참조하세요.

## <a name="viewing-application-reviews"></a>응용 프로그램 검토 보기

응용 프로그램이 앱 스토어에 릴리스되면, 응용 프로그램을 무료로 구입하거나 다운로드한 사용자는 응용 프로그램에 대한 검토를 작성하고 별 등급을 남길 수 있습니다. 이 섹션을 사용하여 해당 검토를 확인합니다. 예:

[![](itunesconnect-images/reviews01.png "응용 프로그램 검토 보기")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 iTunes Connect를 사용하여 앱 스토어에 릴리스하기 위해 Xamarin.iOS 응용 프로그램을 준비하는 방법 및 스토어에서 응용 프로그램에 대해 표시되는 모든 정보를 유지 관리하는 방법을 설명합니다.

## <a name="related-links"></a>관련 링크

- [이미지 작업](~/ios/app-fundamentals/images-icons/index.md)
- [iOS 앱 개발 워크플로 가이드: 응용 프로그램 배포](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [앱 스토어 제출 팁](https://developer.apple.com/appstore/resources/submission/tips.html)
- [앱 스토어 검토 지침](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [iTunes Connect 개발자 가이드](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
