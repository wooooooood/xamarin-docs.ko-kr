---
title: Xamarin.iOS 앱 배포 개요
description: 이 문서는 Xamarin.iOS 애플리케이션에 사용할 수 있는 배포 기술에 대한 개요를 제공하고, 해당 항목에 대한 자세한 문서를 가리키는 포인터 역할을 합니다.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 20126849027f735e9ecd3599c290b4e7a57f837e
ms.sourcegitcommit: ec62e2624295aa502ec35ac782031d61d61c3aaa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75886582"
---
# <a name="xamarinios-app-distribution-overview"></a>Xamarin.iOS 앱 배포 개요

_이 문서는 Xamarin.iOS 애플리케이션에 사용할 수 있는 배포 기술에 대한 개요를 제공하고, 해당 항목에 대한 자세한 문서를 가리키는 포인터 역할을 합니다._

Xamarin.iOS 앱이 개발되면 소프트웨어 개발 수명 주기의 다음 단계는 아래 다이어그램에서 강조 표시된 부분과 같이 사용자에게 앱을 배포하는 것입니다.

[![iOS 앱을 개발한 후 사용자에게 해당 앱을 배포(다이어그램에서 강조 표시된 섹션)](images/publishingdiagram.png)](images/publishingdiagram.png#lightbox)

Apple은 다음과 같은 iOS 애플리케이션 배포 방법을 제공합니다.

- [**App Store**](#app-store-distribution)
- [**사내(엔터프라이즈)** ](#in-house-distribution)
- [**임시**](#ad-hoc-distribution)
- [**비즈니스용 사용자 지정 앱**](#custom-apps-for-business)

이러한 모든 시나리오에서는 적절한 *프로비전 프로필*을 사용하여 애플리케이션을 프로비전해야 합니다. 프로비전 프로필은 애플리케이션 ID 및 의도된 배포 메커니즘뿐만 아니라 코드 서명 정보도 포함된 파일입니다. 앱 스토어 배포가 아닌 경우 앱을 배포할 수 있는 디바이스에 대한 정보도 포함되어 있습니다.

## <a name="app-store-distribution"></a>App Store 배포

> [!IMPORTANT]
> Apple은 2019년 3월부터 App Store에 제출된 모든 앱과 업데이트가 iOS 12.1 SDK 이상에서 빌드되어 Xcode 10.1 이상에 포함된다고 [발표했습니다](https://developer.apple.com/ios/submit/).
> 앱은 iPhone XS 및 12.9인치 iPad Pro 화면 크기도 지원해야 합니다.

iOS 애플리케이션이 iOS 디바이스의 소비자에게 배포되는 기본 방법입니다. 앱 스토어에 제출된 모든 앱에는 Apple의 승인이 필요합니다.

앱은 *iTunes Connect*라는 포털을 통해 앱 스토어에 제출됩니다. [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드는 이 포털을 설정하고 사용하여 앱 스토어에 게시하기 위해 Xamarin.iOS 앱을 준비하는 방법에 대해 자세히 설명합니다.

**Apple Developer Program**에 속한 개발자만 iTunes Connect에 액세스할 수 있습니다. **Apple Developer Enterprise Program**의 구성원은 액세스할 수 없습니다.

자세한 내용은 [앱 스토어 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) 가이드를 참조하세요.

## <a name="in-house-distribution"></a>사내 배포

*엔터프라이즈 배포*라고도 하는 사내 배포를 사용하면 **Apple Developer Enterprise Program**의 구성원이 내부적으로 동일한 조직의 다른 구성원에게 앱을 배포할 수 있습니다. 사내 배포에는 앱 스토어 검토가 필요하지 않으며, 애플리케이션을 설치할 수 있는 디바이스의 수가 제한되지 않는다는 이점이 있습니다. 그러나 **Apple Developer Enterprise Program** 구성원에게는 iTunes Connect에 대한 액세스 권한이 **없으므로** 정식 사용자가 앱을 배포해야 합니다.

사내 애플리케이션을 설정하고 배포하는 방법에 대한 자세한 내용은 [사내 배포 가이드](~/ios/deploy-test/app-distribution/in-house-distribution.md)를 참조하세요.

## <a name="ad-hoc-distribution"></a>임시 배포

Xamarin.iOS 애플리케이션은 **Apple Developer Program** 및 **Apple Developer Enterprise Program** 모두에서 사용할 수 있는 임시 배포를 통해 사용자가 테스트할 수 있으며, 최대 100개의 iOS 디바이스를 테스트하도록 허용합니다. iTunes Connect가 옵션이 아닌 경우 임시 배포에 대한 모범 사례는 회사 내 배포입니다.

사내 애플리케이션을 설정하고 배포하는 방법에 대한 자세한 내용은 [임시 배포 가이드](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)를 참조하세요.

## <a name="custom-apps-for-business"></a>비즈니스용 사용자 지정 앱

Apple에서는 기업 및 교육 기관을 대상으로 앱의 [사용자 지정 배포](https://developer.apple.com/business/custom-apps/)를 허용합니다. [Apple Business Manager 사용자 가이드](https://support.apple.com/guide/apple-business-manager/welcome/web)를 검토하여 자세한 내용을 확인하세요.

## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
