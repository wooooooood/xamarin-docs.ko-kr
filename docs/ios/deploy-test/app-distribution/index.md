---
title: Xamarin.iOS 앱 배포 개요
description: 이 문서는 Xamarin.iOS 애플리케이션에 사용할 수 있는 배포 기술에 대한 개요를 제공하고, 해당 항목에 대한 자세한 문서를 가리키는 포인터 역할을 합니다.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: c7b31e05a5b9d75559fae14f467312f5abc8fe94
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289802"
---
# <a name="xamarinios-app-distribution-overview"></a>Xamarin.iOS 앱 배포 개요

_이 문서는 Xamarin.iOS 애플리케이션에 사용할 수 있는 배포 기술에 대한 개요를 제공하고, 해당 항목에 대한 자세한 문서를 가리키는 포인터 역할을 합니다._

Xamarin.iOS 앱이 개발되면 소프트웨어 개발 수명 주기의 다음 단계는 아래 다이어그램에서 강조 표시된 부분과 같이 사용자에게 앱을 배포하는 것입니다.


[![](images/publishingdiagram.png "iOS 앱을 개발한 후 사용자에게 해당 앱을 배포(다이어그램에서 강조 표시된 섹션)")](images/publishingdiagram.png#lightbox)


Xamarin.iOS에서 지원하는 iOS 애플리케이션을 배포하기 위해 Apple에서 제공하는 방법은 다음과 같습니다.

1. [**앱 스토어**](#App_Store_Distribution)
2. [**사내(엔터프라이즈)** ](#In-House_Distribution)
3. [**임시**](#Ad_Hoc_Distribution)

이러한 모든 시나리오에서는 적절한 *프로비전 프로필*을 사용하여 애플리케이션을 프로비전해야 합니다. 프로비전 프로필은 애플리케이션 ID 및 의도된 배포 메커니즘뿐만 아니라 코드 서명 정보도 포함된 파일입니다. 앱 스토어 배포가 아닌 경우 앱을 배포할 수 있는 디바이스에 대한 정보도 포함되어 있습니다.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>앱 스토어 배포

> [!IMPORTANT]
> Apple은 2019년 3월부터 App Store에 제출된 모든 앱과 업데이트가 iOS 12.1 SDK 이상에서 빌드되어 Xcode 10.1 이상에 포함된다고 [발표했습니다](https://developer.apple.com/ios/submit/).
> 앱은 iPhone XS 및 12.9인치 iPad Pro 화면 크기도 지원해야 합니다.

iOS 애플리케이션이 iOS 디바이스의 소비자에게 배포되는 기본 방법입니다. 앱 스토어에 제출된 모든 앱에는 Apple의 승인이 필요합니다.

앱은 *iTunes Connect*라는 포털을 통해 앱 스토어에 제출됩니다. [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) 가이드는 이 포털을 설정하고 사용하여 앱 스토어에 게시하기 위해 Xamarin.iOS 앱을 준비하는 방법에 대해 자세히 설명합니다.

**Apple Developer Program**에 속한 개발자만 iTunes Connect에 액세스할 수 있습니다. **Apple Developer Enterprise Program**의 구성원은 액세스할 수 없습니다.

자세한 내용은 [앱 스토어 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md) 가이드를 참조하세요.

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>사내 배포

*엔터프라이즈 배포*라고도 하는 사내 배포를 사용하면 **Apple Developer Enterprise Program**의 구성원이 내부적으로 동일한 조직의 다른 구성원에게 앱을 배포할 수 있습니다. 사내 배포에는 앱 스토어 검토가 필요하지 않으며, 애플리케이션을 설치할 수 있는 디바이스의 수가 제한되지 않는다는 이점이 있습니다. 그러나 **Apple Developer Enterprise Program** 구성원에게는 iTunes Connect에 대한 액세스 권한이 **없으므로** 정식 사용자가 앱을 배포해야 합니다.

사내 애플리케이션을 설정하고 배포하는 방법에 대한 자세한 내용은 [사내 배포 가이드](~/ios/deploy-test/app-distribution/in-house-distribution.md)를 참조하세요.

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>임시 배포

Xamarin.iOS 애플리케이션은 **Apple Developer Program** 및 **Apple Developer Enterprise Program** 모두에서 사용할 수 있는 임시 배포를 통해 사용자가 테스트할 수 있으며, 최대 100개의 iOS 디바이스를 테스트하도록 허용합니다. iTunes Connect가 옵션이 아닌 경우 임시 배포에 대한 모범 사례는 회사 내 배포입니다.

사내 애플리케이션을 설정하고 배포하는 방법에 대한 자세한 내용은 [임시 배포 가이드](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 애플리케이션에 사용할 수 있는 배포 메커니즘에 대해 간략히 설명했습니다. iTunes 앱 스토어, 임시 및 사내 배포를 소개하고 자세한 정보에 대한 링크를 제공했습니다.

## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
