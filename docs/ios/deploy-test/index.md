---
title: Xamarin.iOS 앱 배포 및 테스트
description: 이 문서는 Xamarin.iOS 애플리케이션 배포 및 테스트와 관련된 항목을 설명하는 다양한 설명서로 연결합니다. 예를 들어 앱 배포, .ipa 파일, 프로비전, 무선 배포, TestFlight 및 디버깅입니다.
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: f3f5e27e97b7b62ade66ea2dc50a79ac03d51f90
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026462"
---
# <a name="deploying-and-testing-xamarinios-apps"></a>Xamarin.iOS 앱 배포 및 테스트

이 섹션에서는 애플리케이션을 배포하는 방법과 테스트에 사용되는 토픽에 대해 설명합니다. 이 토픽에는 디버깅에 사용되는 도구, 테스터에게 배포하는 방법 및 App Store에 애플리케이션을 게시하는 방법이 포함됩니다.

## <a name="app-distribution"></a>[앱 배포](~/ios/deploy-test/app-distribution/index.md)

이 아티클에서는 Xamarin.iOS 애플리케이션을 구성, 빌드 및 게시하여 다양한 수단으로 배포하는 방법을 보여줍니다.

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [사내(Enterprise) 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

## <a name="ipa-deployment"></a>[IPA 배포](~/ios/deploy-test/app-distribution/ipa-support.md)

Ad-Hoc 및 Enterprise 배포를 통해 개발자는 테스트용 또는 내부 회사 사용자용으로 배포할 수 있는 패키지를 만들 수 있습니다. 이 문서에서는 iTunes를 사용하여 iOS 디바이스에 동기화할 수 있는 IPA를 만드는 방법을 설명합니다.

## <a name="provisioning"></a>[프로비전](provisioning/index.md)

이 가이드에서는 속성 목록 작업과 같은 코드 서명 및 프로비전 기본 사항과 애플리케이션 서비스에 대한 앱을 프로비전하는 방법을 다룹니다. 

## <a name="wireless-deployment"></a>[무선 배포](wireless-deployment.md)

 Xcode 9은 앱을 배포하고 디버그할 때마다 디바이스를 유선으로 연결하지 않고 네트워크를 통해 iOS 디바이스 또는 Apple TV에 배포하는 옵션을 도입했습니다. 이 기능은 현재 미리 보기입니다.

## <a name="testflight"></a>[TestFlight](~/ios/deploy-test/testflight.md)

TestFlight는 현재 Apple에서 소유하고 있으며, Xamarin.iOS 앱을 베타 테스트하는 기본 방법입니다. 이 아티클에서는 앱 업로드부터 iTunes Connect 사용에 이르기까지 TestFlight 프로세스의 모든 단계를 안내합니다.

## <a name="debugging-in-xamarinios"></a>[Xamarin.iOS에서 디버깅](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Mac IDE용 Visual Studio 및 Visual Studio는 iOS 시뮬레이터 및 iOS 디바이스 모두에서 Xamarin.iOS 애플리케이션 디버깅을 지원합니다. 이 아티클에서는 디버거를 사용하는 방법과 지원되는 다양한 옵션을 구성하는 방법을 보여줍니다.

## <a name="touchunit"></a>[Touch.Unit](~/ios/deploy-test/touch.unit.md)

이 문서에서는 Xamarin.iOS 프로젝트의 단위 테스트를 만드는 방법을 설명합니다.
Xamarin.iOS의 단위 테스트는 iOS Test Runner와 단위 테스트 작성을 위한 친숙한 API 집합을 제공하는 [NUnitLite](http://www.nunitlite.com/) 프레임워크의 수정된 버전을 모두 포함하는 Touch.Unit 프레임워크를 사용하여 수행합니다.

## <a name="using-instruments-to-detect-native-leaks-using-markheap"></a>[MarkHeap을 사용하여 네이티브 누수를 검색하는 기기 사용](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

이 아티클에서는 모든 iOS 디바이스 및 모든 Xamarin.iOS 애플리케이션에 기기를 사용하는 방법을 설명합니다. 또한 시뮬레이터에서 애플리케이션을 프로파일링하는 방법을 설명합니다.

## <a name="walkthrough---using-apples-instrument-tool"></a>[연습 - Apple의 계측 도구 사용](~/ios/deploy-test/walkthrough-apples-instrument.md)

이 아티클에서는 Apple의 계측 도구를 사용하여 Xamarin으로 빌드된 iOS 애플리케이션의 메모리 문제를 진단하는 방법을 설명합니다. 계측을 시작하고, 힙 스냅샷을 만들고, 메모리 증가를 분석하는 방법을 보여 줍니다. 또한 계측을 사용하여 메모리 문제가 발생하는 정확한 코드 줄을 표시하여 찾아내는 방법을 보여 줍니다.

## <a name="linking-on-ios"></a>[iOS에서 연결](linker.md)

애플리케이션 패키지의 크기를 최대한 줄일 수 있도록 링커의 작동 원리와 설정 및 사용량을 수정하는 방법을 설명합니다.

## <a name="xamarinios-performance"></a>[Xamarin.iOS 성능](performance.md)

Xamarin.iOS로 빌드된 애플리케이션의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 애플리케이션에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.

## <a name="mtouch"></a>[mtouch](mtouch.md)

프로젝트를 iOS에서 사용할 수 있는 애플리케이션으로 빌드하는 명령줄 도구인 mtouch.exe에 대한 메모와 정보입니다.

## <a name="ios-build-mechanics"></a>[iOS 빌드 메커니즘](ios-build-mechanics.md)

이 가이드에서는 앱의 시간을 맞추는 방법 및 빠른 빌드를 위해 모든 빌드 구성에 사용 가능한 메서드를 사용하는 방법을 설명합니다.
