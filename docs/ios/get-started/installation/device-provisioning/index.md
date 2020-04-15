---
title: Xamarin.iOS에 대한 디바이스 프로비저닝
description: 이 문서에서는 애플리케이션 테스트에 사용할 수 있도록 디바이스를 프로비전하는 방법을 설명합니다. 또한 푸시 알림과 같은 기능을 사용할 수 있도록 앱을 구성하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CACA5236-3C90-F6DF-FD4E-0797B61670CE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/06/2018
ms.openlocfilehash: bb1ef1e948c796e6b0d91230be3c82c5a8c7d366
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "79303740"
---
# <a name="device-provisioning-for-xamarinios"></a>Xamarin.iOS에 대한 디바이스 프로비저닝

Xamarin.iOS 애플리케이션을 개발하는 동안 시뮬레이터 뿐만 아니라 물리적 디바이스에 앱을 배포하여 테스트하는 것은 필수적입니다. 디바이스 전용 버그 및 성능 문제는 메모리 또는 네트워크 연결과 같은 하드웨어 용량 제한으로 인해 디바이스에서 실행할 때 발생할 수 있습니다. 물리적 디바이스에서 테스트하려면 디바이스는 *프로비전*되어야 하며 디바이스가 테스트용으로 사용되는 것을 Apple에 알려야 합니다.

아래 그림에 강조 표시된 섹션은 iOS 프로비전을 설정하는 데 필요한 단계를 보여 줍니다.

[![](images/provisioningdiagram.png "The highlighted sections in this image show the steps required to get set up for iOS provisioning")](images/provisioningdiagram.png#lightbox)

이 후 다음 단계는 애플리케이션을 배포하는 것입니다. 배포에 대한 자세한 내용은 [앱 배포](~/ios/deploy-test/app-distribution/index.md) 가이드를 참조하세요.

디바이스에 애플리케이션을 배포하기 전에 Apple의 개발자 프로그램에 대한 활성 구독이 *있거나*[무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)을 사용해야 합니다. Apple에서는 두 개의 프로그램 옵션을 제공합니다.

- **Apple 개발자 프로그램** – 개인이거나 조직을 대표하는지 여부에 관계 없이 Apple 개발자 프로그램을 통해 앱을 개발하고, 테스트하고, 배포할 수 있습니다.
- **Apple 개발자 엔터프라이즈 프로그램** – 엔터프라이즈 프로그램은 사내에서만 앱을 개발하고 배포하려는 조직에 가장 적합합니다. 엔터프라이즈 프로그램의 구성원은 iTunes Connect에 액세스할 수 없고 만든 앱을 앱 스토어에 게시할 수 없습니다.

이러한 프로그램 중 하나를 등록하려면 [Apple 개발자 포털](https://developer.apple.com/programs/enroll/)을 방문하여 등록합니다. Apple 개발자로서 등록하려면 [Apple ID](https://appleid.apple.com/)가 필요합니다. 이 가이드는 사용자가 Apple 개발자 프로그램의 구성원**이라는** 가정으로 만들어졌습니다.

또는 Apple은 Apple의 개발자 프로그램의 구성원일 필요 *없이* 단일 애플리케이션을 단일 디바이스에서 실행할 수 있도록 하는 Xcode 7에서 [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)을 도입했습니다. [여기](~/ios/get-started/installation/device-provisioning/free-provisioning.md#limitations)에서 설명된 대로 이러한 방식으로 프로비전할 때 여러 제한 사항이 있습니다.

디바이스에서 실행되는 모든 애플리케이션은 애플리케이션 및 개발자에 대한 정보를 포함하는 메타데이터(또는 *지문*)의 집합을 포함해야 합니다. Apple은 이 지문을 사용하여 사용자의 디바이스에 배포하거나 사용자의 디바이스에서 실행될 때 애플리케이션이 손상되지 않도록 합니다. 이는 앱 개발자에게 개발자로서 해당 Apple ID를 등록하고, 앱 ID를 설정하고, 인증서를 요청하고, 애플리케이션이 배포될 디바이스를 등록하도록 하여 수행됩니다.

디바이스에 애플리케이션을 배포할 때 프로비전 프로필도 iOS 디바이스에 설치됩니다. 프로비전 프로필은 앱이 빌드 시 서명되었고 Apple에서 암호로 서명했다는 정보를 확인하기 위해 존재합니다. 프로비전 프로필 및 '지문' 검사는 함께 다음을 확인하여 애플리케이션을 디바이스에 배포할 수 있는지 여부를 결정합니다.

- **누구**(인증서 – 프로비전 프로필에 해당 퍼블릭 키가 있는 프라이빗 키로 앱이 서명되었나요? 인증서는 또한 개발자를 개발 팀과 연결합니다.)
- **무엇**(개별 앱 ID – Info.plist의 번들 식별자 설정은 프로비저닝 프로필의 앱 ID와 일치하나요?)
- **어디**(디바이스 - 디바이스는 프로비전 프로필에 포함되어 있나요?)

이러한 단계는 애플리케이션 및 디바이스를 포함하여 개발 프로세스 중에 만들어지거나 사용된 모든 항목이 Apple 개발자 계정에 대해 역추적될 수 있도록 합니다.

## <a name="provisioning-your-device"></a>디바이스 프로비저닝

iOS 디바이스를 프로비전하는 두 가지 방법이 있습니다.

- **자동(권장)** – 프로젝트에서 **자동 프로비저닝** 구성표를 선택하여 Visual Studio에서 서명 ID, 앱 ID 및 프로비전 프로필을 자동으로 만들고 관리하도록 합니다. 프로비전을 자동으로 관리하는 방법에 대한 자세한 내용은 [자동 프로비전](automatic-provisioning.md) 가이드를 참조하세요. iOS 디바이스를 프로비전하는 권장 방법입니다.

- **수동** - 서명 ID, 앱 ID 및 프로비전 프로필은 [수동 프로비전](manual-provisioning.md) 가이드에 설명된 대로 Apple 개발자 포털을 통해 만들어지고 관리될 수 있습니다. 그런 다음, 이러한 아티팩트는 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 가이드에 설명된 대로 관리될 수 있습니다.

## <a name="provisioning-for-application-services"></a>애플리케이션 서비스 프로비전

Apple은 Xamarin.iOS 애플리케이션에 활성화할 수 있는 다양한 애플리케이션 서비스(다른 이름: 기능)를 제공합니다. 이러한 애플리케이션 서비스는 **앱 ID**를 만들 때 iOS 프로비전 포털에서 구성하고 Xamarin.iOS 애플리케이션 프로젝트에 속하는 **Entitlements.plist** 파일에서도 구성해야 합니다. 앱에 애플리케이션 서비스를 추가하는 방법에 대한 자세한 내용은 [기능 소개](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드 및 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.

- 필요한 앱 서비스를 포함하는 앱 ID를 만듭니다.
- 앱 ID를 포함하는 새로운 [프로비전 프로필](#provisioning-your-device)을 만듭니다.
- Xamarin.iOS 프로젝트에서 자격 설정

## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
